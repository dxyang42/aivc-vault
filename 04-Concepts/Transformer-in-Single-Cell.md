# Transformer 在单细胞中的应用

## 概述

Transformer 架构最初为自然语言处理设计，近年来被成功应用于单细胞数据分析。核心思想是将基因视为"词汇"，细胞视为"句子"，利用自注意力机制建模基因间的复杂关系。

## 核心概念

### 基因作为 Token

#### 基本思想
在单细胞 Transformer 中，每个基因被表示为一个 token：

```
细胞 = [基因1表达, 基因2表达, ..., 基因N表达]
     = [token_1, token_2, ..., token_N]
```

#### 基因嵌入 (Gene Embedding)
每个基因 g_i 有两个组成部分：
1. **基因身份嵌入** e_i^gene: 标识基因类型
2. **表达值嵌入** e_i^expr: 编码表达水平

```
h_i^(0) = e_i^gene + e_i^expr
```

#### 表达值编码方式

| 方法 | 描述 | 适用场景 |
|------|------|----------|
| **离散分箱** | 将连续值分箱为离散类别 | BERT 类模型 |
| **连续投影** | 线性/MLP 投影 | GPT 类模型 |
| **傅里叶特征** | 正弦/余弦编码 | 需要精细粒度 |
| **对数变换** | log(1+x) 后嵌入 | 处理稀疏数据 |

### 位置编码 (Positional Encoding)

#### 为什么需要位置编码
与自然语言不同，基因没有固有的顺序。位置编码用于：
1. 标识基因身份
2. 引入基因间的先验关系
3. 编码基因功能相似性

#### 位置编码类型

##### 1. 可学习位置编码
```
PE_i = W_pos[i]
```
每个基因学习独立的位置嵌入。

##### 2. 基于功能的位置编码
利用基因本体 (GO) 或通路信息：
```
PE_i = Encode(GO_i, Pathway_i)
```

##### 3. 染色体位置编码
基于基因在染色体上的位置：
```
PE_i = Encode(chrom_i, pos_i)
```

##### 4. 旋转位置编码 (RoPE)
```python
def apply_rotary_pos_emb(q, k, cos, sin):
    # 旋转位置编码
    q_rot = q * cos - rotate_half(q) * sin
    k_rot = k * cos - rotate_half(k) * sin
    return q_rot, k_rot
```

## 架构变体

### 1. Encoder-only (BERT-style)

#### 代表模型
- **scBERT** (Tencent AI Lab)
- **Geneformer** (Christina Theodoris)

#### 架构特点
```
输入: [CLS] Gene1 Gene2 ... GeneN
      ↓
    Embedding
      ↓
    Transformer Encoder × L
      ↓
输出: 细胞表示 ([CLS]) / 基因表示
```

#### 训练目标
- **掩码基因建模 (MGM)**: 随机掩码基因，预测其表达
- **细胞类型分类**: 监督学习细胞标签
- **对比学习**: 拉近同类细胞表示

### 2. Decoder-only (GPT-style)

#### 代表模型
- **scGPT** (Bo Wang Lab)
- **scFoundation** (HGI)

#### 架构特点
```
输入: <bos> Gene1 Gene2 ... GeneN
      ↓
    Causal Masked Self-Attention
      ↓
    Transformer Decoder × L
      ↓
输出: 下一个基因预测 / 细胞生成
```

#### 训练目标
- **自回归建模**: 预测序列中的下一个基因
- **细胞生成**: 生成完整细胞表达谱

### 3. Encoder-Decoder

#### 应用场景
- 细胞-细胞翻译（如正常→疾病）
- 多组学整合（RNA→蛋白质）
- 扰动预测（控制→处理）

## 注意力机制

### 自注意力计算
```
Attention(Q, K, V) = softmax(Q · K^T / sqrt(d_k)) · V
```

在单细胞中：
- Q, K, V 是基因查询、键、值矩阵
- 注意力权重表示基因间关系强度

### 基因调控网络推断
从注意力权重可以推断基因调控关系：

```
A_ij = Attention(gene_i, gene_j)
```

高注意力权重 A_ij 可能表示基因 j 对基因 i 有调控关系。

### 多头注意力
```
MultiHead(H) = Concat(head_1, ..., head_h) · W^O

where: head_i = Attention(H · W_i^Q, H · W_i^K, H · W_i^V)
```

不同注意力头可以捕获不同类型的基因关系（如激活、抑制、共表达等）。

不同头可以捕获不同类型的基因关系。

## 在 AIVC 中的应用

### 1. 细胞编码器
```python
class CellTransformer(nn.Module):
    def __init__(self, n_genes, d_model, n_layers, n_heads):
        super().__init__()
        self.gene_embedding = GeneEmbedding(n_genes, d_model)
        self.transformer = TransformerEncoder(
            d_model, n_layers, n_heads
        )
    
    def forward(self, gene_expression):
        # gene_expression: [batch, n_genes]
        x = self.gene_embedding(gene_expression)
        x = self.transformer(x)
        
        # 聚合为细胞表示
        cell_embedding = x.mean(dim=1)  # 或取 [CLS]
        return cell_embedding
```

### 2. 扰动预测
- 输入：控制细胞 + 扰动信息
- 输出：预测扰动后细胞状态
- 条件化：将扰动作为额外 token

### 3. 细胞生成
- 自回归生成细胞表达谱
- 条件化生成特定细胞类型
- 插值生成中间状态

## 预训练策略

### 大规模预训练
| 数据集 | 细胞数 | 组织 |
|--------|--------|------|
| **CELLxGENE** | 数千万 | 多组织 |
| **Human Cell Atlas** | 数百万 | 全身 |
| **Mouse Cell Atlas** | 数百万 | 小鼠 |

### 预训练任务
1. **掩码基因建模**: 随机掩码 15% 基因
2. **细胞类型预测**: 监督分类
3. **批次校正**: 对抗学习去除批次效应
4. **对比学习**: 细胞表示学习

## 关键论文

1. **Zhou et al. (2022)** "scBERT" - *Nature Machine Intelligence*
   - 首个单细胞 BERT 模型

2. **Cui et al. (2024)** "scGPT" - *Nature Methods*
   - 首个单细胞 GPT 模型

3. **Theodoris et al. (2023)** "Geneformer" - *Nature*
   - 基于迁移学习的基因网络推断

4. **Yang et al. (2022)** "scTransformer" - 早期 Transformer 应用

## 挑战与局限

1. **计算复杂度**: O(N²) 注意力，基因数通常 2万+
2. **稀疏性**: 单细胞数据高度稀疏 (90%+ 零值)
3. **批次效应**: 不同实验批次间的差异
4. **可解释性**: 注意力权重的生物学意义

## 优化方向

| 方向 | 方法 |
|------|------|
| **高效注意力** | Linformer, Performer, Flash Attention |
| **稀疏处理** | 零值特殊处理，稀疏注意力 |
| **层次建模** | 基因→通路→细胞层次 |
| **先验知识** | 整合通路、调控网络 |

---
tags: [method, llm, tongji, 2026, language-model]
year: 2026
institution: "同济大学 DELTA Lab"
authors: "啜国晖, 陈晓涵, 杨兴博, 高溢骋, 汪伟旭, 赵宇恒, 董科竟, 刘琦"
architecture: "Language Model"
paper: "bioRxiv 2026.03"
code: "https://github.com/xcompbio/CellHermes"
status: "已收录"
date: 2026-03-28
---

# CellHermes 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[同济 DELTA Lab]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **方法名称** | CellHermes |
| **发表时间** | 2026年3月 (预印本) |
| **开发团队** | 同济大学数字生命智能体实验室 (DELTA Lab) |
| **核心成员** | 啜国晖, 陈晓涵, 杨兴博, 高溢骋, 汪伟旭, 赵宇恒, 董科竟, 刘琦 |
| **合作机构** | 亥姆霍兹慕尼黑中心 (Fabian J. Theis) |

## 核心技术

### 架构设计
CellHermes是**细胞语言模型**，以**自然语言为桥梁**连接异构组学数据：

1. **自然语言桥梁**
   - 将表格数据（单细胞转录组）转化为"基因表达句子"
   - 将图结构数据（蛋白质互作网络）转化为自然语言陈述

2. **参数高效微调**
   - 基于现有预训练大语言模型
   - 采用**LoRA (Low-Rank Adaptation)**技术
   - 无需从零训练模型骨干

3. **多模态异构数据融合**
   - 统一不同组学描述模态和形式
   - 整合单细胞转录组和蛋白质互作网络

## 数学公式

### 语言模型嵌入

#### 基因表达文本化

CellHermes将单细胞基因表达数据转化为自然语言描述：

$$\text{Text}_{\text{cell}} = \text{Tokenize}\left(\text{GenesToSentence}(\{g_i: x_i > \tau\})\right)$$

其中：
- $g_i$ 为基因名称
- $x_i$ 为基因表达值
- $\tau$ 为表达阈值

基因表达句子示例：
```
"Cell expresses high levels of CD4, IL2, and FOXP3, 
 moderate levels of CD3D, and low levels of CD8A..."
```

#### 蛋白质网络文本化

将PPI网络转化为自然语言陈述：

$$\text{Text}_{\text{PPI}} = \bigcup_{(p_i, p_j) \in \mathcal{E}} \text{Format}(p_i, \text{interacts with}, p_j)$$

其中 $\mathcal{E}$ 为PPI网络边集。

### 大语言模型嵌入

CellHermes使用预训练LLM（如GPT、LLaMA）生成嵌入：

$$e_{\text{cell}} = \text{LLM}_{\text{encoder}}(\text{Text}_{\text{cell}}) \in \mathbb{R}^{d_{\text{LLM}}}$$

其中 $d_{\text{LLM}}$ 通常为768（BERT-base）或4096（LLaMA）。

### LoRA微调

CellHermes采用LoRA进行参数高效微调：

$$W' = W_0 + \Delta W = W_0 + BA$$

其中：
- $W_0 \in \mathbb{R}^{d \times k}$ 为预训练权重（冻结）
- $B \in \mathbb{R}^{d \times r}$ 和 $A \in \mathbb{R}^{r \times k}$ 为可学习的低秩矩阵
- $r \ll \min(d, k)$ 为秩（通常 $r=8$ 或 $16$）

对于Transformer的每一层，LoRA应用于：
- 查询投影：$W_q' = W_q + B_q A_q$
- 键投影：$W_k' = W_k + B_k A_k$
- 值投影：$W_v' = W_v + B_v A_v$

### 多模态融合

#### 跨模态注意力

$$\text{CrossModalAttn}(Q_{\text{scRNA}}, K_{\text{PPI}}, V_{\text{PPI}}) = \text{softmax}\left(\frac{Q_{\text{scRNA}} K_{\text{PPI}}^T}{\sqrt{d_k}}\right) V_{\text{PPI}}$$

#### 融合表示

$$h_{\text{fused}} = \text{LayerNorm}\left(\alpha \cdot h_{\text{scRNA}} + (1-\alpha) \cdot h_{\text{PPI}} + \text{CrossModalAttn}(h_{\text{scRNA}}, h_{\text{PPI}}, h_{\text{PPI}})\right)$$

其中 $\alpha$ 为可学习的模态权重。

### 训练目标

#### 掩码语言建模 (MLM)

$$\mathcal{L}_{\text{MLM}} = -\mathbb{E}_{x \in \mathcal{M}} \left[ \log P(x | \text{Text}_{\setminus \mathcal{M}}) \right]$$

其中 $\mathcal{M}$ 为掩码的词元集合。

#### 跨模态对齐

$$\mathcal{L}_{\text{align}} = \|\text{sim}(e_{\text{scRNA}}, e_{\text{PPI}}) - \text{sim}_{\text{true}}\|^2$$

其中 $\text{sim}(\cdot, \cdot)$ 为余弦相似度。

#### 总损失

$$\mathcal{L}_{\text{total}} = \mathcal{L}_{\text{MLM}} + \lambda_{\text{align}} \mathcal{L}_{\text{align}} + \lambda_{\text{reg}} \|BA\|^2$$

最后一项为LoRA正则化。

## 技术特点

| 特点 | 描述 |
|------|------|
| **自然语言接口** | 以自然语言为交互接口 |
| **多模态融合** | 融合异构组学数据 |
| **参数高效** | LoRA微调，无需从头训练 |
| **统一表示** | 统一不同组学描述模态 |

## 与AlphaCell的关系

CellHermes与**AlphaCell**是同济DELTA Lab同时发布的两个互补方法：

| 方法 | 定位 | 核心技术 | 功能 |
|------|------|---------|------|
| **AlphaCell** | 虚拟细胞世界模型 | 流匹配，全基因组表征 | 预测细胞状态转换 |
| **CellHermes** | 细胞语言模型 | 自然语言桥梁，LoRA微调 | 多模态数据融合，语义理解 |

## 功能特点

- ✅ **自然语言理解**: 理解基因表达的自然语言描述
- ✅ **多模态整合**: 整合转录组和蛋白质网络数据
- ✅ **语义统一**: 统一不同组学数据的语义表示
- ✅ **参数高效**: 基于LoRA的微调，计算成本低

## 应用场景

1. **基因功能预测**
   - 基于自然语言描述预测基因功能
   - 理解基因间的语义关系

2. **细胞类型特异性网络重建**
   - 重建细胞类型特异性的蛋白质互作网络
   - 理解细胞特异的调控机制

3. **多模态数据查询**
   - 使用自然语言查询单细胞数据
   - 跨模态数据检索

## 优势与局限

### 优势
- 自然语言接口，易于使用
- 多模态数据融合能力强
- 参数高效，训练成本低
- 可解释性强

### 局限
- 训练数据多样性相对有限
- 需要进一步优化文本生成的可解释性
- 预印本阶段，待同行评审

## 开源状态

- **预印本**: 已发布 (2026年3月)
- **开源代码**: 即将开源 (请关注相关发布)

## 团队背景

**同济大学数字生命智能体实验室 (DELTA Lab)**
- 专注于数字生命和虚拟细胞研究
- 与Fabian Theis等国际团队合作
- 在单细胞分析和虚拟细胞领域有重要贡献

## 相关论文

- CellHermes预印本 (2026年3月)
- AlphaCell预印本 (2026年3月)

---

*文档创建时间: 2026-03-28*

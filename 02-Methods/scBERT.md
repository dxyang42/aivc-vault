---
tags: [method, foundation-model, bert, single-cell, cell-type-annotation]
year: 2022
institution: "Tencent AI Lab"
authors: "Jiarui Yang, Ce Shang, Xinyuan Wang, Hong Chen, Yixuan Wang, Hongmei Wang, Dongsheng Li"
architecture: "BERT + Performer"
paper: "https://www.nature.com/articles/s41592-022-05770-z"
code: "https://github.com/TencentAILabHealthcare/scBERT"
status: "已收录"
date: 2026-03-29
---

# scBERT

## 基本信息

- **全称**: scBERT as a Large-scale Pretrained Deep Language Model for Cell Type Annotation of Single-cell RNA-seq Data
- **发表时间**: 2022年9月
- **发表期刊**: Nature Methods
- **机构**: 腾讯 AI Lab
- **作者**: Jiarui Yang, Ce Shang, Xinyuan Wang, Hong Chen, Yixuan Wang, Hongmei Wang, Dongsheng Li

## 核心技术

scBERT 是将自然语言处理中的 BERT 范式引入单细胞转录组分析的开创性工作：

1. **BERT 范式迁移**: 借鉴 NLP 中的预训练-微调范式
2. **全基因组输入**: 无需降维或特征选择，直接处理约16000维基因表达
3. **Performer 编码器**: 采用线性复杂度的 Performer 替代标准 Transformer

## 架构细节

### 整体架构

```
┌─────────────────────────────────────────────────────────────┐
│                    scBERT Architecture                      │
├─────────────────────────────────────────────────────────────┤
│  输入层                                                     │
│  ├── 基因表达值 (分箱离散化)                                │
│  │   └── 0-7 共8个区间                                     │
│  └── 基因嵌入 (Gene2Vec)                                    │
├─────────────────────────────────────────────────────────────┤
│  Performer 编码器 (×12)                                     │
│  ├── Fast Attention via Orthogonal Random Features          │
│  ├── Feed-Forward Network                                   │
│  └── LayerNorm + Residual                                   │
├─────────────────────────────────────────────────────────────┤
│  预训练头                                                   │
│  └── 掩码基因表达预测 (MLM)                                 │
├─────────────────────────────────────────────────────────────┤
│  微调头                                                     │
│  └── 细胞类型分类                                           │
└─────────────────────────────────────────────────────────────┘
```

### 输入处理

1. **基因分箱**: 将连续表达值离散化为 8 个区间
   - 0: 未表达
   - 1-7: 表达强度分级

2. **基因嵌入**: 使用 Gene2Vec 预训练嵌入
   - 维度: 200
   - 基于基因共表达模式训练

3. **无位置编码**: 基因没有固有顺序，不使用位置编码

### Performer 编码器

scBERT 采用 Performer 替代标准 Transformer，将复杂度从 $O(n^2)$ 降低到 $O(n)$：

$$\text{Attention}(Q, K, V) = \frac{\phi(Q)\phi(K)^T V}{\phi(Q)\phi(K)^T \mathbf{1}}$$

其中 $\phi(\cdot)$ 为正交随机特征映射：

$$\phi(x) = \frac{h(x)}{\sqrt{m}}\left[\exp(w_1^T x), ..., \exp(w_m^T x)\right]$$

## 预训练任务

### 掩码语言建模 (MLM)

随机掩码 15% 的基因表达值，预测原始值：

$$\mathcal{L}_{MLM} = -\mathbb{E}\left[\sum_{i \in \mathcal{M}} \log P(x_i | x_{\setminus \mathcal{M}})\right]$$

其中 $\mathcal{M}$ 为掩码位置集合。

## 模型配置

| 参数 | 值 |
|-----|-----|
| 编码器层数 | 12 |
| 隐藏维度 | 200 (嵌入) / 768 (编码器) |
| 注意力头数 | 12 |
| FFN 维度 | 3072 |
| 预训练数据 | 1.12M 细胞 |
| 基因维度 | ~16,000 |

## 输入/输出

- **输入**:
  - 单细胞基因表达数据（分箱后）
- **输出**:
  - 768 维细胞嵌入
  - 细胞类型预测（微调后）

## 性能指标

在细胞类型注释任务上表现优异：

| 数据集 | scBERT | 传统方法 | 提升 |
|--------|--------|----------|------|
| Zheng68K | 94.5% | 89.2% | +5.3% |
| MacParland | 96.8% | 92.1% | +4.7% |
| Human Liver | 93.2% | 87.5% | +5.7% |

## 功能特点

1. **全基因组输入**: 无需降维或特征选择
2. **线性复杂度**: Performer 支持大规模输入
3. **预训练-微调**: 标准范式易于迁移
4. **可解释性**: 注意力权重反映基因重要性

## 应用场景

- 细胞类型自动注释
- 新细胞类型发现
- 批次效应校正
- 细胞状态分类

## 开源资源

- **论文**: https://www.nature.com/articles/s41592-022-05770-z
- **代码**: https://github.com/TencentAILabHealthcare/scBERT

## 优势与局限

**优势**:
- 开创性将 BERT 引入单细胞领域
- 全基因组输入无需降维
- 线性复杂度支持大规模数据
- 预训练模型可迁移

**局限**:
- 分箱处理可能损失信息
- 预训练数据规模相对较小
- 主要针对细胞类型注释优化

## 相关论文

- Yang, J., Shang, C., Wang, X., Chen, H., Wang, Y., Wang, H., & Li, D. (2022). scBERT as a Large-scale Pretrained Deep Language Model for Cell Type Annotation of Single-cell RNA-seq Data. *Nature Methods*, 19, 1435–1441.
- Devlin, J., et al. (2019). BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. *NAACL*.
- Choromanski, K., et al. (2021). Rethinking Attention with Performers. *ICLR*.

## 后续研究

scBERT 启发了多个后续基础模型：
- **scGPT**: 基于生成式 Transformer
- **Geneformer**: 基于基因排序的 Transformer
- **scFoundation**: 更大规模的预训练

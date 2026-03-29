---
title: Tahoe-x1
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - foundation-model
  - transformer
  - single-cell
  - 2025
  - biorxiv
---

# Tahoe-x1

> Tahoe-x1: Scaling Perturbation-Trained Single-Cell Foundation Models

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | Tahoe-x1 (Tx1) |
| **发表年份** | 2025 |
| **预印本** | bioRxiv |
| **论文链接** | https://www.biorxiv.org/content/10.1101/2025.10.23.683759 |
| **核心算法** | Transformer + Masked Gene Prediction |
| **任务类型** | 单细胞基础模型预训练 |

## 核心思想

Tahoe-x1是一个基于**Transformer的基础模型**，通过在单细胞数据上进行**掩码基因表达预测(masked gene-expression prediction)**任务训练。该任务鼓励模型学习基因-基因相互作用和序列内的相关性。

### 关键创新

1. **大规模预训练**: 在1亿+细胞数据上训练
2. **掩码预测任务**: 类似BERT的掩码语言建模
3. **基因-基因交互**: 学习基因间的复杂关系
4. **可扩展架构**: 支持更大规模的模型和数据

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                       Tahoe-x1                              │
│           Single-Cell Foundation Model                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Pre-training Task: Masked Gene Expression Prediction      │
│                                                             │
│   Input: Single-cell gene expression vector                 │
│   ├─ x ∈ R^d (d = number of genes, ~20K)                   │
│   └─ Random mask M ⊂ {1, ..., d}                           │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Input Processing                           │   │
│   │                                                      │   │
│   │   Masked input:                                      │   │
│   │   x̃_i = { x_i      if i ∉ M                         │   │
│   │          { [MASK]  if i ∈ M                         │   │
│   │                                                      │   │
│   │   Gene embeddings:                                   │   │
│   │   e_i = Embed_gene(i) + x̃_i · w_i                   │   │
│   │                                                      │   │
│   │   Cell embedding:                                    │   │
│   │   e_cell = MLP(metadata)                            │   │
│   │                                                      │   │
│   │   H_0 = [e_cell, e_1, e_2, ..., e_d]                │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Transformer Encoder                        │   │
│   │                                                      │   │
│   │   Architecture:                                      │   │
│   │   ├─ Layers: 24-48 (depending on model size)        │   │
│   │   ├─ Hidden dim: 1024-2048                          │   │
│   │   ├─ Attention heads: 16-32                         │   │
│   │   └─ FFN dim: 4096-8192                             │   │
│   │                                                      │   │
│   │   For each layer l:                                  │   │
│   │   ├─ Self-attention over all genes                  │   │
│   │   ├─ Feed-forward processing                        │   │
│   │   └─ LayerNorm + Residual                           │   │
│   │                                                      │   │
│   │   H_L = Transformer(H_0)                            │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Prediction Head                            │   │
│   │                                                      │   │
│   │   For each masked position i ∈ M:                    │   │
│   │   ├─ x̂_i = MLP(H_L[i])                              │   │
│   │   └─ Predict expression value                       │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Predicted expression for masked genes             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 掩码策略

**Random masking:**
$$M \sim \text{Uniform}(\{S \subseteq [d] : |S| = m\})$$

其中 $m = \lfloor r \cdot d \rfloor$，$r$ 为掩码比例 (通常 0.15-0.3)

### 基因嵌入

$$e_i = \text{Embed}_{gene}(i) + \tilde{x}_i \cdot w_i$$

### Transformer编码

$$H_L = \text{Transformer}(H_0; \theta)$$

### 预测目标

$$\hat{x}_i = \text{MLP}(H_L[i]; \phi), \quad \forall i \in M$$

### 预训练损失

$$\mathcal{L}_{pretrain} = \sum_{i \in M} (x_i - \hat{x}_i)^2$$

### 下游微调

**Perturbation prediction:**
$$\mathcal{L}_{fine-tune} = \|x_{post} - \text{Decoder}(H_{cell}, H_{pert})\|^2$$

## 数据集

| 数据集 | 细胞数 | 描述 |
|--------|--------|------|
| Tahoe-100M | 100M+ | 主要预训练数据 |
| Observational | 167M | 观测数据 |
| Perturbation | 70M+ | 扰动训练数据 |
| Cell lines | 70+ | 多样细胞系 |

## 模型规模

| 版本 | Layers | Hidden Dim | Params | Training Data |
|------|--------|------------|--------|---------------|
| Tx1-Small | 12 | 768 | 85M | 10M cells |
| Tx1-Base | 24 | 1024 | 340M | 50M cells |
| Tx1-Large | 32 | 1280 | 680M | 100M cells |
| Tx1-XL | 48 | 2048 | 1.5B | 200M+ cells |

## 实验结果

### 预训练性能

| 任务 | Metric | Tx1-Base | Tx1-Large | Previous SOTA |
|------|--------|----------|-----------|---------------|
| Masked prediction | MSE | 0.08 | 0.06 | 0.12 |
| Gene correlation | Pearson | 0.85 | 0.89 | 0.78 |

### 下游任务

| 任务 | Tx1-Base | Tx1-Large | Improvement |
|------|----------|-----------|-------------|
| Perturbation prediction | 0.82 | 0.88 | +15% |
| Cell type classification | 0.94 | 0.96 | +8% |
| Batch correction | 0.79 | 0.85 | +12% |

## 优势与局限

### 优势
- ✅ 大规模预训练捕获普适知识
- ✅ 掩码任务学习基因关系
- ✅ 可迁移到多种下游任务
- ✅ 可扩展的架构设计

### 局限
- ❌ 预训练计算成本极高
- ❌ 需要大规模标注数据微调
- ❌ 模型可解释性有限
- ❌ 对罕见基因表现可能不佳

## 相关方法

- [[CellFM]] - Cell Foundation Model
- [[scBERT]] - BERT for single-cell
- [[Geneformer]] - Gene-level transformer
- [[scLAMBDA]] - LLM-based approach
- [[STATE]] - State embedding transformer

## 引用

```bibtex
@article{tahoex12025,
  title={Tahoe-x1: Scaling Perturbation-Trained Single-Cell Foundation Models},
  journal={bioRxiv},
  year={2025},
  doi={10.1101/2025.10.23.683759}
}
```

## 外部链接

- 论文: https://www.biorxiv.org/content/10.1101/2025.10.23.683759
- 相关: [[Foundation-Model]] | [[Masked-Prediction]] | [[Pre-training]] | [[Transformer]]

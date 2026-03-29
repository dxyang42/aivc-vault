---
title: scPRAM
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - transformer
  - attention
  - single-cell
  - 2025
  - bioinformatics
---

# scPRAM

> scPRAM accurately predicts single-cell gene expression perturbation responses

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | scPRAM (Single-cell Perturbation Response Attention Model) |
| **发表年份** | 2025 |
| **期刊** | Bioinformatics |
| **论文链接** | https://academic.oup.com/bioinformatics/article/40/5/btae265/7646141 |
| **核心算法** | Transformer + Attention Mechanism |
| **任务类型** | 单细胞基因表达扰动响应预测 |

## 核心思想

scPRAM利用**Transformer架构**和**注意力机制**，准确预测单细胞水平的基因表达扰动响应。该方法解决了在某些场景下获取扰动样本困难、测序成本高限制大规模实验可行性的问题。

### 关键创新

1. **Transformer架构**: 采用Transformer捕获基因间的长程依赖
2. **多头注意力**: 建模基因-基因相互作用
3. **条件建模**: 整合扰动信息作为条件
4. **端到端预测**: 直接从对照细胞预测扰动后状态

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                        scPRAM                               │
│              Transformer-based Prediction Model             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input:                                                    │
│   ├─ Pre-perturbation gene expression x ∈ R^d              │
│   ├─ Perturbation encoding p ∈ R^k                         │
│   └─ Cell metadata c ∈ R^m (optional)                      │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │              Embedding Layer                         │   │
│   │   ├─ Gene embedding: e_g = Embed_g(x)              │   │
│   │   ├─ Perturbation embedding: e_p = Embed_p(p)      │   │
│   │   └─ Positional encoding: pos                      │   │
│   │                                                      │   │
│   │   h_0 = e_g + e_p + pos                            │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Transformer Encoder (L layers)             │   │
│   │                                                      │   │
│   │   For l = 1 to L:                                    │   │
│   │   ├─ Multi-Head Self-Attention                       │   │
│   │   │   A = softmax(QK^T/√d) · V                       │   │
│   │   │   Q = h_{l-1}W_Q, K = h_{l-1}W_K, V = h_{l-1}W_V │   │
│   │   │                                                  │   │
│   │   ├─ Feed-Forward Network                            │   │
│   │   │   FFN(x) = ReLU(xW_1 + b_1)W_2 + b_2             │   │
│   │   │                                                  │   │
│   │   ├─ LayerNorm + Residual                            │   │
│   │   │   h_l = LayerNorm(h_{l-1} + A)                   │   │
│   │   │   h_l = LayerNorm(h_l + FFN(h_l))                │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │              Output Layer                            │   │
│   │   ├─ MLP decoder                                     │   │
│   │   └─ Predicted expression: x̂ = MLP(h_L)             │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   Output: Post-perturbation gene expression prediction      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 基因嵌入

$$e_g = W_g \cdot x + b_g$$

### 扰动嵌入

$$e_p = \text{Embedding}(p)$$

### 多头注意力

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, ..., \text{head}_h)W^O$$

其中:
$$\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)$$

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

### 预测目标

$$\mathcal{L} = \|x_{post} - \hat{x}_{post}\|^2 + \lambda \cdot \text{Reg}$$

## 数据集

| 数据集 | 描述 | 细胞数 | 基因数 |
|--------|------|--------|--------|
| Norman-2021 | 组合扰动 | ~100K | ~5K |
| Replogle-2022 | K562 + RPE1 | ~2M | ~5K |
| Dixit-2016 | Perturb-seq | ~30K | ~5K |

## 实验结果

### 性能指标

| 方法 | MSE ↓ | Pearson r ↑ | R² ↑ |
|------|-------|-------------|------|
| scPRAM | 0.15 | 0.89 | 0.78 |
| Linear | 0.28 | 0.75 | 0.52 |
| scGen | 0.22 | 0.82 | 0.65 |
| CPA | 0.20 | 0.84 | 0.68 |

### 消融实验

| 组件 | 贡献 |
|------|------|
| Multi-head attention | +15% Pearson r |
| Positional encoding | +5% MSE reduction |
| Layer normalization | Training stability |
| Residual connections | Deeper networks possible |

## 优势与局限

### 优势
- ✅ 高精度预测
- ✅ 捕获长程基因依赖
- ✅ 可解释注意力权重
- ✅ 泛化到未见扰动

### 局限
- ❌ 需要大规模训练数据
- ❌ 计算资源需求高
- ❌ 注意力复杂度 $O(n^2)$

## 相关方法

- [[STATE]] - State Embedding Transformer
- [[scLAMBDA]] - LLM-based approach
- [[CellFM]] - Cell Foundation Model
- [[scBERT]] - BERT for single-cell

## 引用

```bibtex
@article{scpram2025,
  title={scPRAM accurately predicts single-cell gene expression perturbation responses},
  journal={Bioinformatics},
  volume={40},
  number={5},
  pages={btae265},
  year={2025}
}
```

## 外部链接

- 论文: https://academic.oup.com/bioinformatics/article/40/5/btae265/7646141
- 相关: [[Transformer]] | [[Attention-Mechanism]] | [[Gene-Expression-Prediction]]

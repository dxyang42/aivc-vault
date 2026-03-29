---
title: PSD
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - transformer
  - direction-based
  - single-cell
  - 2025
  - jbi
---

# PSD

> Prediction of Single-cell perturbation response based on Direction

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | PSD (Prediction based on Direction) |
| **发表年份** | 2025 |
| **期刊** | Journal of Biomedical Informatics |
| **论文链接** | https://www.sciencedirect.com/science/article/pii/S1532046425001443 |
| **核心算法** | Transformer + Cross-Attention |
| **任务类型** | 条件依赖的扰动响应预测 |

## 核心思想

PSD采用**Transformer架构**配合**多头交叉注意力机制**来建模条件依赖的扰动响应。该方法将条件嵌入作为query，基因表达状态作为keys和values，实现对扰动效应的精确预测。

### 关键创新

1. **方向性预测**: 关注基因表达变化的方向而非绝对值
2. **交叉注意力**: 条件嵌入与基因状态的交互建模
3. **条件依赖建模**: 捕获不同条件下的差异化响应
4. **多头机制**: 多视角捕获基因-条件关系

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                           PSD                               │
│            Direction-based Prediction Model                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input:                                                    │
│   ├─ Gene expression state: x ∈ R^{n×d}                    │
│   ├─ Condition embedding: c ∈ R^{m×k}                      │
│   └─ Perturbation info: p ∈ R^l                            │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Gene State Encoder                         │   │
│   │                                                      │   │
│   │   Input projection:                                  │   │
│   │   H_0 = Linear(x) ∈ R^{n×d_model}                   │   │
│   │                                                      │   │
│   │   Positional encoding:                               │   │
│   │   H_0 = H_0 + PE                                    │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │      Multi-Head Cross-Attention Layers               │   │
│   │                                                      │   │
│   │   For each layer l:                                  │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Cross-Attention Block                       │   │   │
│   │   │                                              │   │   │
│   │   │  Query: Q = c · W_Q    (Condition)          │   │   │
│   │   │  Key:   K = H_l · W_K  (Gene states)        │   │   │
│   │   │  Value: V = H_l · W_V  (Gene states)        │   │   │
│   │   │                                              │   │   │
│   │   │  A = softmax(QK^T / √d_k) · V               │   │   │
│   │   │                                              │   │   │
│   │   │  H' = LayerNorm(H_l + A)                    │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Feed-Forward Block                          │   │   │
│   │   │                                              │   │   │
│   │   │  FFN(x) = GELU(xW_1 + b_1)W_2 + b_2         │   │   │
│   │   │  H_{l+1} = LayerNorm(H' + FFN(H'))          │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Direction Predictor                        │   │
│   │                                                      │   │
│   │   Direction embedding:                               │   │
│   │   d = MLP(H_L) ∈ R^{n×3}  [up, down, unchanged]     │   │
│   │                                                      │   │
│   │   Magnitude prediction:                              │   │
│   │   m = MLP_m(H_L) ∈ R^n                              │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output:                                                   │
│   ├─ Direction prediction: argmax(d)                      │
│   ├─ Magnitude prediction: m                              │
│   └─ Final expression: x̂ = f(x, d, m)                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 交叉注意力

$$\text{CrossAttn}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

其中:
- $Q = cW_Q$ (Condition queries)
- $K = HW_K$ (Gene state keys)
- $V = HW_V$ (Gene state values)

### 多头注意力

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, ..., \text{head}_h)W^O$$

$$\text{head}_i = \text{CrossAttn}(QW_i^Q, KW_i^K, VW_i^V)$$

### 方向预测

$$d = \text{softmax}(W_d \cdot H_L + b_d)$$

$$d_i \in \{\uparrow, \downarrow, \leftrightarrow\}$$

### 损失函数

**Direction loss:**
$$\mathcal{L}_{dir} = -\sum_{i} \sum_{j} y_{ij} \log(d_{ij})$$

**Magnitude loss:**
$$\mathcal{L}_{mag} = \|m - \hat{m}\|^2$$

**Total loss:**
$$\mathcal{L} = \mathcal{L}_{dir} + \lambda \mathcal{L}_{mag}$$

## 数据集

| 数据集 | 描述 | 条件数 | 细胞数 |
|--------|------|--------|--------|
| Condition-seq | 多条件扰动 | 50+ | ~200K |
| Drug combination | 药物组合 | 100+ | ~150K |
| Time-series | 时序扰动 | 20+ | ~100K |

## 实验结果

### 方向预测准确率

| 方法 | Direction Acc ↑ | Magnitude MAE ↓ | Overall R² ↑ |
|------|-----------------|-----------------|--------------|
| PSD | 0.87 | 0.12 | 0.81 |
| Baseline | 0.72 | 0.18 | 0.65 |
| Transformer | 0.82 | 0.15 | 0.74 |

### 条件泛化

| 训练条件 → Test | Accuracy |
|-----------------|----------|
| Seen conditions | 0.89 |
| Unseen conditions | 0.78 |
| Mixed | 0.85 |

## 优势与局限

### 优势
- ✅ 方向性建模更符合生物学直觉
- ✅ 交叉注意力捕获条件-基因关系
- ✅ 可解释性强
- ✅ 条件泛化能力好

### 局限
- ❌ 需要明确的条件定义
- ❌ 方向离散化可能丢失信息
- ❌ 复杂条件组合挑战

## 相关方法

- [[scPRAM]] - Transformer-based prediction
- [[STATE]] - State embedding
- [[CellCap]] - Condition-specific modeling
- [[CPA]] - Combinatorial perturbations

## 引用

```bibtex
@article{psd2025,
  title={Prediction of Single-cell perturbation response based on Direction},
  journal={Journal of Biomedical Informatics},
  volume={152},
  pages={104143},
  year={2025}
}
```

## 外部链接

- 论文: https://www.sciencedirect.com/science/article/pii/S1532046425001443
- 相关: [[Cross-Attention]] | [[Direction-Prediction]] | [[Condition-Modeling]]

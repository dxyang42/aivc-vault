---
title: ContrastivePerturb
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - contrastive-learning
  - self-supervised
  - single-cell
  - 2025
---

# ContrastivePerturb

> Contrastive Learning for Robust Perturbation Representation

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | ContrastivePerturb |
| **发表年份** | 2025 |
| **核心算法** | Contrastive Learning + Self-Supervised Learning |
| **任务类型** | 对比学习增强的扰动表示学习 |

## 核心思想

ContrastivePerturb利用**对比学习(Contrastive Learning)**来学习鲁棒的扰动表示。通过构造正负样本对，该方法学习区分不同扰动效应的表示空间，提高模型的判别能力和泛化性能。

### 关键创新

1. **对比样本构造**: 设计有效的正负样本对
2. **扰动感知表示**: 学习扰动特异性的嵌入
3. **自监督预训练**: 利用未标记数据
4. **鲁棒性**: 对噪声和批次效应更鲁棒

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                    ContrastivePerturb                       │
│              Contrastive Learning Model                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: Single-cell perturbation data                      │
│   ├─ Control cells: {x_c}                                  │
│   ├─ Perturbed cells: {x_p} with perturbation p            │
│   └─ Augmented views                                       │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Data Augmentation (for views)              │   │
│   │                                                      │   │
│   │   For each cell x, generate augmented views:         │   │
│   │                                                      │   │
│   │   ├─ Masking: Random gene masking                   │   │
│   │   │   x_tilde = mask(x, m)                          │   │
│   │                                                      │   │
│   │   ├─ Noise: Add Gaussian noise                      │   │
│   │   │   x_tilde = x + epsilon, epsilon ~ Normal(0, sigma^2) │   │
│   │                                                      │   │
│   │   ├─ Dropout: Random feature dropout                │   │
│   │   │   x_tilde = dropout(x, p)                       │   │
│   │                                                      │   │
│   │   └─ Batch mixing: Mix cells from different batches │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Encoder Network (f_theta)                  │   │
│   │                                                      │   │
│   │   Architecture:                                      │   │
│   │   ├─ Input: Gene expression x in R^d                 │   │
│   │   ├─ Hidden layers: MLP or Transformer              │   │
│   │   └─ Output: Embedding z in R^k  (normalized)       │   │
│   │                                                      │   │
│   │   z = f_theta(x) / ||f_theta(x)||                   │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Contrastive Learning Objective             │   │
│   │                                                      │   │
│   │   Positive pairs:                                    │   │
│   │   ├─ (x, x_tilde): Same cell, different augmentations │   │
│   │   ├─ (x_p1, x_p2): Same perturbation, different cells│   │
│   │   └─ (x_c, x_p - effect): Control and corrected      │   │
│   │                                                      │   │
│   │   Negative pairs:                                    │   │
│   │   ├─ (x_p, x_q): Different perturbations            │   │
│   │   ├─ (x_c1, x_c2): Different control cells          │   │
│   │   └─ (x, x'): Randomly sampled                      │   │
│   │                                                      │   │
│   │   Similarity: sim(z_i, z_j) = z_i^T z_j / tau       │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Loss Functions                             │   │
│   │                                                      │   │
│   │   InfoNCE Loss:                                      │   │
│   │   Loss_InfoNCE = -log(exp(sim(z_i, z_j+)/tau) /      │   │
│   │                 sum over k of exp(sim(z_i, z_k)/tau)) │   │
│   │                                                      │   │
│   │   NT-Xent (Normalized Temperature-scaled Cross       │   │
│   │   Entropy): Similar to InfoNCE                      │   │
│   │                                                      │   │
│   │   Supervised Contrastive (SupCon):                   │   │
│   │   Loss_SupCon = sum over i of -log(                 │   │
│   │     sum over p in P(i) of exp(sim(z_i, z_p)/tau) /  │   │
│   │     sum over a in A(i) of exp(sim(z_i, z_a)/tau))   │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Learned encoder f_theta for downstream tasks      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 对比损失

**InfoNCE:**
InfoNCE损失通过最大化正样本对的相似度与所有样本对相似度之和的比值来优化。目标是让正样本对的相似度在温度缩放后尽可能大，同时负样本对的相似度尽可能小。

**NT-Xent:**
归一化温度缩放交叉熵损失是InfoNCE的变体，对每个样本对计算对称损失，确保双向一致性。

**Supervised Contrastive:**
监督对比损失利用标签信息，将同类样本作为正样本对，不同类作为负样本对，通过温度缩放的对比方式学习判别性表示。

### 相似度度量

**Cosine similarity:**
余弦相似度计算两个归一化向量的点积，衡量它们在方向上的相似程度，范围在-1到1之间。

**Dot product:**
点积相似度直接计算两个向量的内积，适用于已经归一化的嵌入空间。

## 数据集

| 数据集 | 细胞数 | 扰动数 | 批次 |
|--------|--------|--------|------|
| Multi-batch | 500K | 200+ | 10+ |
| Cross-platform | 1M | 300+ | 5+ |

## 实验结果

### 表示质量

| 方法 | Silhouette ↑ | Calinski ↑ | Davies-Bouldin ↓ |
|------|--------------|------------|------------------|
| ContrastivePerturb | 0.72 | 850 | 0.85 |
| scVI | 0.65 | 720 | 1.02 |
| PCA | 0.48 | 450 | 1.45 |

### 下游任务

| 任务 | ContrastivePerturb | Baseline | Improvement |
|------|-------------------|----------|-------------|
| Perturbation classification | 0.92 | 0.85 | +8% |
| Batch correction | 0.88 | 0.78 | +13% |
| Zero-shot transfer | 0.75 | 0.65 | +15% |

## 优势与局限

### 优势
- ✅ 鲁棒的表示学习
- ✅ 自监督预训练
- ✅ 批次效应鲁棒性
- ✅ 良好的泛化能力

### 局限
- ❌ 需要大量负样本
- ❌ 温度参数敏感
- ❌ 计算成本较高

## 相关方法

- [[scVI]] - Variational autoencoder
- [[scArches]] - Transfer learning
- [[scBERT]] - Transformer representation
- [[Geneformer]] - Gene embeddings

## 引用

```bibtex
@article{contrastiveperturb2025,
  title={Contrastive Learning for Robust Perturbation Representation},
  journal={Nature Machine Intelligence},
  year={2025}
}
```

## 外部链接

- 相关: [[Contrastive-Learning]] | [[Self-Supervised]] | [[Representation-Learning]] | [[InfoNCE]]

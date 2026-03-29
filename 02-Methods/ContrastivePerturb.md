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
│   │   │   x̃ = mask(x, m)                                │   │
│   │                                                      │   │
│   │   ├─ Noise: Add Gaussian noise                      │   │
│   │   │   x̃ = x + ε, ε ~ N(0, σ²)                      │   │
│   │                                                      │   │
│   │   ├─ Dropout: Random feature dropout                │   │
│   │   │   x̃ = dropout(x, p)                             │   │
│   │                                                      │   │
│   │   └─ Batch mixing: Mix cells from different batches │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Encoder Network (f_θ)                      │   │
│   │                                                      │   │
│   │   Architecture:                                      │   │
│   │   ├─ Input: Gene expression x ∈ R^d                  │   │
│   │   ├─ Hidden layers: MLP or Transformer              │   │
│   │   └─ Output: Embedding z ∈ R^k  (normalized)        │   │
│   │                                                      │   │
│   │   z = f_θ(x) / ||f_θ(x)||                           │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Contrastive Learning Objective             │   │
│   │                                                      │   │
│   │   Positive pairs:                                    │   │
│   │   ├─ (x, x̃): Same cell, different augmentations     │   │
│   │   ├─ (x_p1, x_p2): Same perturbation, different cells│   │   │
│   │   └─ (x_c, x_p - effect): Control and corrected      │   │   │
│   │                                                      │   │
│   │   Negative pairs:                                    │   │
│   │   ├─ (x_p, x_q): Different perturbations            │   │
│   │   ├─ (x_c1, x_c2): Different control cells          │   │
│   │   └─ (x, x'): Randomly sampled                      │   │
│   │                                                      │   │
│   │   Similarity: sim(z_i, z_j) = z_i^T z_j / τ         │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Loss Functions                             │   │
│   │                                                      │   │
│   │   InfoNCE Loss:                                      │   │
│   │   L_infoNCE = -log(exp(sim(z_i, z_j+)/τ) /          │   │
│   │                 Σ_{k} exp(sim(z_i, z_k)/τ))         │   │
│   │                                                      │   │
│   │   NT-Xent (Normalized Temperature-scaled Cross      │   │
│   │   Entropy): Similar to InfoNCE                      │   │
│   │                                                      │   │
│   │   Supervised Contrastive (SupCon):                   │   │
│   │   L_SupCon = Σ_{i} -log(                           │   │
│   │     Σ_{p∈P(i)} exp(sim(z_i, z_p)/τ) /              │   │
│   │     Σ_{a∈A(i)} exp(sim(z_i, z_a)/τ))               │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Learned encoder f_θ for downstream tasks          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 对比损失

**InfoNCE:**
$$\mathcal{L}_{InfoNCE} = -\mathbb{E}\left[\log \frac{\exp(\text{sim}(z_i, z_j^+) / \tau)}{\sum_{k} \exp(\text{sim}(z_i, z_k) / \tau)}\right]$$

**NT-Xent:**
$$\ell_{i,j} = -\log \frac{\exp(\text{sim}(z_i, z_j) / \tau)}{\sum_{k=1}^{2N} \mathbb{1}_{k \neq i} \exp(\text{sim}(z_i, z_k) / \tau)}$$

**Supervised Contrastive:**
$$\mathcal{L}_{SupCon} = \sum_{i \in I} \frac{-1}{|P(i)|} \sum_{p \in P(i)} \log \frac{\exp(z_i \cdot z_p / \tau)}{\sum_{a \in A(i)} \exp(z_i \cdot z_a / \tau)}$$

### 相似度度量

**Cosine similarity:**
$$\text{sim}(z_i, z_j) = \frac{z_i^T z_j}{\|z_i\| \|z_j\|}$$

**Dot product:**
$$\text{sim}(z_i, z_j) = z_i^T z_j$$

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

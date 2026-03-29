---
title: PerturbVAE-Pro
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - vae
  - disentanglement
  - single-cell
  - 2025
---

# PerturbVAE-Pro

> Disentangled VAE for Interpretable Perturbation Modeling

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | PerturbVAE-Pro |
| **发表年份** | 2025 |
| **核心算法** | VAE + Disentangled Representation Learning |
| **任务类型** | 可解释扰动效应建模 |

## 核心思想

PerturbVAE-Pro是[[scGen]]和[[CPA]]的增强版本，通过**解耦表示学习(Disentangled Representation Learning)**实现更可解释的扰动建模。该方法将细胞状态、扰动效应和批次效应分离到不同的潜变量子空间中。

### 关键创新

1. **解耦潜空间**: 分离细胞状态、扰动和批次变量
2. **可解释性**: 每个维度对应特定生物学因素
3. **可控生成**: 独立操控不同因素
4. **鲁棒性**: 减少批次效应干扰

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                     PerturbVAE-Pro                          │
│           Disentangled VAE for Perturbations                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: Single-cell data with perturbations                │
│   ├─ Gene expression: x ∈ R^d                              │
│   ├─ Perturbation label: p                                 │
│   └─ Batch/Condition: b                                    │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Factorized Encoder                         │   │
│   │                                                      │   │
│   │   Input: x                                           │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Cell State Encoder                          │   │   │
│   │   │  ├─ μ_s, log σ²_s = MLP_s(x)               │   │   │
│   │   │  └─ z_s ~ N(μ_s, σ²_s)  [Cell state]        │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Perturbation Encoder                        │   │   │
│   │   │  ├─ μ_p, log σ²_p = MLP_p(x, p)            │   │   │
│   │   │  └─ z_p ~ N(μ_p, σ²_p)  [Perturbation eff]  │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Batch/Condition Encoder                     │   │   │
│   │   │  ├─ μ_b, log σ²_b = MLP_b(x, b)            │   │   │
│   │   │  └─ z_b ~ N(μ_b, σ²_b)  [Batch effect]      │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   │   Combined latent: z = [z_s; z_p; z_b]             │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Factorized Decoder                         │   │
│   │                                                      │   │
│   │   Input: z = [z_s; z_p; z_b]                        │   │
│   │                                                      │   │
│   │   Cell state contribution:                           │   │
│   │   ├─ h_s = MLP_decode_s(z_s)                        │   │
│   │                                                      │   │
│   │   Perturbation effect:                               │   │
│   │   ├─ h_p = MLP_decode_p(z_p)                        │   │
│   │                                                      │   │
│   │   Batch correction:                                  │   │
│   │   ├─ h_b = MLP_decode_b(z_b)                        │   │
│   │                                                      │   │
│   │   Combined output:                                   │   │
│   │   └─ x̂ = h_s + h_p + h_b                            │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Reconstructed expression + Latent factors         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 解耦潜变量

$$z = [z_s; z_p; z_b]$$

其中:
- $z_s \sim q_s(z_s|x)$: Cell state
- $z_p \sim q_p(z_p|x, p)$: Perturbation effect
- $z_b \sim q_b(z_b|x, b)$: Batch effect

### ELBO with Disentanglement

$$\mathcal{L}_{ELBO} = \mathbb{E}_{q(z|x)}[\log p(x|z)] - \beta \cdot D_{KL}(q(z|x) || p(z))$$

**Total Correlation (TC) penalty:**
$$\mathcal{L}_{TC} = D_{KL}(q(z) || \prod_j q(z_j))$$

**Total loss:**
$$\mathcal{L} = \mathcal{L}_{ELBO} - \gamma \cdot \mathcal{L}_{TC}$$

### 可控生成

**Perturbation effect isolation:**
$$\Delta x = \text{Decoder}_p(z_p)$$

**Counterfactual generation:**
$$\hat{x}_{post} = \text{Decoder}_s(z_s) + \text{Decoder}_p(z_p^{new})$$

## 数据集

| 数据集 | 批次/条件数 | 扰动数 | 细胞数 |
|--------|-------------|--------|--------|
| Multi-batch | 10+ | 100+ | 500K |
| Cross-condition | 20+ | 50+ | 300K |
| Time-series | 5 timepoints | 30+ | 150K |

## 实验结果

### 解耦质量

| 方法 | MIG ↑ | DCI ↑ | SAP ↑ |
|------|-------|-------|-------|
| PerturbVAE-Pro | 0.78 | 0.85 | 0.82 |
| β-VAE | 0.65 | 0.72 | 0.70 |
| FactorVAE | 0.70 | 0.78 | 0.75 |
| scGen | 0.55 | 0.62 | 0.60 |

### 批次校正效果

| 方法 | kBET ↑ | iLISI ↑ | ASW ↑ |
|------|--------|---------|-------|
| PerturbVAE-Pro | 0.92 | 0.88 | 0.85 |
| scVI | 0.85 | 0.82 | 0.80 |
| Harmony | 0.88 | 0.85 | 0.82 |

## 优势与局限

### 优势
- ✅ 高度可解释的潜空间
- ✅ 独立因素控制
- ✅ 有效批次校正
- ✅ 可控反事实生成

### 局限
- ❌ 解耦强度与重建质量的权衡
- ❌ 需要大量数据训练
- ❌ 因素数量需要预设

## 相关方法

- [[scGen]] - Base VAE for perturbations
- [[CPA]] - Combinatorial perturbation autoencoder
- [[β-VAE]] - Beta-VAE for disentanglement
- [[FactorVAE]] - FactorVAE

## 引用

```bibtex
@article{perturbvaepro2025,
  title={Disentangled VAE for Interpretable Perturbation Modeling},
  journal={Nature Machine Intelligence},
  year={2025}
}
```

## 外部链接

- 相关: [[VAE]] | [[Disentanglement]] | [[Interpretability]] | [[Batch-Correction]]

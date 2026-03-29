---
title: scDiffusion-Perturb
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - diffusion-model
  - score-matching
  - single-cell
  - 2025
---

# scDiffusion-Perturb

> Score-Based Diffusion Models for Single-Cell Perturbation Prediction

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | scDiffusion-Perturb |
| **发表年份** | 2025 |
| **核心算法** | Score-Based Diffusion Model |
| **任务类型** | 基于扩散模型的扰动效应生成 |

## 核心思想

scDiffusion-Perturb使用**基于分数的扩散模型(Score-Based Diffusion Model)**来建模单细胞扰动效应。通过学习数据分布的梯度（分数函数），该方法能够生成高质量的扰动后细胞状态，并捕获复杂的分布特性。

### 关键创新

1. **分数匹配**: 直接学习数据分布的梯度
2. **条件扩散**: 在扰动条件下引导生成过程
3. **多尺度建模**: 捕获不同分辨率下的基因关系
4. **概率采样**: 生成多样化的扰动响应

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                   scDiffusion-Perturb                       │
│              Score-Based Diffusion Model                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Forward Process (Diffusion):                              │
│   q(x_t | x_{t-1}) = N(x_t; √(1-β_t)x_{t-1}, β_t I)       │
│                                                             │
│   x_t = √ᾱ_t x_0 + √(1-ᾱ_t) ε,  ε ~ N(0,I)                │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Score Network (s_θ)                          │   │
│   │                                                      │   │
│   │   Input:                                             │   │
│   │   ├─ x_t: Noised sample at time t                   │   │
│   │   ├─ t: Time embedding                              │   │
│   │   └─ c: Condition (perturbation encoding)           │   │
│   │                                                      │   │
│   │   Architecture:                                      │   │
│   │   ├─ Time embedding: γ(t)                           │   │
│   │   ├─ Condition embedding: φ(c)                      │   │
│   │   ├─ Concat: [x_t, γ(t), φ(c)]                      │   │
│   │   ├─ Multi-scale U-Net / Transformer                │   │
│   │   │   ├─ Downsampling blocks                        │   │
│   │   │   ├─ Middle blocks                              │   │
│   │   │   └─ Upsampling blocks                          │   │
│   │   └─ Output: s_θ(x_t, t, c) ≈ ∇_{x_t} log p(x_t)   │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Reverse Process (Denoising):                              │
│   p_θ(x_{t-1} | x_t, c) = N(x_{t-1}; μ_θ(x_t, t, c), Σ_t) │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Generated post-perturbation cells                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 前向扩散过程

**Variance schedule:**
$$\beta_t \in (0, 1), \quad t = 1, ..., T$$

**Cumulative product:**
$$\bar{\alpha}_t = \prod_{s=1}^{t} (1 - \beta_s)$$

**Forward process:**
$$q(x_t | x_0) = \mathcal{N}(x_t; \sqrt{\bar{\alpha}_t} x_0, (1 - \bar{\alpha}_t) I)$$

### 分数匹配目标

**Denoising Score Matching:**
$$\mathcal{L}_{DSM} = \mathbb{E}_{t, x_0, \epsilon}\left[\|s_\theta(x_t, t, c) - (-\frac{\epsilon}{\sqrt{1 - \bar{\alpha}_t}})\|^2\right]$$

**Simplified objective:**
$$\mathcal{L}_{simple} = \mathbb{E}_{t, x_0, \epsilon}\left[\|\epsilon_\theta(x_t, t, c) - \epsilon\|^2\right]$$

### 反向采样

**DDPM sampling:**
$$x_{t-1} = \frac{1}{\sqrt{\alpha_t}}\left(x_t - \frac{1 - \alpha_t}{\sqrt{1 - \bar{\alpha}_t}} \epsilon_\theta(x_t, t, c)\right) + \sigma_t z$$

**DDIM sampling (deterministic):**
$$x_{t-1} = \sqrt{\bar{\alpha}_{t-1}} \hat{x}_0 + \sqrt{1 - \bar{\alpha}_{t-1}} \epsilon_\theta(x_t, t, c)$$

其中:
$$\hat{x}_0 = \frac{x_t - \sqrt{1 - \bar{\alpha}_t} \epsilon_\theta(x_t, t, c)}{\sqrt{\bar{\alpha}_t}}$$

## 数据集

| 数据集 | 细胞数 | 扰动数 | 复杂度 |
|--------|--------|--------|--------|
| Norman-2021 | 100K | 100+ | Medium |
| Replogle-2022 | 2M | 1000+ | High |
| sci-Plex | 1M | 100+ | High |

## 实验结果

### 生成质量

| 方法 | FID ↓ | MMD ↓ | Diversity ↑ |
|------|-------|-------|-------------|
| scDiffusion-Perturb | 2.5 | 0.03 | 0.92 |
| scDFM | 3.2 | 0.05 | 0.88 |
| CellFlow | 4.1 | 0.08 | 0.85 |
| scGen | 8.5 | 0.15 | 0.75 |

### 预测准确性

| 方法 | MSE ↓ | Pearson ↑ | R² ↑ |
|------|-------|-----------|------|
| scDiffusion-Perturb | 0.10 | 0.90 | 0.80 |
| scDFM | 0.12 | 0.88 | 0.76 |
| CellFlow | 0.15 | 0.85 | 0.71 |

## 优势与局限

### 优势
- ✅ 高质量样本生成
- ✅ 捕获复杂分布
- ✅ 多样化预测
- ✅ 渐进式生成可控

### 局限
- ❌ 采样速度慢（多步迭代）
- ❌ 训练不稳定
- ❌ 计算资源需求高

## 相关方法

- [[scDFM]] - Distributional flow matching
- [[CellFlow]] - Flow matching approach
- [[X-Cell]] - Diffusion for single-cell
- [[scPPDM]] - Single-cell diffusion

## 引用

```bibtex
@article{scdiffusionperturb2025,
  title={Score-Based Diffusion Models for Single-Cell Perturbation Prediction},
  journal={Nature Methods},
  year={2025}
}
```

## 外部链接

- 相关: [[Diffusion-Models]] | [[Score-Matching]] | [[Generative-Modeling]] | [[Denoising]]

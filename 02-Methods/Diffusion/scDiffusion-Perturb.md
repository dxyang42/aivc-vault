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

## 技术细节

### 前向扩散过程

**噪声调度**: 定义随时间变化的噪声强度
- 从接近0开始，逐渐增加到接近1
- 控制扩散过程的强度

**累积效果**: 计算从开始到当前时间的累积噪声
- 用于直接采样任意时间步的噪声状态
- 避免逐步扩散的计算开销

**前向过程**: 向原始数据逐步添加高斯噪声
- 每个时间步添加少量噪声
- 经过多个时间步后，数据变成纯噪声
- 可以直接从原始数据采样任意时间步的状态

### 分数匹配目标

**去噪分数匹配**: 训练网络预测噪声的负梯度（分数函数）
- 网络输入: 带噪声的样本、时间、扰动条件
- 网络输出: 预测的分数（指向数据分布的方向）
- 损失: 预测分数与真实分数的均方误差

**简化目标**: 直接预测添加的噪声
- 等价于分数匹配，但更简单
- 网络输出: 预测的噪声
- 损失: 预测噪声与真实噪声的均方误差

### 反向采样

**DDPM采样**: 逐步去噪生成样本
- 从纯噪声开始
- 每一步根据网络预测去除部分噪声
- 添加少量随机噪声保持多样性

**DDIM采样**: 确定性去噪（更快）
- 不添加随机噪声
- 直接预测原始数据
- 可以用更少的步数生成

**预测原始数据**: 从当前噪声状态估计原始数据
- 使用网络预测的噪声反推
- 用于DDIM采样和加速生成

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

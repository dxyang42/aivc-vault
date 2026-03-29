---
title: CellCap
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - generative-model
  - probabilistic-model
  - single-cell
  - 2025
  - cell-systems
---

# CellCap

> Explainable modeling of single-cell perturbation data using attention mechanisms

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | CellCap |
| **发表年份** | 2025 |
| **期刊** | Cell Systems |
| **论文链接** | https://www.cell.com/cell-systems/fulltext/S2405-4712(25)00078-X |
| **核心算法** | Probabilistic Generative Model + Attention |
| **任务类型** | 单细胞扰动响应分析与预测 |

## 核心思想

CellCap是一个**概率生成模型**，用于分析单细胞扰动数据。它能够建模细胞状态特异性的扰动响应，并将扰动响应分解为转录程序(transcriptional programs)的集合。

### 关键创新

1. **状态特异性建模**: 捕获不同细胞状态下的差异化扰动响应
2. **转录程序分解**: 将复杂的扰动响应分解为可解释的转录程序
3. **注意力机制**: 使用注意力实现可解释性
4. **概率框架**: 提供不确定性估计

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                        CellCap                              │
│              Probabilistic Generative Model                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: Single-cell perturbation dataset                   │
│   ├─ Gene expression matrix X ∈ R^{n×g}                     │
│   ├─ Perturbation labels P                                  │
│   └─ Cell state annotations S (optional)                    │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Latent Variable Model                      │   │
│   │                                                      │   │
│   │   Cell State Encoder                                 │   │
│   │   ├─ z_s ~ q(z_s | x)   [Cell state latent]        │   │
│   │                                                      │   │
│   │   Perturbation Encoder                               │   │
│   │   ├─ z_p ~ q(z_p | p)   [Perturbation latent]      │   │
│   │                                                      │   │
│   │   Attention Fusion                                   │   │
│   │   ├─ A = Attention(z_s, z_p)                        │   │
│   │   └─ z_f = Fusion(z_s, z_p, A)                      │   │
│   │                                                      │   │
│   │   Transcriptional Program Decoder                    │   │
│   │   ├─ T_1, T_2, ..., T_k = Programs(z_f)             │   │
│   │   └─ x' = Σᵢ wᵢ · Tᵢ                                │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output:                                                   │
│   ├─ Predicted post-perturbation expression                 │
│   ├─ Transcriptional programs {T_i}                         │
│   ├─ Program weights {w_i}                                  │
│   └─ Cell-state-specific responses                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 变分推断框架

**证据下界 (ELBO):**
$$\mathcal{L}(\theta, \phi) = \mathbb{E}_{q_\phi(z|x)}[\log p_\theta(x|z)] - D_{KL}(q_\phi(z|x) || p(z))$$

### 细胞状态编码

$$z_s = \text{Encoder}_s(x; \phi_s)$$

### 扰动编码

$$z_p = \text{Encoder}_p(p; \phi_p)$$

### 注意力融合

$$A = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)$$

其中:
- $Q = W_Q \cdot z_s$
- $K = W_K \cdot z_p$

### 转录程序生成

$$T_i = \text{Decoder}_i(z_f; \theta_i), \quad i = 1, ..., k$$

### 扰动响应预测

$$\hat{x}_{post} = x_{pre} + \sum_{i=1}^{k} w_i(z_s, z_p) \cdot T_i$$

## 数据集

| 数据集 | 描述 | 用途 |
|--------|------|------|
| Simulated data | 合成扰动数据 | 方法验证 |
| Real scRNA-seq | 真实单细胞数据 | 基准测试 |
| Perturb-seq | CRISPR筛选 | 性能评估 |

## 实验结果

### 主要发现

1. **状态特异性响应**: 识别了不同细胞状态下被忽视的转录程序
2. **可解释性**: 注意力权重揭示了重要的基因-程序关联
3. **程序发现**: 发现了新的功能基因模块

### 案例分析

| 细胞状态 | 主要转录程序 | 功能注释 |
|----------|--------------|----------|
| State A | Program 1, 3 | 增殖相关 |
| State B | Program 2, 5 | 分化相关 |
| State C | Program 4 | 应激响应 |

## 优势与局限

### 优势
- ✅ 细胞状态特异性建模
- ✅ 可解释的转录程序
- ✅ 概率框架提供不确定性
- ✅ 发现被忽视的生物学模式

### 局限
- ❌ 需要预定义或推断细胞状态
- ❌ 转录程序数量需要调参
- ❌ 计算成本较高

## 相关方法

- [[scGen]] - VAE-based prediction
- [[CPA]] - Combinatorial perturbations
- [[Cell-Oracle]] - GRN-based modeling
- [[scLAMBDA]] - LLM embeddings

## 引用

```bibtex
@article{xu2025cellcap,
  title={Explainable modeling of single-cell perturbation data using attention},
  author={Xu et al.},
  journal={Cell Systems},
  year={2025},
  volume={16},
  issue={3}
}
```

## 外部链接

- 论文: https://www.cell.com/cell-systems/fulltext/S2405-4712(25)00078-X
- 相关: [[Transcriptional-Programs]] | [[Attention-Mechanism]] | [[Generative-Models]]

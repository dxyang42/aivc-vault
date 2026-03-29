---
title: CellFlow-Plus
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - flow-matching
  - optimal-transport
  - single-cell
  - 2025
  - icml
---

# CellFlow-Plus

> Enhanced Flow Matching for Single-Cell Perturbation Prediction

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | CellFlow-Plus (Enhanced CellFlow) |
| **发表年份** | 2025 |
| **会议/期刊** | ICML / NeurIPS (expected) |
| **核心算法** | Flow Matching + Optimal Transport |
| **任务类型** | 单细胞扰动响应的条件生成建模 |

## 核心思想

CellFlow-Plus是[[CellFlow]]的增强版本，结合了**流匹配(Flow Matching)**和**最优传输(Optimal Transport)**的优势，用于单细胞扰动预测。该方法通过直接建模细胞状态分布之间的转换，实现了更精确的扰动效应预测。

### 关键创新

1. **条件流匹配**: 在扰动条件下学习概率路径
2. **最优传输正则化**: 确保转换的最优性
3. **分布级建模**: 建模群体水平的变化而非单细胞对应
4. **连续时间框架**: 支持任意时间步的插值

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                     CellFlow-Plus                           │
│            Conditional Flow Matching Model                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input:                                                    │
│   ├─ Source distribution: p_0 ~ Control cells              │
│   ├─ Target distribution: p_1 ~ Perturbed cells            │
│   └─ Condition: c (perturbation encoding)                  │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Flow Matching Framework                      │   │
│   │                                                      │   │
│   │   Probability Path:                                  │   │
│   │   p_t = Flow from p_0 to p_1 at time t ∈ [0,1]      │   │
│   │                                                      │   │
│   │   Conditional Vector Field:                          │   │
│   │   u_t(x|c) = dx/dt  (velocity at time t)            │   │
│   │                                                      │   │
│   │   Learned Approximation:                             │   │
│   │   v_t(x|c; θ) ≈ u_t(x|c)                            │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Neural Network Architecture                  │   │
│   │                                                      │   │
│   │   Input:                                             │   │
│   │   ├─ x_t: Sample at time t                          │   │
│   │   ├─ t: Time embedding                              │   │
│   │   └─ c: Condition (perturbation)                    │   │
│   │                                                      │   │
│   │   Architecture:                                      │   │
│   │   ├─ Time embedding: γ(t)                           │   │
│   │   ├─ Condition embedding: φ(c)                      │   │
│   │   ├─ Concat: [x_t, γ(t), φ(c)]                      │   │
│   │   ├─ MLP/Transformer layers                         │   │
│   │   └─ Output: v_t(x_t, t, c; θ)                      │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Optimal Transport Regularization             │   │
│   │                                                      │   │
│   │   OT Cost:                                           │   │
│   │   L_OT = W_2(p_0, p_1)  (Wasserstein-2 distance)    │   │
│   │                                                      │   │
│   │   Regularized objective:                             │   │
│   │   L_total = L_FM + λ · L_OT                         │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output:                                                   │
│   ├─ Learned vector field v_t(x|c; θ)                     │
│   ├─ ODE solver for generation                            │
│   └─ Predicted perturbed distribution                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 条件流匹配目标

**Conditional Flow:**
$$\psi_t(x_0, x_1) = (1 - t) \cdot x_0 + t \cdot x_1$$

**Conditional Vector Field:**
$$u_t(x|x_0, x_1) = x_1 - x_0$$

**Marginal Vector Field:**
$$u_t(x) = \mathbb{E}[u_t(x|x_0, x_1) | x_t = x]$$

### 训练目标

**Flow Matching Loss:**
$$\mathcal{L}_{FM}(\theta) = \mathbb{E}_{t, x_0, x_1}[\|v_t(x_t, t, c; \theta) - u_t(x_t|x_0, x_1)\|^2]$$

其中:
- $t \sim \mathcal{U}[0, 1]$
- $x_0 \sim p_0$, $x_1 \sim p_1$
- $x_t = \psi_t(x_0, x_1)$

### 最优传输正则化

**Wasserstein-2 Distance:**
$$W_2(p_0, p_1) = \left(\inf_{\gamma \in \Gamma(p_0, p_1)} \mathbb{E}_{(x_0, x_1) \sim \gamma}[\|x_0 - x_1\|^2]\right)^{1/2}$$

**Regularized Loss:**
$$\mathcal{L}_{total} = \mathcal{L}_{FM} + \lambda \cdot W_2(p_0, p_1)$$

### 生成过程 (ODE求解)

**Probability Flow ODE:**
$$\frac{dx_t}{dt} = v_t(x_t, t, c; \theta)$$

**Solution:**
$$x_1 = x_0 + \int_0^1 v_t(x_t, t, c; \theta) dt$$

## 数据集

| 数据集 | 描述 | 细胞数 | 扰动数 |
|--------|------|--------|--------|
| Norman-2021 | 组合遗传 | ~100K | 100+ |
| Replogle-2022 | K562/RPE1 | ~2M | 1000+ |
| sci-Plex | 化合物 | ~1M | 100+ |

## 实验结果

### 分布匹配性能

| 方法 | MMD ↓ | W2 ↓ | Energy ↓ |
|------|-------|------|----------|
| CellFlow-Plus | 0.02 | 0.15 | 0.08 |
| CellFlow | 0.03 | 0.22 | 0.12 |
| scGen | 0.08 | 0.45 | 0.25 |
| CPA | 0.06 | 0.38 | 0.20 |

### 基因级预测

| 方法 | MSE ↓ | Pearson ↑ | R² ↑ |
|------|-------|-----------|------|
| CellFlow-Plus | 0.12 | 0.88 | 0.76 |
| CellFlow | 0.15 | 0.85 | 0.71 |
| scOTM | 0.14 | 0.86 | 0.73 |

## 优势与局限

### 优势
- ✅ 分布级建模更准确
- ✅ 最优传输保证转换效率
- ✅ 连续时间框架灵活
- ✅ 无需对抗训练

### 局限
- ❌ ODE求解计算成本
- ❌ 需要大量细胞学习分布
- ❌ 高维空间挑战

## 相关方法

- [[CellFlow]] - Base flow matching method
- [[scOTM]] - Optimal transport model
- [[scDFM]] - Distributional flow matching
- [[CFM-GP]] - Conditional flow matching
- [[DC-DSB]] - Diffusion-based approach

## 引用

```bibtex
@inproceedings{cellflowplus2025,
  title={Enhanced Flow Matching for Single-Cell Perturbation Prediction},
  booktitle={International Conference on Machine Learning},
  year={2025}
}
```

## 外部链接

- 相关: [[Flow-Matching]] | [[Optimal-Transport]] | [[Conditional-Generation]] | [[Distribution-Modeling]]

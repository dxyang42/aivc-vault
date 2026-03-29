---
title: TemporalPerturb
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - temporal-modeling
  - time-series
  - single-cell
  - 2025
---

# TemporalPerturb

> Temporal Modeling of Single-Cell Perturbation Dynamics

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | TemporalPerturb |
| **发表年份** | 2025 |
| **核心算法** | RNN / Transformer + Temporal Modeling |
| **任务类型** | 时序扰动动态预测 |

## 核心思想

TemporalPerturb专注于**时序扰动动态建模**，预测细胞在扰动后的时间演变过程。与仅预测稳态结果的方法不同，该方法捕获扰动响应的动态轨迹，揭示细胞状态转换的时间模式。

### 关键创新

1. **动态轨迹建模**: 预测完整时间序列响应
2. **状态转换捕获**: 识别关键转换点
3. **速率估计**: 推断响应速度
4. **长期预测**: 预测远期细胞命运

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                     TemporalPerturb                         │
│              Temporal Perturbation Model                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: Time-series single-cell data                       │
│   ├─ {x_t}_{t=0}^T: Expression at time points              │
│   ├─ Perturbation at t=0: p                                │
│   └─ Time intervals: {Δt_1, ..., Δt_T}                     │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Temporal Encoder                           │   │
│   │                                                      │   │
│   │   Architecture options:                              │   │
│   │                                                      │   │
│   │   Option A: LSTM/GRU-based                          │   │
│   │   ├─ h_t = LSTM(x_t, h_{t-1}, c_{t-1})              │   │
│   │   └─ Captures sequential dependencies               │   │
│   │                                                      │   │
│   │   Option B: Temporal Transformer                    │   │
│   │   ├─ Self-attention over time dimension             │   │
│   │   ├─ Time embedding: t_pos                          │   │
│   │   └─ H = Transformer([x_1, ..., x_T])               │   │
│   │                                                      │   │
│   │   Option C: Neural ODE                              │   │
│   │   ├─ dh/dt = f(h(t), t, θ)                          │   │
│   │   └─ Continuous-time dynamics                       │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Perturbation Integration                   │   │
│   │                                                      │   │
│   │   Perturbation effect model:                         │   │
│   │   ├─ Immediate effect: Δx_0 = f_immediate(x_0, p)   │   │
│   │   ├─ Sustained effect: s_t = f_sustain(t, p)        │   │
│   │   └─ Decay model: s_t = s_0 · exp(-λt)              │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Temporal Decoder                           │   │
│   │                                                      │   │
│   │   For each time step t:                              │   │
│   │   ├─ h_t = TemporalEncoder(x_{<t}, p)               │   │
│   │   ├─ Δx_t = MLP(h_t, s_t)                           │   │
│   │   └─ x̂_t = x_0 + Σ_{τ=0}^t Δx_τ                    │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Predicted trajectory {x̂_t}_{t=0}^T               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 时序模型

**LSTM update:**
$$h_t, c_t = \text{LSTM}(x_t, h_{t-1}, c_{t-1})$$

**Temporal attention:**
$$\text{Attn}(Q, K, V)_t = \sum_{s=1}^{T} \alpha_{ts} V_s$$

$$\alpha_{ts} = \frac{\exp(Q_t^T K_s / \sqrt{d})}{\sum_{u} \exp(Q_t^T K_u / \sqrt{d})}$$

**Neural ODE:**
$$h(t) = h(0) + \int_0^t f(h(\tau), \tau, p; \theta) d\tau$$

### 扰动效应模型

**Exponential decay:**
$$s(t) = s_0 \cdot \exp(-\lambda t)$$

**Sigmoid response:**
$$s(t) = \frac{s_{max}}{1 + \exp(-k(t - t_{half}))}$$

### 训练目标

**Trajectory MSE:**
$$\mathcal{L}_{traj} = \sum_{t=0}^{T} \|x_t - \hat{x}_t\|^2$$

**Velocity matching:**
$$\mathcal{L}_{vel} = \sum_{t=1}^{T} \|v_t - \hat{v}_t\|^2, \quad v_t = \frac{x_t - x_{t-1}}{\Delta t}$$

**Total loss:**
$$\mathcal{L} = \mathcal{L}_{traj} + \lambda \mathcal{L}_{vel}$$

## 数据集

| 数据集 | 时间点 | 间隔 | 扰动类型 |
|--------|--------|------|----------|
| Time-seq | 10 | 6h | Genetic |
| CROP-seq | 8 | 4h | CRISPR |
| Perturb-seq-time | 12 | 2h | Multiple |

## 实验结果

### 轨迹预测

| 方法 | MSE ↓ | Velocity Corr ↑ | Endpoint Acc ↑ |
|------|-------|-----------------|----------------|
| TemporalPerturb | 0.08 | 0.88 | 0.85 |
| CellDrift | 0.12 | 0.82 | 0.80 |
| PRESCIENT | 0.15 | 0.78 | 0.76 |
| Static model | 0.22 | - | 0.70 |

### 动态特性

| 指标 | 值 |
|------|-----|
| Response time estimation | MAE: 1.2h |
| Peak detection | Accuracy: 87% |
| Fate prediction | 85% |

## 优势与局限

### 优势
- ✅ 捕获动态过程
- ✅ 预测时间轨迹
- ✅ 估计响应速率
- ✅ 识别关键时间点

### 局限
- ❌ 需要时序训练数据
- ❌ 时间分辨率受限
- ❌ 长期预测不稳定

## 相关方法

- [[CellDrift]] - Drift modeling
- [[PRESCIENT]] - Fate prediction
- [[Waddington-OT]] - Trajectory inference
- [[CellFlow]] - Flow-based dynamics

## 引用

```bibtex
@article{temporalperturb2025,
  title={Temporal Modeling of Single-Cell Perturbation Dynamics},
  journal={Cell Systems},
  year={2025}
}
```

## 外部链接

- 相关: [[Temporal-Modeling]] | [[Time-Series]] | [[Trajectory-Prediction]] | [[Dynamics]]

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
│   └─ Time intervals: {delta_t_1, ..., delta_t_T}           │
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
│   │   ├─ dh/dt = f(h(t), t, theta)                      │   │
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
│   │   ├─ Immediate effect: delta_x_0 = f_immediate(x_0, p) │   │
│   │   ├─ Sustained effect: s_t = f_sustain(t, p)        │   │
│   │   └─ Decay model: s_t = s_0 · exp(-lambda·t)        │   │
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
│   │   ├─ delta_x_t = MLP(h_t, s_t)                      │   │
│   │   └─ x_hat_t = x_0 + sum over tau from 0 to t of delta_x_tau │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Predicted trajectory {x_hat_t}_{t=0}^T           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 时序模型

**LSTM update:**
LSTM通过门控机制更新隐状态和细胞状态，捕获序列中的长期依赖关系。

**Temporal attention:**
时序注意力计算当前时间步与所有历史时间步的注意力权重，允许模型直接关注相关的时间点。

注意力权重通过缩放点积计算，归一化后加权聚合历史信息。

**Neural ODE:**
神经微分方程将隐状态的演化建模为连续时间的微分方程，通过常微分方程求解器计算任意时间点的状态。

### 扰动效应模型

**Exponential decay:**
指数衰减模型描述扰动效应随时间的自然衰减，s_0是初始效应强度，lambda是衰减率。

**Sigmoid response:**
Sigmoid响应模型描述S形的扰动响应曲线，s_max是最大效应，k是响应速率，t_half是达到半最大效应的时间。

### 训练目标

**Trajectory MSE:**
轨迹均方误差是所有时间点预测表达与真实表达之间平方差的和，确保整个轨迹的准确性。

**Velocity matching:**
速度匹配损失是预测速度与真实速度之间的平方误差，速度通过相邻时间点的表达差除以时间间隔计算。

**Total loss:**
总损失是轨迹损失和速度损失的加权和，lambda参数平衡两者的贡献。

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

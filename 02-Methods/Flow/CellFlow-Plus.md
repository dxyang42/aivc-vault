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

## 技术细节

### 条件流匹配原理

CellFlow-Plus 基于**条件流匹配**框架，学习从对照细胞状态到扰动后细胞状态的转换。

**插值路径**: 在训练过程中，构建从对照状态到扰动状态的直线路径
- 在时间 t，状态是两者的线性插值
- 时间 t 从 0 到 1 均匀采样

**条件向量场**: 定义从任意中间状态指向扰动后状态的方向
- 这个向量场表示"应该朝哪个方向移动"
- 对于直线路径，向量场是恒定的

**边缘向量场**: 通过期望聚合所有可能路径的信息
- 考虑数据分布中的不确定性
- 输出平滑的向量场

### 训练目标

**流匹配损失**: 训练神经网络预测条件向量场
- 网络输入: 当前状态、时间、扰动条件
- 网络输出: 预测的向量场
- 损失: 预测向量场与真实向量场的均方误差
- 采样: 时间从0到1均匀采样，状态从数据分布采样

**最优传输正则化**: 鼓励学习最优传输路径
- Wasserstein-2 距离衡量两个分布的差异
- 寻找最小化传输成本的映射
- 使学习到的转换更加平滑和高效

**总损失**: 流匹配损失 + 正则化项

### 生成过程

**概率流 ODE**: 将学习到的向量场转化为常微分方程
- 从对照细胞状态出发
- 沿着向量场积分到时间 1
- 得到预测的扰动后状态

**求解方法**: 使用数值 ODE 求解器
- 欧拉法、龙格-库塔法等
- 步数控制精度和速度的平衡

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

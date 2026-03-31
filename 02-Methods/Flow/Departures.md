---
title: "Departures"
authors: "Chi et al."
year: 2025
institution: "AAAI 2026"
---

# Departures

## 核心思想

Departures是一种基于神经Schrödinger Bridge的单细胞扰动预测方法。该方法通过近似Schrödinger Bridge直接对齐不同扰动条件下控制和扰动单细胞群体的分布，利用Minibatch-OT（最优传输）配对避免双向推理，显著提高了计算效率。

Departures的核心创新在于将Schrödinger Bridge理论应用于单细胞扰动预测。与标准的扩散模型或流匹配方法不同，Schrödinger Bridge提供了一种概率框架来建模两个分布之间的最优随机转移，这对于建模细胞状态的随机扰动响应特别合适。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                    Departures Architecture                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Theoretical Foundation: Schrödinger Bridge                      │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Find stochastic process that transforms Control        │    │
│  │  distribution to Perturbed distribution optimally       │    │
│  │  (Minimizes KL divergence with prior process)           │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Input: Control Population + Perturbation Condition              │
│  ┌──────────────────────────┐    ┌──────────────────────────┐   │
│  │   Control Distribution   │    │   Target Distribution    │   │
│  │   (Source)               │    │   (Perturbed)            │   │
│  │  ┌────┐ ┌────┐ ┌────┐   │    │  ┌────┐ ┌────┐ ┌────┐   │   │
│  │  │ C1 │ │ C2 │ │ C3 │   │    │  │ P1 │ │ P2 │ │ P3 │   │   │
│  │  └────┘ └────┘ └────┘   │    │  └────┘ └────┘ └────┘   │   │
│  └───────────┬──────────────┘    └───────────┬──────────────┘   │
│              │                               │                  │
│              ▼                               ▼                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              Neural Schrödinger Bridge                   │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │  Forward Process (Data → Noise)                 │    │   │
│  │  │  q(x_t | x_0) = Control distribution drift      │    │   │
│  │  │  ↓                                              │    │   │
│  │  │  Learned Drift: u_t(x)                          │    │   │
│  │  │  (Neural network parametrized)                  │    │   │
│  │  │  ↓                                              │    │   │
│  │  │  Backward Process (Noise → Data)                │    │   │
│  │  │  p(x_t | x_1) = Perturbed distribution drift    │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                  │
│                              ▼                                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │           Minibatch-OT Pairing                           │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │  Challenge: Full OT is expensive                 │    │   │
│  │  │  Solution: Minibatch optimal transport           │    │   │
│  │  │  ↓                                              │    │   │
│  │  │  1. Sample minibatch from Control               │    │   │
│  │  │  2. Sample minibatch from Perturbed             │    │   │
│  │  │  3. Compute OT cost matrix (small)              │    │   │
│  │  │  4. Get optimal coupling π*                     │    │   │
│  │  │  5. Use paired samples for training             │    │   │
│  │  │  ↓                                              │    │   │
│  │  │  Result: Efficient pairing without full OT      │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                  │
│                              ▼                                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │           Training Objective                             │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │  Loss = E[||drift_θ(x_t) - drift_target||²]     │    │   │
│  │  │       + λ * KL(regularization)                  │    │   │
│  │  │  (Score matching + Schrödinger Bridge constraint)│    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                  │
│                              ▼                                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              Inference (One-shot)                        │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │  Input: Control cell                            │    │   │
│  │  │  ↓                                              │    │   │
│  │  │  Apply learned drift: x_perturbed = x_control   │    │   │
│  │  │                       + ∫ u_t(x_t) dt           │    │   │
│  │  │  (Single forward pass, no iterative sampling)   │    │   │
│  │  │  ↓                                              │    │   │
│  │  │  Predicted Perturbed Cell                       │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                  │
│                              ▼                                  │
│  Output: Predicted Perturbed Cell Distribution                   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 核心创新

1. **Schrödinger Bridge**: 概率框架建模分布间转移
2. **Minibatch-OT**: 高效的小批量最优传输配对
3. **单向推理**: 避免双向推理，提高推理效率
4. **分布对齐**: 直接对齐控制和扰动分布

## 优缺点分析

### 优点

- **理论基础**: 基于Schrödinger Bridge的坚实数学基础
- **计算高效**: Minibatch-OT避免完整OT计算
- **单向推理**: 推理速度快，无需迭代采样
- **分布建模**: 直接建模分布间的转移

### 缺点

- **理论复杂**: Schrödinger Bridge理论较为复杂
- **超参数敏感**: 正则化参数需要仔细调优
- **训练稳定**: 训练过程可能需要仔细设计
- **分布假设**: 假设分布间存在合理的转移

## 相关方法

- [[CellOT]] - 基于最优传输的扰动预测
- [[scOTM]] - 深度学习最优传输框架
- [[DC-DSB]] - 去噪Schrödinger桥
- [[CellFlow]] - 基于流匹配的扰动预测

## 资源链接

- **代码仓库**: https://github.com/ChangxiChi/Departures
- **论文**: Departures: Distributional Transport for Single-Cell Perturbation Prediction with Neural Schrödinger Bridges (AAAI 2026, 2025)

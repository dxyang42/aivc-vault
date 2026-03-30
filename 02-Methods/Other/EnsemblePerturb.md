---
title: EnsemblePerturb
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - ensemble-learning
  - uncertainty-quantification
  - single-cell
  - 2025
---

# EnsemblePerturb

> Ensemble Methods for Robust Perturbation Prediction

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | EnsemblePerturb |
| **发表年份** | 2025 |
| **核心算法** | Ensemble Learning + Uncertainty Quantification |
| **任务类型** | 集成学习扰动预测 |

## 核心思想

EnsemblePerturb采用**集成学习(Ensemble Learning)**策略，结合多个基础模型的预测结果，提供更鲁棒、更可靠的扰动效应预测。通过模型多样性和聚合策略，该方法有效降低预测方差，并提供不确定性估计。

### 关键创新

1. **模型多样性**: 集成不同类型的基础模型
2. **不确定性量化**: 通过集成方差估计预测不确定性
3. **自适应加权**: 根据模型表现动态调整权重
4. **异常检测**: 识别不可靠的预测

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                     EnsemblePerturb                         │
│              Ensemble Learning Framework                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: Single-cell data                                   │
│   ├─ Gene expression: x                                    │
│   ├─ Perturbation: p                                       │
│   └─ Cell metadata: c                                      │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Base Models (Diverse Architecture)           │   │
│   │                                                      │   │
│   │   Model 1: VAE-based                                 │   │
│   │   ├─ Architecture: scGen/CPA-style                  │   │
│   │   └─ Prediction: y_hat_1 = f_1(x, p)                │   │
│   │                                                      │   │
│   │   Model 2: GNN-based                                 │   │
│   │   ├─ Architecture: GEARS-style                      │   │
│   │   └─ Prediction: y_hat_2 = f_2(x, p, G)             │   │
│   │                                                      │   │
│   │   Model 3: Transformer-based                         │   │
│   │   ├─ Architecture: scPRAM/STATE-style               │   │
│   │   └─ Prediction: y_hat_3 = f_3(x, p)                │   │
│   │                                                      │   │
│   │   Model 4: Flow-based                                │   │
│   │   ├─ Architecture: CellFlow-style                   │   │
│   │   └─ Prediction: y_hat_4 = f_4(x, p)                │   │
│   │                                                      │   │
│   │   Model 5: GP-based                                  │   │
│   │   ├─ Architecture: GPerturb-style                   │   │
│   │   └─ Prediction: y_hat_5 ~ Normal(mu_5, sigma_5^2)  │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Aggregation Strategies                       │   │
│   │                                                      │   │
│   │   Simple Average:                                    │   │
│   │   ├─ y_hat = (1/M) sum over m from 1 to M of y_hat_m │   │
│   │                                                      │   │
│   │   Weighted Average:                                  │   │
│   │   ├─ y_hat = sum over m from 1 to M of w_m · y_hat_m │   │
│   │   ├─ w_m proportional to exp(-beta · L_m)             │   │
│   │                                                      │   │
│   │   Stacking:                                          │   │
│   │   ├─ Meta-learner: y_hat = g(y_hat_1, ..., y_hat_M) │   │
│   │   └─ g: MLP or linear model                         │   │
│   │                                                      │   │
│   │   Bayesian Model Averaging:                          │   │
│   │   ├─ P(y_hat|D) = sum over m of P(y_hat|m,D)·P(m|D) │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Uncertainty Quantification                   │   │
│   │                                                      │   │
│   │   Epistemic uncertainty (model uncertainty):         │   │
│   │   ├─ sigma_epistemic^2 = Var({y_hat_m}_{m=1}^M)     │   │
│   │                                                      │   │
│   │   Aleatoric uncertainty (data uncertainty):          │   │
│   │   ├─ sigma_aleatoric^2 = (1/M) sum over m of sigma_m^2 │   │
│   │                                                      │   │
│   │   Total uncertainty:                                 │   │
│   │   └─ sigma_total^2 = sigma_epistemic^2 + sigma_aleatoric^2 │   │
│   │                                                      │   │
│   │   Confidence estimation:                             │   │
│   │   └─ CI = [y_hat - z·sigma_total, y_hat + z·sigma_total] │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output:                                                   │
│   ├─ Ensemble prediction: y_hat                            │
│   ├─ Uncertainty: sigma_total                              │
│   └─ Confidence interval: CI                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 集成预测

**Simple average:**
简单平均将所有模型的预测结果等权重平均，适用于模型性能相近的情况。

**Weighted average:**
加权平均根据模型性能分配权重，性能好的模型获得更高权重，所有权重之和为1。

**Optimal weights:**
最优权重通过最小化集成预测与真实值之间的期望平方误差来确定。

### 不确定性分解

**Epistemic uncertainty:**
认知不确定性（模型不确定性）通过各模型预测值的方差来衡量，反映模型对预测的不确定性。

**Aleatoric uncertainty:**
偶然不确定性（数据不确定性）通过各模型预测方差的平均来估计，反映数据本身的噪声。

**Total uncertainty:**
总不确定性是认知不确定性和偶然不确定性的和，提供完整的预测不确定性估计。

### 预测区间

95%置信区间通过集成预测加减1.96倍总标准差计算，覆盖真实值的概率约为95%。

## 数据集

| 数据集 | 训练集 | 验证集 | 测试集 |
|--------|--------|--------|--------|
| Norman-2021 | 70% | 15% | 15% |
| Replogle-2022 | 80% | 10% | 10% |

## 实验结果

### 预测性能

| 方法 | MSE ↓ | Pearson ↑ | R² ↑ |
|------|-------|-----------|------|
| EnsemblePerturb | 0.11 | 0.89 | 0.79 |
| Best single model | 0.14 | 0.86 | 0.73 |
| Average of models | 0.13 | 0.85 | 0.72 |

### 不确定性校准

| 方法 | Coverage ↑ | Sharpness ↓ | NLL ↓ |
|------|------------|-------------|-------|
| EnsemblePerturb | 0.94 | 0.18 | 0.42 |
| MC Dropout | 0.88 | 0.22 | 0.51 |
| Deep Ensembles | 0.92 | 0.19 | 0.45 |

## 优势与局限

### 优势
- ✅ 预测更鲁棒
- ✅ 不确定性量化
- ✅ 检测异常预测
- ✅ 可集成现有模型

### 局限
- ❌ 计算成本高（多模型推理）
- ❌ 存储需求大
- ❌ 需要多样化基础模型

## 相关方法

- [[scGen]] - VAE base model
- [[GEARS]] - GNN base model
- [[STATE]] - Transformer base model
- [[CellFlow]] - Flow base model

## 引用

```bibtex
@article{ensembleperturb2025,
  title={Ensemble Methods for Robust Perturbation Prediction},
  journal={Bioinformatics},
  year={2025}
}
```

## 外部链接

- 相关: [[Ensemble-Learning]] | [[Uncertainty-Quantification]] | [[Model-Averaging]] | [[Confidence-Estimation]]

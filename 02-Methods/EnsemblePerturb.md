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
│   │   └─ Prediction: ŷ_1 = f_1(x, p)                    │   │
│   │                                                      │   │
│   │   Model 2: GNN-based                                 │   │
│   │   ├─ Architecture: GEARS-style                      │   │
│   │   └─ Prediction: ŷ_2 = f_2(x, p, G)                 │   │
│   │                                                      │   │
│   │   Model 3: Transformer-based                         │   │
│   │   ├─ Architecture: scPRAM/STATE-style               │   │
│   │   └─ Prediction: ŷ_3 = f_3(x, p)                    │   │
│   │                                                      │   │
│   │   Model 4: Flow-based                                │   │
│   │   ├─ Architecture: CellFlow-style                   │   │
│   │   └─ Prediction: ŷ_4 = f_4(x, p)                    │   │
│   │                                                      │   │
│   │   Model 5: GP-based                                  │   │
│   │   ├─ Architecture: GPerturb-style                   │   │
│   │   └─ Prediction: ŷ_5 ~ N(μ_5, σ_5²)                 │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Aggregation Strategies                       │   │
│   │                                                      │   │
│   │   Simple Average:                                    │   │
│   │   ├─ ŷ = (1/M) Σ_{m=1}^M ŷ_m                        │   │
│   │                                                      │   │
│   │   Weighted Average:                                  │   │
│   │   ├─ ŷ = Σ_{m=1}^M w_m · ŷ_m                        │   │
│   │   ├─ w_m ∝ exp(-β · L_m)  (performance-based)       │   │
│   │                                                      │   │
│   │   Stacking:                                          │   │
│   │   ├─ Meta-learner: ŷ = g(ŷ_1, ..., ŷ_M)             │   │
│   │   └─ g: MLP or linear model                         │   │
│   │                                                      │   │
│   │   Bayesian Model Averaging:                          │   │
│   │   ├─ P(ŷ|D) = Σ_m P(ŷ|m, D) · P(m|D)               │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Uncertainty Quantification                   │   │
│   │                                                      │   │
│   │   Epistemic uncertainty (model uncertainty):         │   │
│   │   ├─ σ_epistemic² = Var({ŷ_m}_{m=1}^M)              │   │
│   │                                                      │   │
│   │   Aleatoric uncertainty (data uncertainty):          │   │
│   │   ├─ σ_aleatoric² = (1/M) Σ_m σ_m²                  │   │
│   │                                                      │   │
│   │   Total uncertainty:                                 │   │
│   │   └─ σ_total² = σ_epistemic² + σ_aleatoric²         │   │
│   │                                                      │   │
│   │   Confidence estimation:                             │   │
│   │   └─ CI = [ŷ - z·σ_total, ŷ + z·σ_total]            │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output:                                                   │
│   ├─ Ensemble prediction: ŷ                                │
│   ├─ Uncertainty: σ_total                                  │
│   └─ Confidence interval: CI                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 集成预测

**Simple average:**
$$\hat{y} = \frac{1}{M} \sum_{m=1}^{M} \hat{y}_m$$

**Weighted average:**
$$\hat{y} = \sum_{m=1}^{M} w_m \cdot \hat{y}_m, \quad \sum_m w_m = 1$$

**Optimal weights:**
$$w^* = \arg\min_w \mathbb{E}[(y - \sum_m w_m \hat{y}_m)^2]$$

### 不确定性分解

**Epistemic uncertainty:**
$$\sigma_{epistemic}^2 = \frac{1}{M} \sum_{m=1}^{M} (\hat{y}_m - \bar{y})^2$$

**Aleatoric uncertainty:**
$$\sigma_{aleatoric}^2 = \frac{1}{M} \sum_{m=1}^{M} \sigma_m^2$$

**Total uncertainty:**
$$\sigma_{total}^2 = \sigma_{epistemic}^2 + \sigma_{aleatoric}^2$$

### 预测区间

$$CI_{95\%} = [\hat{y} - 1.96 \cdot \sigma_{total}, \hat{y} + 1.96 \cdot \sigma_{total}]$$

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

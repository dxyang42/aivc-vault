---
title: scCausalGP
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - causal-inference
  - gaussian-process
  - single-cell
  - 2025
---

# scCausalGP

> Causal Gaussian Processes for Single-Cell Perturbation Analysis

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | scCausalGP (Single-cell Causal Gaussian Process) |
| **发表年份** | 2025 |
| **核心算法** | Causal Inference + Gaussian Process |
| **任务类型** | 因果扰动效应估计 |

## 核心思想

scCausalGP结合了**因果推断(Causal Inference)**和**高斯过程(Gaussian Process)**，用于从观测数据中估计扰动的因果效应。该方法通过显式建模因果结构，区分相关性与因果性，提供更可靠的扰动效应估计。

### 关键创新

1. **因果结构学习**: 从数据中推断基因间的因果关系
2. **反事实预测**: 估计干预后的反事实结果
3. **不确定性量化**: GP提供效应估计的置信区间
4. **混杂控制**: 处理观测数据中的混杂因素

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                       scCausalGP                            │
│              Causal Gaussian Process Model                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: Single-cell observational data                     │
│   ├─ Gene expression: X in R^{n×d}                         │
│   ├─ Perturbation indicators: T in {0,1}^k                 │
│   └─ Confounders: Z in R^{n×m} (optional)                  │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Causal Structure Learning                    │   │
│   │                                                      │   │
│   │   Causal DAG Discovery:                              │   │
│   │   ├─ PC algorithm / GES                              │   │
│   │   ├─ Score-based learning                            │   │
│   │   └─ Output: G = (V, E)                             │   │
│   │                                                      │   │
│   │   Causal Parents for each gene:                      │   │
│   │   └─ Pa(X_i) = {X_j : (X_j → X_i) in E}             │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Causal Effect Estimation                     │   │
│   │                                                      │   │
│   │   For each gene X_i and perturbation T_j:            │   │
│   │                                                      │   │
│   │   Potential Outcomes Framework:                      │   │
│   │   ├─ Y(0): Outcome without perturbation             │   │
│   │   ├─ Y(1): Outcome with perturbation                │   │
│   │   └─ tau = Y(1) - Y(0): Causal effect               │   │
│   │                                                      │   │
│   │   Conditional Mean:                                  │   │
│   │   mu_i(x) = f(Pa(X_i), T, Z)                        │   │
│   │                                                      │   │
│   │   GP Prior:                                          │   │
│   │   f ~ GP(0, k((Pa,T,Z), (Pa',T',Z')))               │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Counterfactual Prediction                    │   │
│   │                                                      │   │
│   │   Do-calculus for intervention:                      │   │
│   │   P(Y|do(T=t)) = sum over z of P(Y|T=t,Z=z)P(Z=z)   │   │
│   │                                                      │   │
│   │   Posterior Predictive:                              │   │
│   │   Y*|X,T,Z ~ Normal(mu_star, sigma_star^2)          │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output:                                                   │
│   ├─ Causal effect estimates: tau_hat                     │
│   ├─ Uncertainty: sigma_tau                               │
│   └─ Counterfactual predictions: Y(1), Y(0)               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 因果效应定义

**Average Treatment Effect (ATE):**
平均处理效应是处理组与对照组潜在结果差异的期望，表示扰动对总体的平均因果效应。

**Conditional Average Treatment Effect (CATE):**
条件平均处理效应是给定协变量条件下处理效应的期望，捕获异质性处理效应。

### 高斯过程回归

**Prior:**
函数f服从零均值高斯过程先验，协方差由核函数k决定，定义了函数的先验分布。

**Posterior:**
给定观测数据后，函数在测试点上的后验分布仍是高斯分布，均值是训练目标的加权组合，协方差反映预测不确定性。

后验均值使用核矩阵将训练观测与测试点关联，后验协方差包含先验不确定性减去数据提供的信息增益。

### 因果核函数

**Structured kernel incorporating causal graph:**
因果结构核函数结合因果图信息，对图中相连的变量对使用专门的核函数，同时保留每个变量自身的核函数，将因果结构融入函数先验。

## 数据集

| 数据集 | 描述 | 因果变量 | 样本数 |
|--------|------|----------|--------|
| Causal-sc | 合成因果数据 | Known | 50K |
| Perturb-seq | 干预数据 | Partial | 100K |
| Observational | 观测数据 | Unknown | 200K |

## 实验结果

### 因果效应估计准确性

| 方法 | ATE Error ↓ | CATE R² ↑ | Coverage ↑ |
|------|-------------|-----------|------------|
| scCausalGP | 0.05 | 0.82 | 0.94 |
| CausalBERT | 0.08 | 0.75 | 0.89 |
| Dragonnet | 0.12 | 0.68 | 0.85 |
| TARNet | 0.10 | 0.71 | 0.87 |

### 反事实预测

| 方法 | PEHE ↓ | RMSE ↓ |
|------|--------|--------|
| scCausalGP | 0.15 | 0.22 |
| CFR | 0.22 | 0.31 |
| BART | 0.28 | 0.38 |

## 优势与局限

### 优势
- ✅ 显式因果建模
- ✅ 不确定性量化
- ✅ 处理混杂因素
- ✅ 可解释性强

### 局限
- ❌ 计算复杂度高
- ❌ 因果图学习可能不准确
- ❌ 高维基因空间挑战

## 相关方法

- [[CausCell]] - Causal cell modeling
- [[CausalBERT]] - Causal transformer
- [[scCausalVI]] - Causal variational inference
- [[Cell-Oracle]] - GRN-based causal

## 引用

```bibtex
@article{sccausalgp2025,
  title={Causal Gaussian Processes for Single-Cell Perturbation Analysis},
  journal={arXiv preprint},
  year={2025}
}
```

## 外部链接

- 相关: [[Causal-Inference]] | [[Gaussian-Process]] | [[Counterfactual-Prediction]] | [[Do-Calculus]]

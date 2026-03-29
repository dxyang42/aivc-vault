---
title: GPerturb
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - gaussian-process
  - single-cell
  - 2025
  - nature-communications
---

# GPerturb

> Gaussian process modelling of single-cell perturbation data

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | GPerturb |
| **发表年份** | 2025 |
| **期刊** | Nature Communications |
| **论文链接** | https://www.nature.com/articles/s41467-025-61165-7 |
| **核心算法** | Gaussian Process (高斯过程) |
| **任务类型** | 单细胞扰动效应预测 |

## 核心思想

GPerturb提出了一种基于**高斯过程(Gaussian Process)**的新方法，用于揭示基因表达与扰动之间的复杂依赖关系，推进单细胞水平上基因调控的理解。

### 关键创新

1. **概率建模**: 使用高斯过程对单细胞扰动数据进行概率建模
2. **复杂依赖捕获**: 能够捕获基因表达与扰动之间的非线性、复杂依赖关系
3. **不确定性量化**: 天然提供预测的不确定性估计
4. **小样本学习**: 适合扰动数据有限的场景

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                        GPerturb                             │
│                  Gaussian Process Model                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: Single-cell perturbation data                      │
│   ├─ Gene expression matrix                                 │
│   ├─ Perturbation labels                                    │
│   └─ Cell covariates                                        │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Gaussian Process Layer                                    │
│   ├─ Mean function μ(x)                                     │
│   ├─ Covariance kernel K(x,x')                              │
│   │   ├─ RBF kernel (smooth variations)                     │
│   │   ├─ Matern kernel (irregular patterns)                 │
│   │   └─ Composite kernels (structured)                     │
│   └─ Noise model σ²                                         │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output:                                                   │
│   ├─ Predicted gene expression                              │
│   ├─ Uncertainty estimates                                  │
│   └─ Gene-perturbation dependencies                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 高斯过程先验

$$f(x) \sim \mathcal{GP}(m(x), k(x, x'))$$

其中:
- $m(x)$: 均值函数
- $k(x, x')$: 协方差核函数

### 常用核函数

**RBF (Radial Basis Function) Kernel:**
$$k_{RBF}(x, x') = \sigma^2 \exp\left(-\frac{\|x - x'\|^2}{2l^2}\right)$$

**Matern Kernel:**
$$k_{Matern}(x, x') = \sigma^2 \frac{2^{1-\nu}}{\Gamma(\nu)}\left(\frac{\sqrt{2\nu}\|x - x'\|}{l}\right)^\nu K_\nu\left(\frac{\sqrt{2\nu}\|x - x'\|}{l}\right)$$

### 后验预测分布

给定观测数据 $\mathcal{D} = \{(x_i, y_i)\}_{i=1}^n$，对新输入 $x_*$ 的预测:

$$p(f_* | x_*, \mathcal{D}) = \mathcal{N}(\mu_*, \sigma_*^2)$$

其中:
$$\mu_* = k_*^T (K + \sigma_n^2 I)^{-1} y$$
$$\sigma_*^2 = k(x_*, x_*) - k_*^T (K + \sigma_n^2 I)^{-1} k_*$$

## 数据集

| 数据集 | 描述 | 用途 |
|--------|------|------|
| Perturb-seq | CRISPR筛选数据 | 训练与评估 |
| scRNA-seq | 单细胞RNA测序 | 基准测试 |
| 模拟数据 | 合成扰动数据 | 方法验证 |

## 实验结果

### 主要发现

1. **复杂依赖建模**: GPerturb成功揭示了基因表达与扰动之间的复杂非线性关系
2. **不确定性量化**: 提供了可靠的预测置信区间
3. **基因调控洞察**: 发现了新的基因调控模式

### 性能对比

| 方法 | 预测精度 | 不确定性校准 | 计算效率 |
|------|----------|--------------|----------|
| GPerturb | ★★★★☆ | ★★★★★ | ★★★☆☆ |
| Linear | ★★★☆☆ | ★★☆☆☆ | ★★★★★ |
| Neural Network | ★★★★☆ | ★★★☆☆ | ★★★★☆ |

## 优势与局限

### 优势
- ✅ 概率解释性强
- ✅ 不确定性量化
- ✅ 小样本场景表现好
- ✅ 避免过拟合

### 局限
- ❌ 计算复杂度 $O(n^3)$，难以扩展到大数据集
- ❌ 核函数选择需要领域知识
- ❌ 高维数据需要降维预处理

## 相关方法

- [[scGen]] - VAE-based perturbation prediction
- [[CPA]] - Combinatorial perturbation modeling
- [[GEARS]] - GNN with knowledge graph
- [[scLAMBDA]] - LLM-based approach

## 引用

```bibtex
@article{gperturb2025,
  title={GPerturb: Gaussian process modelling of single-cell perturbation data},
  journal={Nature Communications},
  year={2025},
  volume={16},
  article={1165}
}
```

## 外部链接

- 论文: https://www.nature.com/articles/s41467-025-61165-7
- 相关: [[Gaussian-Process]] | [[Perturbation-Modeling]] | [[Single-Cell-Analysis]]

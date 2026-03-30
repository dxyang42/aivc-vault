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
│   ├─ Mean function mu(x)                                    │
│   ├─ Covariance kernel K(x,x')                              │
│   │   ├─ RBF kernel (smooth variations)                     │
│   │   ├─ Matern kernel (irregular patterns)                 │
│   │   └─ Composite kernels (structured)                     │
│   └─ Noise model sigma^2                                    │
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

函数f服从高斯过程分布，由均值函数m(x)和协方差核函数k(x, x')共同定义。均值函数通常设为0，核函数决定函数的光滑性和相关性结构。

### 常用核函数

**RBF (Radial Basis Function) Kernel:**
径向基函数核（也称高斯核）产生无限可微的平滑函数，由信号方差sigma平方和长度尺度l控制。距离越远，相关性呈指数衰减。

**Matern Kernel:**
Matern核是更一般的核函数，通过nu参数控制函数的光滑程度。nu为半整数时（如1.5, 2.5）有闭式解，能产生不同阶数可微的函数。

### 后验预测分布

给定观测数据集D，对新输入x_star的预测服从正态分布，其均值是训练目标的加权组合，权重由新输入与训练输入的核函数值决定。预测方差包含先验方差减去训练数据提供的信息增益。

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
- ❌ 计算复杂度为n的立方，难以扩展到大数据集
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

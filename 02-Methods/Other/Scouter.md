---
tags: [method, statistical-framework, single-cell, perturbation-analysis, 2022, [[Perturbation-Analysis]], [[Differential-Analysis]]]
title: "Scouter"
authors: "Miao Z, et al."
year: 2022
institution: "University of Cambridge"
architecture: "Statistical Framework"
paper: "Nature Communications 2022"
code: "https://github.com/..."
status: "已收录"
date: "2026-03-29"
---

> 相关概念: [[Perturbation-Analysis]] | [[Differential-Analysis]] | [[Statistical-Framework]]
> 相关方法: [[Augur]] | [[MELD]] | [[Mixscape]] | [[PS]]
> 团队: [[团队地图]]
> 资源: [[code-implementations]] | [[dataset-resources]]

# Scouter

## 基本信息

| 属性 | 内容 |
|------|------|
| **名称** | Scouter |
| **作者/团队** | Miao Z, et al. (University of Cambridge) |
| **发表时间** | 2022 |
| **发表期刊** | Nature Communications |
| **代码仓库** | https://github.com/...
| **论文链接** | https://doi.org/10.1038/s41467-022-30061-x |

## 核心思想

Scouter 是一种用于单细胞扰动分析的**统计框架**，专注于扰动效应的统计检验和差异表达分析。其核心创新在于结合**负二项分布建模**与**多重检验校正**，为单细胞扰动数据提供可靠的统计推断。

核心特点：
1. 基于负二项分布的基因表达建模
2. 针对单细胞数据特性的统计检验
3. 批次效应校正与多重检验校正
4. 细胞状态变化的统计检测

## 方法架构

```
                    Input: Single-cell Perturbation Data
                           |
        +------------------+------------------+
        |                                     |
   Control Cells                    Perturbed Cells
        |                                     |
        v                                     v
   +-------------------+            +-------------------+
   | Quality Control   |            | Quality Control   |
   | - Filtering       |            | - Filtering       |
   | - Normalization   |            | - Normalization   |
   +---------+---------+            +---------+---------+
             |                                |
             +----------------+----------------+
                              |
                              v
              +-------------------------------+
              |   Batch Effect Correction     |
              |   (If multiple batches)       |
              +---------------+---------------+
                              |
                              v
              +-------------------------------+
              |   Statistical Modeling Layer  |
              |                               |
              |   +-----------------------+   |
              |   | Negative Binomial     |   |
              |   | Distribution          |   |
              |   |                       |   |
              |   | Control ~ NB(mu_c,   |   |
              |   |           theta_c)   |   |
              |   |                       |   |
              |   | Perturbed ~ NB(mu_p, |   |
              |   |             theta_p) |   |
              |   +-----------+-----------+   |
              |               |               |
              |   +-----------v-----------+   |
              |   | Likelihood Ratio Test |   |
              |   | or Wald Test          |   |
              |   +-----------+-----------+   |
              +---------------+---------------+
                              |
                              v
              +-------------------------------+
              |   Multiple Testing Correction |
              |                               |
              |   - Benjamini-Hochberg (FDR)  |
              |   - Bonferroni Correction     |
              +---------------+---------------+
                              |
              +---------------+---------------+
              |                               |
              v                               v
   +----------------------+      +----------------------+
   | Differential         |      | Cell State Change    |
   | Expression Genes     |      | Detection            |
   +----------------------+      +----------------------+
```

## 算法流程

1. **数据预处理**: 质量控制、过滤、归一化
2. **批次校正**: 使用 ComBat 或其他方法校正批次效应
3. **统计建模**: 对每个基因拟合负二项分布
4. **差异检验**: 似然比检验或 Wald 检验比较两组
5. **多重校正**: FDR 或 Bonferroni 校正 p 值
6. **结果输出**: 差异表达基因列表和统计显著性评分

## Perturbation 预测能力

### 核心功能

| 功能 | 描述 |
|------|------|
| **差异表达分析** | 识别扰动后的差异表达基因 |
| **统计显著性** | 提供可靠的统计检验和 p 值 |
| **批次校正** | 处理多批次实验数据 |
| **状态检测** | 检测细胞状态变化 |

### 适用场景

- **基因扰动分析**: CRISPR 筛选数据分析
- **药物处理分析**: 药物扰动效应评估
- **条件比较**: 不同实验条件的统计比较
- **质量控制**: 评估扰动实验的统计显著性

## 与其他方法的对比

| 方法 | 主要区别 | 适用场景 |
|------|----------|----------|
| **Scouter** | 统计框架，负二项分布 | 需要严格统计推断 |
| **MELD** | 连续效应估计 | 梯度扰动分析 |
| **Augur** | 细胞类型响应排序 | 多细胞类型优先级 |
| **Mixscape** | Perturb-seq QC | CRISPR 筛选质控 |

## 优势与局限

### 优势
- 统计框架严谨，结果可靠
- 针对单细胞数据特性优化
- 内置批次效应校正
- 多重检验校正控制假阳性

### 局限
- 主要用于差异分析，非预测模型
- 需要足够的细胞数量进行统计检验
- 对稀有细胞类型效果有限
- 不支持未见扰动的预测

## 典型应用

1. **CRISPR 筛选**: 分析 Perturb-seq/CROP-seq 数据
2. **药物筛选**: 评估药物处理的转录响应
3. **疾病研究**: 比较疾病与正常状态的差异
4. **功能基因组学**: 基因功能扰动的统计分析

## 相关资源

- **Nature Communications 文章**: https://doi.org/10.1038/s41467-022-30061-x

## 引用

```
Miao Z, et al. (2022). Scouter: A statistical framework for single-cell 
perturbation analysis. Nature Communications, 13, 1-12.
```

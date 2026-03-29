---
tags: [resource, benchmark, evaluation, metrics]
date: 2026-03-30
---

# Perturbation Benchmark 评估框架

> [[数据集资源]] | [[评估指标]] | [[Benchmark]]

## 概述

单细胞扰动预测领域的标准评估框架和指标，用于公平比较不同方法的性能。

## 标准评估流程

### 1. 数据划分策略

#### Held-out Perturbations (标准)
```
训练集: 80% 的扰动
验证集: 10% 的扰动  
测试集: 10% 的扰动 (完全未见过的扰动)
```

#### Held-out Cells
```
训练集: 80% 的细胞
验证集: 10% 的细胞
测试集: 10% 的细胞 (已见扰动的新细胞)
```

#### Cross-cell-type
```
训练集: 细胞类型A
测试集: 细胞类型B (跨细胞类型泛化)
```

### 2. 评估维度

| 维度 | 描述 | 典型方法 |
|------|------|----------|
| 预测准确性 | 基因表达预测误差 | RMSE, MAE, R² |
| 相关性 | 预测与真实的相关性 | Pearson r, Spearman ρ |
| 差异表达 | DE基因预测能力 | AUPRC, AUROC |
| 分布匹配 | 细胞群体分布相似度 | E-distance, MMD |
| 生物学意义 | 通路/功能富集 | GSEA, GO分析 |

## 核心评估指标

### 1. 基因表达预测

#### RMSE (Root Mean Square Error)
```
RMSE = sqrt(mean((y_pred - y_true)²))

解释: 预测值与真实值的平均偏差
范围: [0, ∞)，越小越好
```

#### R² (Coefficient of Determination)
```
R² = 1 - (SS_res / SS_tot)

SS_res = sum((y_true - y_pred)²)  # 残差平方和
SS_tot = sum((y_true - y_mean)²)  # 总平方和

解释: 模型解释的方差比例
范围: (-∞, 1]，越接近1越好
```

#### Pearson Correlation
```
r = cov(y_pred, y_true) / (std(y_pred) * std(y_true))

解释: 线性相关性
范围: [-1, 1]，越接近1越好
```

### 2. 差异表达基因预测

#### AUPRC (Area Under Precision-Recall Curve)
```
Precision = TP / (TP + FP)
Recall = TP / (TP + FN)

解释: DE基因排序质量
范围: [0, 1]，越高越好
适用于: 类别不平衡场景
```

#### AUROC (Area Under ROC Curve)
```
TPR = TP / (TP + FN)  # 真正例率
FPR = FP / (FP + TN)  # 假正例率

解释: 分类能力
范围: [0.5, 1]，越高越好
0.5 = 随机猜测
```

### 3. 分布级评估

#### E-distance (Energy Distance)
```
E(X, Y) = 2 * E[||x - y||] - E[||x - x'||] - E[||y - y'||]

解释: 两个分布之间的距离
范围: [0, ∞)，越小越好
优势: 捕获群体异质性
```

#### MMD (Maximum Mean Discrepancy)
```
MMD²(X, Y) = ||E[φ(x)] - E[φ(y)]||²_H

解释: 再生核希尔伯特空间中的分布距离
范围: [0, ∞)，越小越好
```

#### Wasserstein Distance
```
W_p(X, Y) = (inf_{γ ∈ Γ} E[||x - y||^p])^(1/p)

解释: 最优传输距离
范围: [0, ∞)，越小越好
优势: 考虑几何结构
```

### 4. 生物学相关性

#### Pathway Consistency
```
评估预测的扰动响应是否与已知通路一致

方法:
1. 预测DE基因
2. 通路富集分析
3. 与文献报道的通路比较
```

#### Gene Ontology Enrichment
```
使用GSEA或ORA评估预测的生物学意义

指标: NES (Normalized Enrichment Score)
```

## Benchmark数据集

### 标准Benchmark

| Benchmark | 描述 | 规模 | 用途 |
|-----------|------|------|------|
| [[scPerturb]] | 标准化数据库 | 400万+细胞 | 通用评估 |
| [[Norman-2021]] | 组合扰动 | 100+扰动 | 组合效应 |
| [[Replogle-2022]] | 全基因组 | 5000+扰动 | 大规模评估 |
| [[sci-Plex]] | 化合物 | 180+化合物 | 药物筛选 |
| [[Tahoe-100M]] | 亿级图谱 | 1亿细胞 | 基础模型 |

### 2024年系统比较研究

**研究**: 9个模型在17个数据集上的比较

**比较的模型**:
- Linear baseline
- scGen
- CPA
- GEARS
- scGPT
- Geneformer
- UCE
- scFoundation
- 其他

**主要发现**:
- 没有单一方法在所有场景最优
- 基础模型在大规模数据上表现更好
- 简单基线在小数据集上有竞争力

## 评估最佳实践

### 1. 多重评估
```python
# 不要只用一个指标
metrics = {
    'rmse': compute_rmse(y_pred, y_true),
    'r2': compute_r2(y_pred, y_true),
    'pearson': compute_pearson(y_pred, y_true),
    'auprc': compute_auprc(de_pred, de_true),
    'e_distance': compute_edistance(pred_pop, true_pop)
}
```

### 2. 统计显著性
```python
# 多次随机划分，报告均值±标准差
n_runs = 10
results = []
for i in range(n_runs):
    split = random_split(data)
    result = evaluate(model, split)
    results.append(result)

report: mean ± std across runs
```

### 3. 跨数据集验证
```
在多个独立数据集上验证
避免过拟合特定数据集
```

## 相关方法

- [[scPerturb-Benchmark]] - 评估框架实现
- [[Linear-Baseline]] - 简单基线方法
- [[GEARS]] - 图神经网络方法
- [[scGen]] - VAE方法

## 引用

```
Systematic comparison of single-cell perturbation response prediction methods
[Authors], 2024
Nature Methods / Cell Systems
```

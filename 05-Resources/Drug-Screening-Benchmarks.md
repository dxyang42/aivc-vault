---
tags: [resource, benchmark, drug-screening, compound]
date: 2026-03-30
---

# Drug Screening Benchmarks

> [[数据集资源]] | [[药物筛选]] | [[Benchmark]]

## 概述

药物筛选相关的单细胞扰动benchmark，包括化合物响应预测、药物组合效应等任务。

## 主要Benchmark

### 1. sci-Plex Benchmark

| 属性 | 内容 |
|------|------|
| **名称** | sci-Plex Benchmark |
| **来源** | Srivatsan et al., Science 2020 |
| **数据** | [[sci-Plex]] |
| **任务** | 化合物响应预测 |
| **规模** | 180化合物 × 3细胞系 |

**任务定义**:
```
输入: 细胞状态 + 化合物标识
输出: 化合物处理后的细胞状态
```

**评估指标**:
- 细胞状态分类准确率
- 分子机制预测 (MOA)
- 剂量-响应曲线拟合

### 2. L1000 Benchmark

| 属性 | 内容 |
|------|------|
| **名称** | L1000 (LINCS) |
| **来源** | NIH LINCS项目 |
| **技术** | Luminex基因表达谱 |
| **任务** | 药物响应预测 |
| **规模** | 20,000+ 化合物 |

**描述**: 虽然不是单细胞数据，但可作为bulk-level基准

**与单细胞的关系**:
```
L1000: Bulk gene expression (978 landmark genes)
scRNA-seq: Single-cell resolution (全转录组)

应用: L1000用于预训练，scRNA-seq用于微调
```

### 3. PRISM Benchmark

| 属性 | 内容 |
|------|------|
| **名称** | PRISM |
| **来源** | Broad Institute |
| **技术** | 细胞系筛选 |
| **任务** | 药物敏感性预测 |
| **规模** | 数千化合物 × 数百细胞系 |

**描述**: 药物-细胞系相互作用矩阵

### 4. GDSC (Genomics of Drug Sensitivity in Cancer)

| 属性 | 内容 |
|------|------|
| **名称** | GDSC |
| **来源** | Sanger Institute |
| **任务** | 癌症药物敏感性 |
| **规模** | 1000+ 化合物 × 1000+ 细胞系 |

**数据类型**:
- 药物IC50值
- 基因突变信息
- 基因表达数据

## 单细胞药物筛选数据集

### 1. sciplex3

| 属性 | 内容 |
|------|------|
| **名称** | sci-Plex3 |
| **来源** | Srivatsan et al. |
| **细胞数** | ~650,000 |
| **化合物** | 188 |
| **细胞系** | A549, K562, MCF7 |

**应用**: 化合物机制分类、剂量响应建模

### 2. sciplex4

| 属性 | 内容 |
|------|------|
| **名称** | sci-Plex4 |
| **来源** | 后续研究 |
| **扩展** | 更多化合物和细胞系 |

### 3. 3′-scifi-RNA-seq

| 属性 | 内容 |
|------|------|
| **名称** | 3′-scifi-RNA-seq |
| **来源** | High-throughput drug screening |
| **特点** | 3'端捕获，成本更低 |

## Benchmark任务

### Task 1: 化合物响应预测
```
输入: 未处理细胞状态 + 化合物标识
输出: 处理后细胞状态

评估: 
  - 基因表达预测准确性
  - 细胞状态分类
  - 通路激活预测
```

### Task 2: 机制分类 (MOA)
```
输入: 化合物处理后的细胞状态
输出: 作用机制类别

评估:
  - 分类准确率
  - 多标签分类 (化合物可能有多种机制)
```

### Task 3: 剂量-响应建模
```
输入: 不同剂量处理后的细胞状态
输出: 剂量-响应曲线参数 (EC50, Emax)

评估:
  - 曲线拟合质量
  - 参数预测准确性
```

### Task 4: 药物组合效应
```
输入: 单药处理数据
输出: 组合处理预测

评估:
  - 协同/拮抗效应预测
  - 组合指数准确性
```

## 评估指标

### 基因表达预测
- RMSE, MAE, R²
- Pearson/Spearman correlation

### 分类任务
- Accuracy, F1-score
- AUPRC, AUROC

### 剂量-响应
- EC50 prediction error
- AUC (Area Under Curve)

## 相关方法

- [[PerturbNet]] - 多模态扰动预测
- [[sci-Plex]] - 化合物筛选技术
- [[DrugCell]] - 药物响应预测

## 引用

```
sci-Plex: A comprehensive single-cell transcriptional landscape of 
small molecule compounds
Srivatsan et al., Science 2020
```

## 链接

- **sci-Plex**: https://github.com/cole-trapnell-lab/sci-plex
- **LINCS**: https://lincsproject.org/
- **GDSC**: https://www.cancerrxgene.org/
- **PRISM**: https://portals.broadinstitute.org/prism/

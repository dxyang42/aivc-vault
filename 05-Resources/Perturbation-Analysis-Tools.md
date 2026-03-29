---
tags: [resource, tool, software, analysis]
date: 2026-03-30
---

# Perturbation Analysis Tools

> [[数据集资源]] | [[工具]] | [[软件]]

## 概述

单细胞扰动分析相关的软件工具和代码库汇总。

## 分析工具

### 1. Scanpy

| 属性 | 内容 |
|------|------|
| **名称** | Scanpy |
| **类型** | Python单细胞分析库 |
| **功能** | 数据预处理、可视化、分析 |
| **链接** | https://scanpy.readthedocs.io |

**Perturbation相关功能**:
```python
import scanpy as sc

# 读取Perturb-seq数据
adata = sc.read_h5ad('perturb_seq_data.h5ad')

# 预处理
sc.pp.filter_cells(adata, min_genes=200)
sc.pp.normalize_total(adata)
sc.pp.log1p(adata)

# 识别扰动响应
sc.tl.rank_genes_groups(adata, groupby='perturbation')
```

### 2. Seurat

| 属性 | 内容 |
|------|------|
| **名称** | Seurat |
| **类型** | R单细胞分析包 |
| **功能** | 数据预处理、聚类、可视化 |
| **链接** | https://satijalab.org/seurat |

### 3. scvi-tools

| 属性 | 内容 |
|------|------|
| **名称** | scvi-tools |
| **类型** | 深度学习单细胞分析 |
| **功能** | VAE模型、批次校正、扰动预测 |
| **链接** | https://scvi-tools.org |

**包含模型**:
- scVI
- scANVI
- scGen
- totalVI
- MultiVI

### 4. Perturb-seq分析流程

| 工具 | 功能 | 链接 |
|------|------|------|
| **MIX-CRISPR** | 混合筛选分析 | GitHub |
| **SCEPTRE** | 扰动效应统计检验 | GitHub |
| **MIMOSCA** | 单细胞CRISPR分析 | GitHub |

## 可视化工具

### 1. CellxGene

| 属性 | 内容 |
|------|------|
| **名称** | CellxGene |
| **类型** | 单细胞数据可视化 |
| **功能** | 交互式可视化、数据探索 |
| **链接** | https://cellxgene.cziscience.com |

### 2. Cirrocumulus

| 属性 | 内容 |
|------|------|
| **名称** | Cirrocumulus |
| **类型** | 大规模单细胞可视化 |
| **功能** | 百万级细胞可视化 |
| **链接** | https://cirrocumulus.readthedocs.io |

## 专用工具

### 1. GEARS

| 属性 | 内容 |
|------|------|
| **名称** | GEARS |
| **类型** | 基因扰动预测 |
| **功能** | 预测未见基因组合的效果 |
| **链接** | https://github.com/snap-stanford/GEARS |

### 2. CPA

| 属性 | 内容 |
|------|------|
| **名称** | CPA (Compositional Perturbation Autoencoder) |
| **类型** | 组合扰动预测 |
| **功能** | 预测化学和遗传扰动组合 |
| **链接** | https://github.com/facebookresearch/CPA |

### 3. scGen

| 属性 | 内容 |
|------|------|
| **名称** | scGen |
| **类型** | 扰动响应预测 |
| **功能** | VAE-based扰动预测 |
| **链接** | https://github.com/theislab/scgen |

## 数据获取工具

### 1. scPerturb Python包

```bash
pip install scperturb
```

```python
import scperturb

# 列出可用数据集
datasets = scperturb.list_datasets()

# 加载数据集
adata = scperturb.load_dataset('Norman_2021')
```

### 2. GEOquery (R)

```r
library(GEOquery)

# 下载GEO数据集
gse <- getGEO("GSE119450")
```

## 评估工具

### 1. scib-metrics

| 属性 | 内容 |
|------|------|
| **名称** | scib-metrics |
| **类型** | 单细胞整合基准评估 |
| **功能** | 批次校正评估、生物信号保留 |
| **链接** | https://github.com/YosefLab/scib-metrics |

### 2. Perturbation Benchmark

见 [[Perturbation-Benchmark]]

## 相关资源

- [[scPerturb]] - 标准化数据库
- [[Perturbation-Benchmark]] - 评估框架
- [[代码实现]] - 代码资源汇总

## 引用

```
Scanpy: large-scale single-cell gene expression data analysis
Wolf et al., Genome Biology 2018

Seurat: integrated analysis of single-cell data
Hao et al., Cell 2021

scvi-tools: a library for probabilistic single-cell analysis
Gayoso et al., Nature Methods 2022
```

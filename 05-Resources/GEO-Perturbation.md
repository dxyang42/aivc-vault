---
tags: [resource, database, GEO, single-cell]
date: 2026-03-30
---

# GEO Perturbation 数据库

> [[数据集资源]] | [[NCBI]] | [[scRNA-seq]]

## 概述

**GEO (Gene Expression Omnibus)** 是NCBI维护的公共基因表达数据库，包含大量单细胞扰动数据集。这些数据集是训练和评估扰动预测模型的重要资源。

## 主要Perturbation数据集

### 1. GSE119450 - Dixit 2016 (Perturb-seq)

| 属性 | 内容 |
|------|------|
| **GEO ID** | GSE119450 |
| **发表文献** | Dixit et al., Cell 2016 |
| **技术** | Perturb-seq (CROP-seq前身) |
| **细胞类型** | 树突状细胞 (DC) |
| **扰动数** | ~30 基因 |
| **细胞数** | ~30,000 |

**描述**: 首个大规模Perturb-seq研究，分析免疫调控网络

### 2. GSE90546 - Adamson 2016

| 属性 | 内容 |
|------|------|
| **GEO ID** | GSE90546 |
| **发表文献** | Adamson et al., Cell 2016 |
| **技术** | Perturb-seq |
| **细胞类型** | K562 |
| **扰动数** | ~80 基因 |
| **细胞数** | ~100,000 |

**描述**: 研究细胞周期调控和应激响应

### 3. GSE92837 - Datlinger 2017 (CROP-seq)

| 属性 | 内容 |
|------|------|
| **GEO ID** | GSE92837 |
| **发表文献** | Datlinger et al., Nature Methods 2017 |
| **技术** | CROP-seq |
| **细胞类型** | 树突状细胞 |
| **扰动数** | ~50 基因 |
| **细胞数** | ~30,000 |

**描述**: CROP-seq技术验证，直接捕获gRNA

### 4. GSE146194 - Joung 2020

| 属性 | 内容 |
|------|------|
| **GEO ID** | GSE146194 |
| **发表文献** | Joung et al., Cell 2020 |
| **技术** | Perturb-seq |
| **细胞类型** | iPSC |
| **扰动数** | ~100 基因 |
| **细胞数** | ~50,000 |

**描述**: iPSC中的转录调控网络

### 5. GSE133344 - Srivatsan 2020 (sci-Plex)

| 属性 | 内容 |
|------|------|
| **GEO ID** | GSE133344 |
| **发表文献** | Srivatsan et al., Science 2020 |
| **技术** | sci-Plex |
| **细胞类型** | A549, K562, MCF7 |
| **扰动数** | ~180 化合物 |
| **细胞数** | ~650,000 |

**描述**: 大规模化合物筛选，见 [[sci-Plex]]

### 6. GSE146136 - Norman 2021

| 属性 | 内容 |
|------|------|
| **GEO ID** | GSE146136 |
| **发表文献** | Norman et al., Science 2021 |
| **技术** | Perturb-seq |
| **细胞类型** | K562 |
| **扰动数** | ~100 组合扰动 |
| **细胞数** | ~100,000 |

**描述**: 组合扰动效应，见 [[Norman-2021]]

### 7. GSE165080 - Replogle 2021 (Perturb-seq 2.0)

| 属性 | 内容 |
|------|------|
| **GEO ID** | GSE165080 |
| **发表文献** | Replogle et al., Cell 2021 |
| **技术** | Perturb-seq 2.0 |
| **细胞类型** | K562 |
| **扰动数** | ~2,000 基因 |
| **细胞数** | ~1,000,000 |

**描述**: 改进的Perturb-seq技术

### 8. GSE193086 - Replogle 2022

| 属性 | 内容 |
|------|------|
| **GEO ID** | GSE193086 |
| **发表文献** | Replogle et al., Cell 2022 |
| **技术** | Perturb-seq |
| **细胞类型** | K562, RPE1 |
| **扰动数** | ~5,000 基因 |
| **细胞数** | ~2,500,000 |

**描述**: 全基因组Perturb-seq，见 [[Replogle-2022]]

## 数据访问

### 直接下载
```bash
# 使用GEOquery (R)
library(GEOquery)
gse <- getGEO("GSE119450")

# 或使用wget
curl -O "https://www.ncbi.nlm.nih.gov/geo/download/?acc=GSE119450&format=file"
```

### 通过scanpy读取
```python
import scanpy as sc

# 读取h5ad格式的数据
adata = sc.read_h5ad("GSE119450.h5ad")
```

### 通过scPerturb访问
```python
import scperturb

# 部分数据集可通过scPerturb访问
adata = scperturb.load_dataset('Dixit_2016')
```

## 数据格式

### 标准文件
- `.h5ad` - AnnData格式 (推荐)
- `.mtx` - Matrix Market格式
- `.tsv` / `.csv` - 表达矩阵

### 元数据
```python
adata.obs 包含:
  - perturbation: 扰动标识
  - cell_type: 细胞类型
  - batch: 批次信息
  - n_genes: 检测基因数
  - n_counts: 总UMI数
```

## 使用建议

### 训练数据
- GSE193086 (Replogle 2022) - 大规模训练
- GSE146136 (Norman 2021) - 组合扰动

### 测试数据
- GSE119450 (Dixit 2016) - 早期方法验证
- GSE90546 (Adamson 2016) - 不同细胞类型

### 化合物筛选
- GSE133344 (sci-Plex) - 药物响应

## 相关资源

- [[scPerturb]] - 标准化数据库
- [[Norman-2021]] - 组合扰动
- [[Replogle-2022]] - 大规模Perturb-seq
- [[sci-Plex]] - 化合物筛选

## 链接

- **GEO主页**: https://www.ncbi.nlm.nih.gov/geo/
- **搜索**: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi
- **FTP**: ftp://ftp.ncbi.nlm.nih.gov/geo/

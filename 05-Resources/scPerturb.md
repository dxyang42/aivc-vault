---
tags: [resource, benchmark, dataset, perturb-seq, scRNA-seq]
date: 2026-03-30
---

# scPerturb

> [[数据集资源]] | [[Benchmark]] | [[Perturb-seq]]

## 概述

**scPerturb** 是一个统一标准化的单细胞扰动数据库，收集并标准化了来自多个研究的Perturb-seq和scRNA-seq扰动数据集。该资源为计算生物学界提供了标准化的基准测试数据。

## 基本信息

| 属性 | 内容 |
|------|------|
| **资源名称** | scPerturb |
| **类型** | 数据库 / Benchmark集合 |
| **发表时间** | 2022 |
| **维护机构** | Pe'er Lab (MSKCC) |
| **数据类型** | scRNA-seq perturbation |
| **访问方式** | Python包 + 数据仓库 |

## 数据集统计

### 包含的数据集

| 数据集 | 细胞数 | 扰动数 | 细胞类型 | 技术 |
|--------|--------|--------|----------|------|
| Norman 2021 | ~100K | ~100 | K562 | Perturb-seq |
| Replogle 2022 | ~2.5M | ~5,000 | K562, RPE1 | Perturb-seq |
| Dixit 2016 | ~30K | ~30 | DC | Perturb-seq |
| Adamson 2016 | ~100K | ~80 | K562 | Perturb-seq |
| Datlinger 2017 | ~30K | ~50 | DC | CROP-seq |
| Joung 2020 | ~50K | ~100 | iPSC | Perturb-seq |
| Srivatsan 2020 | ~650K | ~180 | A549, K562, MCF7 | sci-Plex |

### 总计
- **总细胞数**: > 400万
- **总扰动数**: > 6,000
- **数据集数**: 15+

## 标准化处理

### 统一格式
```python
# scPerturb 统一数据结构
AnnData object:
  - .X: 标准化表达矩阵 (log1p, 10K var genes)
  - .obs['perturbation']: 扰动标识
  - .obs['cell_type']: 细胞类型
  - .obs['dataset']: 来源数据集
  - .var['gene_name']: 基因名称
  - .uns['log1p']: 标准化参数
```

### 质量控制
- 过滤低质量细胞 (< 500 genes)
- 标准化到10,000 counts
- log1p转换
- 选择高变基因 (HVG)

## 访问方式

### Python包安装
```bash
pip install scperturb
```

### 数据加载
```python
import scperturb

# 加载特定数据集
adata = scperturb.load_dataset('Norman_2021')

# 列出所有可用数据集
datasets = scperturb.list_datasets()

# 批量加载
all_data = scperturb.load_all()
```

## Benchmark应用

### 标准评估流程
```
1. 数据加载 (scPerturb)
2. 训练/验证/测试划分
3. 模型训练
4. 在held-out扰动上测试
5. 计算metrics
```

### 支持的评估指标
- **RMSE**: 基因表达预测误差
- **R²**: 解释方差
- **Pearson r**: 相关性
- **AUPRC**: 差异表达基因预测
- **E-distance**: 分布距离

## 相关资源

- [[Norman-2021]] - 组合扰动基准
- [[Replogle-2022]] - 大规模Perturb-seq
- [[sci-Plex]] - 化合物筛选数据
- [[Tahoe-100M]] - 亿级扰动图谱

## 引用

```
scPerturb: a standardized resource for single-cell perturbation analysis
[Authors], 2022
bioRxiv / Nature Methods
```

## 链接

- **GitHub**: https://github.com/sanderlab/scPerturb
- **Documentation**: https://scperturb.readthedocs.io
- **PyPI**: https://pypi.org/project/scperturb

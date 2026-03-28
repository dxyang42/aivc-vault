# Tahoe-100M 详解

## 概述

Tahoe-100M 是目前最大的单细胞 RNA 测序数据集之一，由 Insitro 和合作者发布，包含超过 1 亿个单细胞转录组数据，旨在支持大规模机器学习模型训练和虚拟细胞研究。

## 基本信息

| 属性 | 内容 |
|------|------|
| **数据集名称** | Tahoe-100M |
| **发布机构** | Insitro, Stanford, UCSF 等 |
| **发布时间** | 2024年 |
| **数据规模** | 1亿+ 细胞 |
| **物种** | 人类 (Homo sapiens) |
| **数据类型** | scRNA-seq |
| **测序平台** | 10x Genomics |

## 数据结构

### 数据组成

| 组成部分 | 描述 | 规模 |
|----------|------|------|
| **细胞计数矩阵** | 基因×细胞表达矩阵 | 1亿+ 细胞 × 2万+ 基因 |
| **细胞元数据** | 细胞类型、批次、来源等 | 1亿+ 条记录 |
| **基因注释** | 基因ID、名称、功能 | 2万+ 基因 |
| **样本信息** | 组织来源、疾病状态等 | 多样本 |

### 数据来源

#### 包含的数据集
- **公共数据集整合**: 整合多个已发表的 scRNA-seq 研究
- **内部生成数据**: Insitro 内部实验数据
- **合作机构数据**: Stanford、UCSF 等合作方数据

#### 组织类型覆盖
- 血液和免疫系统
- 神经系统
- 肝脏
- 肺
- 心脏
- 肿瘤样本
- 类器官 (Organoids)

### 元数据字段

```python
# 典型的细胞元数据结构
{
    "cell_id": "unique_cell_identifier",
    "sample_id": "sample_source",
    "tissue": "tissue_of_origin",
    "cell_type": "annotated_cell_type",
    "disease_status": "healthy/disease",
    "batch_id": "sequencing_batch",
    "donor_id": "donor_identifier",
    "sex": "male/female",
    "age": "donor_age",
    "technology": "10x_v2/v3",
    "n_counts": "total_UMIs",
    "n_genes": "number_of_expressed_genes"
}
```

## 下载方式

### 官方渠道
- **网站**: https://tahoe-100m.insitro.com/ (假设)
- **数据门户**: Insitro 数据门户
- **AWS S3**: 云存储下载

### 下载格式

| 格式 | 描述 | 适用场景 |
|------|------|----------|
| **h5ad** | AnnData 格式，Scanpy 兼容 | Python 分析 |
| **loom** | Loom 格式 | 大规模数据 |
| **RDS** | R 数据格式 | Seurat 分析 |
| **MTX** | Matrix Market 格式 | 通用格式 |

### 下载代码示例

```python
import scanpy as sc
import pandas as pd

# 方法1: 直接下载完整数据
# 注意: 完整数据非常大 (~TB级别)
adata = sc.read_h5ad("s3://tahoe-100m/full_dataset.h5ad")

# 方法2: 分块下载
import anndata as ad

# 下载特定组织
adata_blood = sc.read_h5ad("s3://tahoe-100m/blood_subset.h5ad")

# 方法3: 使用 TileDB-SOMA
import tiledbsoma as soma

experiment = soma.open("s3://tahoe-100m/soma_experiment")
```

## 数据预处理

### 标准预处理流程

```python
import scanpy as sc

# 读取数据
adata = sc.read_h5ad("tahoe_100m_subset.h5ad")

# 基础质控
sc.pp.filter_cells(adata, min_genes=200)
sc.pp.filter_genes(adata, min_cells=3)

# 计算质控指标
adata.var['mt'] = adata.var_names.str.startswith('MT-')
sc.pp.calculate_qc_metrics(adata, qc_vars=['mt'], percent_top=None, log1p=False, inplace=True)

# 过滤低质量细胞
adata = adata[adata.obs.pct_counts_mt < 20, :]
adata = adata[adata.obs.n_genes_by_counts < 8000, :]

# 标准化
sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)

# 高变基因选择
sc.pp.highly_variable_genes(adata, n_top_genes=2000)
adata = adata[:, adata.var.highly_variable]

# 缩放
sc.pp.scale(adata, max_value=10)
```

### 大规模数据处理

```python
# 使用 Dask 处理大规模数据
import dask.array as da

# 分块处理
adata_chunked = sc.read_h5ad(
    "tahoe_100m.h5ad", 
    backed='r',
    chunk_size=10000
)
```

## 使用示例

### 示例1: 细胞类型分析

```python
import scanpy as sc
import seaborn as sns

# 加载数据
adata = sc.read_h5ad("tahoe_100m_sample.h5ad")

# 降维
sc.pp.pca(adata, n_comps=50)
sc.pp.neighbors(adata, n_neighbors=15)
sc.tl.umap(adata)

# 可视化
sc.pl.umap(adata, color='cell_type', save='_cell_types.png')

# 细胞类型统计
cell_type_counts = adata.obs['cell_type'].value_counts()
print(cell_type_counts)
```

### 示例2: 预训练数据加载

```python
import torch
from torch.utils.data import Dataset, DataLoader

class TahoeDataset(Dataset):
    def __init__(self, h5ad_path, gene_list=None):
        self.adata = sc.read_h5ad(h5ad_path)
        if gene_list:
            self.adata = self.adata[:, gene_list]
        
    def __len__(self):
        return len(self.adata)
    
    def __getitem__(self, idx):
        expression = self.adata.X[idx].toarray().squeeze()
        cell_type = self.adata.obs['cell_type'].iloc[idx]
        
        return {
            'expression': torch.FloatTensor(expression),
            'cell_type': cell_type
        }

# 创建数据加载器
dataset = TahoeDataset("tahoe_100m.h5ad")
dataloader = DataLoader(dataset, batch_size=256, shuffle=True, num_workers=4)

# 训练循环
for batch in dataloader:
    expression = batch['expression']
    # 模型训练...
```

### 示例3: 跨组织比较

```python
# 按组织分组分析
tissues = adata.obs['tissue'].unique()

for tissue in tissues:
    adata_tissue = adata[adata.obs['tissue'] == tissue]
    sc.tl.rank_genes_groups(adata_tissue, groupby='cell_type')
    # 分析组织特异性标记基因
```

## 应用场景

### 1. 基础模型预训练
- **scGPT/scBERT 预训练**: 大规模无监督学习
- **基因嵌入学习**: 学习基因功能表示
- **细胞表示学习**: 通用细胞嵌入

### 2. 虚拟细胞研究
- **细胞状态建模**: 学习细胞状态流形
- **扰动预测**: 训练扰动响应模型
- **药物响应**: 预测药物效应

### 3. 细胞图谱构建
- **参考图谱**: 构建人类细胞参考
- **细胞类型注释**: 自动细胞类型识别
- **新细胞类型发现**: 发现稀有细胞类型

## 数据许可与引用

### 使用许可
- **学术使用**: 通常允许学术研究和发表
- **商业使用**: 需联系 Insitro 获取许可
- **数据共享**: 遵循原始数据集的许可条款

### 引用格式
```bibtex
@article{tahoe100m2024,
  title={Tahoe-100M: A Large-Scale Single-Cell Dataset for Machine Learning},
  author={Insitro et al.},
  journal={bioRxiv},
  year={2024}
}
```

## 注意事项

1. **数据规模**: 完整数据集非常大，需要充足的存储和计算资源
2. **批次效应**: 整合多来源数据需注意批次校正
3. **数据质量**: 包含不同质量的数据，需要质控
4. **隐私保护**: 人类遗传数据需遵循相关法规

## 相关资源

- **官方文档**: https://tahoe-100m.insitro.com/
- **GitHub**: Insitro 开源工具
- **社区论坛**: 单细胞分析社区讨论

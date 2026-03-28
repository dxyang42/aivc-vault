# Replogle 2022 详解

## 概述

Replogle 2022 是由 Weissman Lab 和 Broad Institute 发布的大规模全基因组 Perturb-seq 数据集，包含 K562 和 RPE1 两种细胞系的全基因组 CRISPRi 筛选数据，是目前最大的单细胞遗传筛选数据集之一。

## 基本信息

| 属性 | 内容 |
|------|------|
| **数据集名称** | Replogle 2022 (Perturb-seq 2022) |
| **发表文献** | Replogle et al., Cell 2022 |
| **第一作者** | Joseph M. Replogle |
| **通讯作者** | Jonathan S. Weissman, Aviv Regev |
| **发表时间** | 2022年 |

## 数据结构

### 细胞系信息

| 细胞系 | 类型 | 细胞数 | 扰动基因数 |
|--------|------|--------|------------|
| **K562** | 慢性髓系白血病 | ~2,000,000 | ~9,000 |
| **RPE1** | 视网膜色素上皮 | ~1,000,000 | ~9,000 |

### 实验设计

#### 扰动策略
- **CRISPRi** (CRISPR interference): 基因敲低
- **全基因组覆盖**: 约 9,000 个基因
- **每个基因**: 3-10 个 sgRNA
- **对照**: 非靶向 sgRNA (NT)

#### 数据规模
```
总细胞数: ~3,000,000
总基因数: ~20,000 (转录组)
扰动基因数: ~9,000
每个扰动平均细胞数: ~100-300
```

### 数据组成

```python
# AnnData 对象结构
adata.obs  # 细胞元数据
├── 'gene_target': 目标基因名称
├── 'guide_id': sgRNA ID
├── 'guide_seq': sgRNA 序列
├── 'cell_line': K562 或 RPE1
├── 'batch': 实验批次
├── 'n_counts': UMI 计数
├── 'n_genes': 检测基因数
├── 'percent_mito': 线粒体基因比例
└── 'cell_cycle_phase': G1/S/G2M

adata.var  # 基因信息
├── 'gene_name': 基因符号
├── 'ensembl_id': Ensembl ID
├── 'gene_type': 蛋白编码/lncRNA/etc
└── 'chromosome': 染色体位置

adata.X    # 表达矩阵 (cells × genes)
```

## 下载方式

### 官方来源
- **GEO 数据库**: GSE193852
- **Broad 数据门户**: https://portals.broadinstitute.org/perturb-seq
- **cellxgene**: https://cellxgene.cziscience.com/
- **scPerturb**: 统一接口

### 下载代码

```python
# 方法1: 使用 scanpy 读取
import scanpy as sc

# K562 数据
adata_k562 = sc.read_h5ad("GSE193852_k562_gwps.h5ad")

# RPE1 数据
adata_rpe1 = sc.read_h5ad("GSE193852_rpe1_gwps.h5ad")

# 方法2: 使用 scPerturb
import scperturb
adata_k562 = scperturb.dataset('replogle_k562')
adata_rpe1 = scperturb.dataset('replogle_rpe1')

# 方法3: 从 cellxgene 下载
# 访问 https://cellxgene.cziscience.com/collections/...
```

### 数据格式
- **主要格式**: h5ad (AnnData)
- **文件大小**: 
  - K562: ~10GB
  - RPE1: ~5GB
- **备用格式**: loom, Seurat RDS

## 数据预处理

### 标准预处理

```python
import scanpy as sc
import numpy as np

# 读取数据
adata = sc.read_h5ad("replogle_k562.h5ad")

print(f"原始数据: {adata.n_obs} 细胞 × {adata.n_vars} 基因")

# 基础质控
sc.pp.filter_cells(adata, min_genes=200)
sc.pp.filter_genes(adata, min_cells=10)

# 计算质控指标
adata.var['mt'] = adata.var_names.str.startswith('MT-')
sc.pp.calculate_qc_metrics(
    adata, 
    qc_vars=['mt'], 
    percent_top=[20], 
    inplace=True
)

# 过滤低质量细胞
adata = adata[adata.obs.n_genes_by_counts < 8000, :]
adata = adata[adata.obs.pct_counts_mt < 15, :]
adata = adata[adata.obs.n_counts > 1000, :]

print(f"质控后: {adata.n_obs} 细胞 × {adata.n_vars} 基因")

# 标准化
sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)

# 高变基因
sc.pp.highly_variable_genes(
    adata, 
    n_top_genes=2000,
    subset=True,
    flavor='seurat_v3'
)

# 缩放
sc.pp.scale(adata, max_value=10)
```

### 大规模数据处理

```python
# 对于内存不足的情况，使用 backed 模式
adata = sc.read_h5ad("replogle_k562.h5ad", backed='r')

# 分块处理
chunk_size = 10000
for i in range(0, adata.n_obs, chunk_size):
    chunk = adata[i:i+chunk_size, :].to_memory()
    # 处理 chunk...
```

## 使用示例

### 示例1: 数据加载和探索

```python
import scanpy as sc
import pandas as pd
import matplotlib.pyplot as plt

# 加载数据
adata = sc.read_h5ad("replogle_k562.h5ad")

# 基本信息
print(f"数据集: Replogle 2022 K562")
print(f"细胞数: {adata.n_obs:,}")
print(f"基因数: {adata.n_vars:,}")
print(f"扰动基因数: {adata.obs['gene_target'].nunique():,}")

# 细胞分布
print("\n细胞周期分布:")
print(adata.obs['cell_cycle_phase'].value_counts())

# 扰动统计
perturb_counts = adata.obs['gene_target'].value_counts()
print(f"\n每个扰动的平均细胞数: {perturb_counts.mean():.1f}")
print(f"每个扰动的中位细胞数: {perturb_counts.median():.1f}")
```

### 示例2: 降维和可视化

```python
# 降维
sc.pp.pca(adata, n_comps=50)
sc.pp.neighbors(adata, n_neighbors=15, n_pcs=30)
sc.tl.umap(adata)

# 可视化
sc.pl.umap(adata, color='cell_cycle_phase', save='_cell_cycle.png')
sc.pl.umap(adata, color='gene_target', groups=['TP53', 'KRAS', 'MYC'], 
            save='_perturbations.png')

# 计算聚类
sc.tl.leiden(adata, resolution=0.5)
sc.pl.umap(adata, color='leiden', save='_clusters.png')
```

### 示例3: 差异表达分析

```python
# 选择特定扰动进行分析
target_gene = 'TP53'
adata_subset = adata[
    (adata.obs['gene_target'] == target_gene) | 
    (adata.obs['gene_target'] == 'NT')
]

# 差异表达
sc.tl.rank_genes_groups(
    adata_subset, 
    groupby='gene_target',
    reference='NT',
    method='wilcoxon'
)

# 可视化 top 基因
sc.pl.rank_genes_groups_heatmap(adata_subset, n_genes=20)
sc.pl.rank_genes_groups_dotplot(adata_subset, n_genes=10)

# 获取差异基因结果
deg_results = sc.get.rank_genes_groups_df(adata_subset, group=target_gene)
print(f"{target_gene} 敲低的 Top 差异基因:")
print(deg_results.head(10))
```

### 示例4: 基因功能分析

```python
# 分析特定通路基因的影响
pathway_genes = ['TP53', 'RB1', 'CDKN2A', 'MYC', 'KRAS']  # 细胞周期/癌症相关

# 提取这些基因的扰动数据
adata_pathway = adata[adata.obs['gene_target'].isin(pathway_genes + ['NT'])]

# 计算每个扰动的平均表达谱
perturb_profiles = {}
for gene in pathway_genes:
    cells = adata_pathway[adata_pathway.obs['gene_target'] == gene]
    perturb_profiles[gene] = cells.X.mean(axis=0)

control_profile = adata_pathway[
    adata_pathway.obs['gene_target'] == 'NT'
].X.mean(axis=0)

# 计算扰动效应
perturb_effects = {}
for gene, profile in perturb_profiles.items():
    perturb_effects[gene] = profile - control_profile

# 可视化扰动效应的相关性
import seaborn as sns
effect_matrix = np.array([perturb_effects[g] for g in pathway_genes])
corr_matrix = np.corrcoef(effect_matrix)

plt.figure(figsize=(8, 6))
sns.heatmap(corr_matrix, 
            xticklabels=pathway_genes,
            yticklabels=pathway_genes,
            annot=True, cmap='coolwarm', center=0)
plt.title('Perturbation Effect Correlation')
plt.tight_layout()
plt.savefig('perturbation_correlation.png')
```

### 示例5: 机器学习模型训练

```python
import torch
import torch.nn as nn
from torch.utils.data import Dataset, DataLoader, Subset
import numpy as np

class ReplogleDataset(Dataset):
    def __init__(self, adata, use_hvg=True):
        if use_hvg and 'highly_variable' in adata.var:
            self.adata = adata[:, adata.var.highly_variable]
        else:
            self.adata = adata
            
        self.expression = self.adata.X.toarray()
        
        # 编码基因靶点
        self.targets = self.adata.obs['gene_target'].values
        self.target_to_idx = {t: i for i, t in enumerate(np.unique(self.targets))}
        self.target_idx = np.array([self.target_to_idx[t] for t in self.targets])
        
    def __len__(self):
        return len(self.expression)
    
    def __getitem__(self, idx):
        return {
            'expression': torch.FloatTensor(self.expression[idx]),
            'target': torch.LongTensor([self.target_idx[idx]])[0],
            'target_name': self.targets[idx]
        }

# 创建数据集
dataset = ReplogleDataset(adata)

# 划分训练/测试集
from sklearn.model_selection import train_test_split
indices = list(range(len(dataset)))
train_idx, test_idx = train_test_split(indices, test_size=0.2, random_state=42)

train_dataset = Subset(dataset, train_idx)
test_dataset = Subset(dataset, test_idx)

train_loader = DataLoader(train_dataset, batch_size=256, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=256)

# 定义模型
class PerturbationClassifier(nn.Module):
    def __init__(self, input_dim, hidden_dim, n_classes):
        super().__init__()
        self.encoder = nn.Sequential(
            nn.Linear(input_dim, hidden_dim),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(hidden_dim, hidden_dim),
            nn.ReLU(),
            nn.Dropout(0.2)
        )
        self.classifier = nn.Linear(hidden_dim, n_classes)
    
    def forward(self, x):
        z = self.encoder(x)
        return self.classifier(z)

# 训练
model = PerturbationClassifier(
    input_dim=dataset.expression.shape[1],
    hidden_dim=256,
    n_classes=len(dataset.target_to_idx)
)

criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

# 训练循环
for epoch in range(5):
    model.train()
    total_loss = 0
    for batch in train_loader:
        pred = model(batch['expression'])
        loss = criterion(pred, batch['target'])
        
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        
        total_loss += loss.item()
    
    print(f"Epoch {epoch+1}, Loss: {total_loss/len(train_loader):.4f}")
```

## 应用场景

### 1. 基因功能注释
- **敲低效应**: 分析基因敲低后的转录组变化
- **功能推断**: 从扰动响应推断基因功能
- **通路归属**: 将基因分配到特定通路

### 2. 基因调控网络推断
- **调控关系**: 从转录变化推断调控关系
- **主调控因子**: 识别关键调控基因
- **网络模块**: 发现功能基因模块

### 3. 虚拟扰动模型训练
- **基准数据集**: 评估扰动预测方法
- **预训练**: 大规模预训练基础模型
- **迁移学习**: 迁移到其他细胞类型

### 4. 药物靶点发现
- **靶点验证**: 验证潜在药物靶点
- **脱靶效应**: 预测脱靶效应
- **组合靶点**: 发现协同靶点组合

## 数据特点与注意事项

### 优势
- **规模巨大**: 数百万细胞，统计能力强
- **全基因组覆盖**: 约 9,000 个基因
- **双细胞系**: 可比较不同细胞背景
- **高质量**: 严格的实验设计和质控

### 局限
- **仅 CRISPRi**: 敲低而非敲除
- **仅两种细胞系**: 细胞类型覆盖有限
- **无组合扰动**: 仅单基因扰动
- **数据量大**: 需要充足计算资源

### 使用建议
1. **计算资源**: 准备足够的内存和存储
2. **批次校正**: 注意不同批次间的差异
3. **对照选择**: 谨慎选择对照细胞
4. **效应大小**: 注意 CRISPRi 的敲低效率

## 引用格式

```bibtex
@article{replogle2022mapping,
  title={Mapping information-rich genotype-phenotype landscapes with genome-scale Perturb-seq},
  author={Replogle, Joseph M and Saunders, Reuben A and Pogson, Angela N and others},
  journal={Cell},
  volume={185},
  number={14},
  pages={2559--2575},
  year={2022}
}
```

## 相关资源

- **论文**: https://www.cell.com/cell/fulltext/S0092-8674(22)00570-8
- **数据**: GEO GSE193852
- **代码**: https://github.com/weissman-lab/GWPS
- **cellxgene**: https://cellxgene.cziscience.com/

# sci-Plex 详解

## 概述

sci-Plex (Single-Cell Combinatorial Indexing Perturb-seq) 是一种结合组合索引 (Combinatorial Indexing) 和 Perturb-seq 的高通量单细胞筛选技术，主要用于大规模化合物扰动筛选。该技术能够以低成本筛选数千种化合物对细胞的影响。

## 基本信息

| 属性 | 内容 |
|------|------|
| **技术名称** | sci-Plex (Single-Cell Combinatorial Indexing Perturb-seq) |
| **发表文献** | Srivatsan et al., Science 2020 |
| **作者团队** | Trapnell Lab (UW), Shendure Lab (UW) |
| **技术类型** | 单细胞化合物筛选 |
| **核心创新** | 组合索引 + 单细胞转录组 |

## 技术原理

### 组合索引 (Combinatorial Indexing)

#### 基本概念
通过多轮条形码标记，用少量条形码组合标记大量样本：

```
第一轮: 96 个条形码
第二轮: 96 个条形码  
第三轮: 96 个条形码
总组合数: 96 × 96 × 96 = 884,736
```

#### 工作流程
1. **第一轮标记**: 在 96 孔板中用不同化合物处理细胞，每孔加入第一轮条形码
2. **混合**: 将所有细胞混合
3. **第二轮标记**: 重新分板，加入第二轮条形码
4. **重复**: 进行多轮标记
5. **单细胞捕获**: 使用 10x Genomics 等平台捕获单细胞

### sci-Plex 流程

```
化合物处理 → 第一轮条形码 → 混合 → 
第二轮条形码 → 混合 → 
第三轮条形码 → 单细胞捕获 → 
测序 → 数据分析
```

## 数据集

### sci-Plex 1

| 属性 | 内容 |
|------|------|
| **化合物数** | 188 |
| **细胞数** | ~650,000 |
| **细胞系** | A549, K562, MCF7 |
| **处理时间** | 6h, 24h |

### sci-Plex 2

| 属性 | 内容 |
|------|------|
| **化合物数** | 4,518 |
| **细胞数** | ~2,800,000 |
| **细胞系** | A549, K562 |
| **处理时间** | 24h |

### sci-Plex 3

| 属性 | 内容 |
|------|------|
| **化合物数** | 1,036 |
| **细胞数** | ~1,200,000 |
| **细胞系** | A549 |
| **处理条件** | 不同浓度、时间 |

## 数据结构

### 数据组成

```python
# AnnData 对象结构
adata.obs  # 细胞元数据
├── 'cell_barcode': 10x 细胞条形码
├── 'sample': 样本ID
├── 'cell_type': 细胞系 (A549/K562/MCF7)
├── 'treatment': 化合物名称
├── 'dose': 剂量
├── 'time': 处理时间
├── 'plate': 原始板号
├── 'well': 孔位置
├── 'round1_bc': 第一轮条形码
├── 'round2_bc': 第二轮条形码
├── 'round3_bc': 第三轮条形码
├── 'n_counts': UMI 计数
└── 'n_genes': 检测基因数

adata.var  # 基因信息
├── 'gene_name': 基因符号
└── 'gene_id': Ensembl ID

adata.X    # 表达矩阵
```

### 数据规模对比

| 数据集 | 化合物数 | 细胞数 | 细胞系 |
|--------|----------|--------|--------|
| sci-Plex 1 | 188 | 650K | 3种 |
| sci-Plex 2 | 4,518 | 2.8M | 2种 |
| sci-Plex 3 | 1,036 | 1.2M | 1种 |

## 下载方式

### 官方来源
- **GEO 数据库**: 
  - sci-Plex 1: GSE142475
  - sci-Plex 2: GSE156755
  - sci-Plex 3: GSE174748
- **scPerturb**: `scperturb.dataset('sciplex1/2/3')`
- **cellxgene**: https://cellxgene.cziscience.com/

### 下载代码

```python
import scanpy as sc

# sci-Plex 1
adata_sp1 = sc.read_h5ad("GSE142475_sciPlex1.h5ad")

# sci-Plex 2
adata_sp2 = sc.read_h5ad("GSE156755_sciPlex2.h5ad")

# sci-Plex 3
adata_sp3 = sc.read_h5ad("GSE174748_sciPlex3.h5ad")

# 或使用 scPerturb
import scperturb
adata_sp1 = scperturb.dataset('sciplex1')
adata_sp2 = scperturb.dataset('sciplex2')
adata_sp3 = scperturb.dataset('sciplex3')
```

## 数据预处理

### 标准预处理

```python
import scanpy as sc
import numpy as np

# 读取数据
adata = sc.read_h5ad("sciplex2.h5ad")

print(f"原始数据: {adata.n_obs:,} 细胞 × {adata.n_vars:,} 基因")
print(f"化合物数: {adata.obs['treatment'].nunique()}")

# 基础质控
sc.pp.filter_cells(adata, min_genes=200)
sc.pp.filter_genes(adata, min_cells=10)

# 计算质控指标
adata.var['mt'] = adata.var_names.str.startswith('MT-')
sc.pp.calculate_qc_metrics(adata, qc_vars=['mt'], inplace=True)

# 过滤
adata = adata[adata.obs.n_genes_by_counts < 8000, :]
adata = adata[adata.obs.pct_counts_mt < 20, :]

# 标准化
sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)

# 高变基因
sc.pp.highly_variable_genes(adata, n_top_genes=2000, subset=True)

# 缩放
sc.pp.scale(adata, max_value=10)

print(f"处理后: {adata.n_obs:,} 细胞 × {adata.n_vars:,} 基因")
```

### 化合物信息处理

```python
# 查看化合物分布
treatment_counts = adata.obs['treatment'].value_counts()
print("Top 10 化合物:")
print(treatment_counts.head(10))

# 剂量分析
print("\n剂量分布:")
print(adata.obs['dose'].value_counts())

# 时间分析
print("\n处理时间分布:")
print(adata.obs['time'].value_counts())
```

## 使用示例

### 示例1: 数据探索

```python
import scanpy as sc
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 加载数据
adata = sc.read_h5ad("sciplex2.h5ad")

# 基本信息
print(f"sci-Plex 2 数据集")
print(f"细胞数: {adata.n_obs:,}")
print(f"化合物数: {adata.obs['treatment'].nunique()}")
print(f"细胞系: {adata.obs['cell_type'].unique()}")

# 化合物类别统计
treatment_stats = adata.obs.groupby('treatment').size().describe()
print(f"\n每个化合物的细胞数统计:")
print(treatment_stats)

# 可视化细胞分布
sc.pp.pca(adata, n_comps=50)
sc.pp.neighbors(adata)
sc.tl.umap(adata)

sc.pl.umap(adata, color='cell_type', save='_cell_type.png')
sc.pl.umap(adata, color='treatment', groups=['DMSO'], save='_dmso.png')
```

### 示例2: 化合物效应分析

```python
# 选择特定化合物分析
target_compound = 'Vorinostat'  # HDAC 抑制剂
adata_compound = adata[
    (adata.obs['treatment'] == target_compound) | 
    (adata.obs['treatment'] == 'DMSO')
]

# 差异表达分析
sc.tl.rank_genes_groups(
    adata_compound,
    groupby='treatment',
    reference='DMSO',
    method='wilcoxon'
)

# 可视化
sc.pl.rank_genes_groups_dotplot(adata_compound, n_genes=15)
sc.pl.rank_genes_groups_heatmap(adata_compound, n_genes=20)

# 获取差异基因
deg_results = sc.get.rank_genes_groups_df(
    adata_compound, 
    group=target_compound
)
print(f"{target_compound} 处理的 Top 差异基因:")
print(deg_results.head(10))
```

### 示例3: 剂量-响应分析

```python
# 分析剂量效应
compound = 'Vorinostat'
adata_dose = adata[
    (adata.obs['treatment'] == compound) | 
    (adata.obs['treatment'] == 'DMSO')
]

# 计算每个剂量的平均表达
doses = adata_dose.obs['dose'].unique()
dose_profiles = {}

for dose in doses:
    if dose != 'DMSO':
        cells = adata_dose[adata_dose.obs['dose'] == dose]
        dose_profiles[dose] = cells.X.mean(axis=0)

dmso_profile = adata_dose[adata_dose.obs['treatment'] == 'DMSO'].X.mean(axis=0)

# 计算剂量响应
dose_responses = {}
for dose, profile in dose_profiles.items():
    dose_responses[dose] = np.linalg.norm(profile - dmso_profile)

# 绘制剂量-响应曲线
doses_numeric = [float(d) for d in dose_responses.keys()]
responses = list(dose_responses.values())

plt.figure(figsize=(8, 5))
plt.plot(doses_numeric, responses, 'o-')
plt.xlabel('Dose (nM)')
plt.ylabel('Transcriptional Response')
plt.title(f'{compound} Dose-Response')
plt.xscale('log')
plt.tight_layout()
plt.savefig(f'{compound}_dose_response.png')
```

### 示例4: 化合物聚类

```python
# 计算每个化合物的转录组特征
compounds = adata.obs['treatment'].unique()
compound_profiles = []
compound_names = []

for compound in compounds:
    if compound != 'DMSO':
        cells = adata[adata.obs['treatment'] == compound]
        profile = cells.X.mean(axis=0)
        compound_profiles.append(profile)
        compound_names.append(compound)

# 构建化合物-特征矩阵
compound_matrix = np.array(compound_profiles)

# 聚类分析
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA

# PCA 降维
pca = PCA(n_components=50)
compound_pca = pca.fit_transform(compound_matrix)

# K-means 聚类
kmeans = KMeans(n_clusters=10, random_state=42)
clusters = kmeans.fit_predict(compound_pca)

# 可视化
from sklearn.manifold import TSNE
tsne = TSNE(n_components=2, random_state=42)
compound_tsne = tsne.fit_transform(compound_pca)

plt.figure(figsize=(10, 8))
scatter = plt.scatter(compound_tsne[:, 0], compound_tsne[:, 1], 
                      c=clusters, cmap='tab10', alpha=0.6)
plt.colorbar(scatter, label='Cluster')
plt.title('Compound Clustering based on Transcriptional Response')
plt.tight_layout()
plt.savefig('compound_clustering.png')

# 查看每个聚类的化合物
for i in range(10):
    cluster_compounds = [compound_names[j] for j in range(len(clusters)) if clusters[j] == i]
    print(f"Cluster {i}: {cluster_compounds[:10]}")  # 显示前10个
```

### 示例5: 机器学习模型

```python
import torch
from torch.utils.data import Dataset, DataLoader

class SciPlexDataset(Dataset):
    def __init__(self, adata):
        self.expression = adata.X.toarray()
        
        # 编码化合物
        self.treatments = adata.obs['treatment'].values
        self.treatment_to_idx = {t: i for i, t in enumerate(np.unique(self.treatments))}
        self.treatment_idx = np.array([self.treatment_to_idx[t] for t in self.treatments])
        
        # 编码剂量
        self.doses = adata.obs['dose'].values
        
    def __len__(self):
        return len(self.expression)
    
    def __getitem__(self, idx):
        return {
            'expression': torch.FloatTensor(self.expression[idx]),
            'treatment': torch.LongTensor([self.treatment_idx[idx]])[0],
            'dose': self.doses[idx]
        }

# 创建数据集
dataset = SciPlexDataset(adata)
dataloader = DataLoader(dataset, batch_size=256, shuffle=True)

# 化合物响应预测模型
class CompoundResponsePredictor(nn.Module):
    def __init__(self, input_dim, hidden_dim, n_compounds):
        super().__init__()
        self.treatment_embedding = nn.Embedding(n_compounds, 64)
        
        self.encoder = nn.Sequential(
            nn.Linear(input_dim, hidden_dim),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(hidden_dim, hidden_dim)
        )
        
        self.predictor = nn.Sequential(
            nn.Linear(hidden_dim + 64, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, input_dim)
        )
    
    def forward(self, expression, treatment):
        z = self.encoder(expression)
        t = self.treatment_embedding(treatment)
        z_t = torch.cat([z, t], dim=-1)
        return self.predictor(z_t)

# 训练模型
model = CompoundResponsePredictor(
    input_dim=dataset.expression.shape[1],
    hidden_dim=256,
    n_compounds=len(dataset.treatment_to_idx)
)

criterion = nn.MSELoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

# 训练循环
for epoch in range(5):
    model.train()
    total_loss = 0
    for batch in dataloader:
        # 预测处理后的表达
        pred = model(batch['expression'], batch['treatment'])
        
        # 自编码器目标：重构输入
        loss = criterion(pred, batch['expression'])
        
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        
        total_loss += loss.item()
    
    print(f"Epoch {epoch+1}, Loss: {total_loss/len(dataloader):.4f}")
```

## 应用场景

### 1. 药物发现
- **化合物筛选**: 高通量筛选活性化合物
- **机制研究**: 推断化合物作用机制
- **脱靶效应**: 识别非预期效应

### 2. 化合物分类
- **MoA 预测**: 预测化合物作用机制
- **毒性预测**: 识别潜在毒性化合物
- **重定位**: 发现现有药物的新用途

### 3. 剂量-响应建模
- **EC50 计算**: 计算半数效应浓度
- **剂量优化**: 确定最佳治疗剂量
- **联合用药**: 预测药物组合效应

### 4. 虚拟筛选
- **模型训练**: 训练化合物响应预测模型
- **虚拟筛选**: 预测未测试化合物的效应
- **分子设计**: 指导新化合物设计

## 技术优势与局限

### 优势
- **高通量**: 可同时筛选数千种化合物
- **低成本**: 组合索引降低测序成本
- **单细胞分辨率**: 捕获细胞异质性
- **全转录组**: 无偏倚地检测所有基因

### 局限
- **处理时间短**: 主要 6h/24h，缺少长期效应
- **单一读数**: 仅转录组，无蛋白/代谢
- **细胞系限制**: 主要使用癌细胞系
- **技术噪声**: 组合索引引入的噪声

## 引用格式

```bibtex
@article{srivatsan2020massively,
  title={Massively multiplex chemical transcriptomics at single-cell resolution},
  author={Srivatsan, Sanjay R and McFaline-Figueroa, José L and Ramani, Vijay and others},
  journal={Science},
  volume={367},
  number={6473},
  pages={45--51},
  year={2020}
}
```

## 相关资源

- **论文**: https://science.sciencemag.org/content/367/6473/45
- **数据**: 
  - sci-Plex 1: GEO GSE142475
  - sci-Plex 2: GEO GSE156755
  - sci-Plex 3: GEO GSE174748
- **代码**: https://github.com/cole-trapnell-lab/sci-plex
- **cellxgene**: https://cellxgene.cziscience.com/

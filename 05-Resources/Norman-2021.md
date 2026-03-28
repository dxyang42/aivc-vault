# Norman 2021 详解

## 概述

Norman 2021 是由 MIT Weissman Lab 和 Broad Institute 合作发布的大规模组合 Perturb-seq 数据集，是单细胞遗传学研究的重要基准数据集，用于研究基因相互作用和组合扰动效应。

## 基本信息

| 属性 | 内容 |
|------|------|
| **数据集名称** | Norman 2021 (Perturb-seq) |
| **发表文献** | Norman et al., Science 2019; 扩展数据集 2021 |
| **作者团队** | Weissman Lab (MIT), Regev Lab (Broad) |
| **细胞系** | K562 (慢性髓系白血病) |
| **数据类型** | Perturb-seq (scRNA-seq + CRISPR) |
| **测序平台** | 10x Genomics |

## 数据结构

### 实验设计

#### 扰动类型
| 扰动类别 | 描述 | 数量 |
|----------|------|------|
| **单基因敲除** | 单个基因 CRISPRi | ~100 |
| **双基因组合** | 两基因同时敲除 | ~1000+ 对 |
| **三基因组合** | 三基因同时敲除 | 部分 |
| **对照** | 非靶向 sgRNA | 多个 |

#### 基因选择
- **转录因子**: 主要调控因子
- **染色质调控因子**: 表观遗传调控
- **信号通路基因**: 关键信号分子
- **管家基因**: 基础功能基因

### 数据组成

```
总细胞数: ~200,000+
每个扰动: ~100-500 细胞
基因数: ~20,000 (全转录组)
扰动组合数: 1,000+
```

### 数据文件结构

```python
# AnnData 对象结构
adata.obs  # 细胞元数据
├── 'perturbation': 扰动基因名称 (e.g., "TP53+KRAS")
├── 'perturbation_type': 单/双/三基因
├── 'guide_rna': sgRNA 序列
├── 'batch': 实验批次
├── 'n_counts': UMI 计数
├── 'n_genes': 检测基因数
└── 'cell_cycle': 细胞周期阶段

adata.var  # 基因信息
├── 'gene_name': 基因符号
├── 'highly_variable': 高变基因标记
└── 'gene_category': 基因功能分类

adata.X    # 表达矩阵 (cells × genes)
```

## 下载方式

### 官方来源
- **GEO 数据库**: GSE146194
- **Broad 数据门户**: https://portals.broadinstitute.org/perturb-seq
- **GitHub**: Weissman Lab 代码仓库
- **scPerturb 包**: 统一接口

### 下载代码

```python
# 方法1: 使用 scanpy 读取
import scanpy as sc

# 从 GEO 下载
adata = sc.read_h5ad("GSE146194_norman_2021.h5ad")

# 方法2: 使用 scPerturb
import scperturb
adata = scperturb.dataset('norman_2021')

# 方法3: 直接下载链接
import urllib.request
url = "https://ftp.ncbi.nlm.nih.gov/geo/series/GSE146nnn/GSE146194/..."
urllib.request.urlretrieve(url, "norman_2021.h5ad")
```

### 数据格式
- **主要格式**: h5ad (AnnData)
- **备用格式**: loom, Seurat RDS
- **原始数据**: FASTQ, 计数矩阵

## 数据预处理

### 标准预处理

```python
import scanpy as sc
import numpy as np

# 读取数据
adata = sc.read_h5ad("norman_2021_raw.h5ad")

# 基础质控
sc.pp.filter_cells(adata, min_genes=200)
sc.pp.filter_genes(adata, min_cells=3)

# 计算质控指标
adata.var['mt'] = adata.var_names.str.startswith('MT-')
sc.pp.calculate_qc_metrics(adata, qc_vars=['mt'], inplace=True)

# 过滤低质量细胞
adata = adata[adata.obs.n_genes_by_counts < 8000, :]
adata = adata[adata.obs.pct_counts_mt < 10, :]

# 标准化
sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)

# 高变基因
sc.pp.highly_variable_genes(adata, n_top_genes=2000, subset=True)

# 缩放
sc.pp.scale(adata)
```

### Perturb-seq 特定处理

```python
# 分离对照和扰动细胞
control_cells = adata[adata.obs['perturbation'] == 'control']
perturbed_cells = adata[adata.obs['perturbation'] != 'control']

# 计算扰动效应
# 对每个扰动，计算相对于对照的差异
```

## 使用示例

### 示例1: 加载和探索

```python
import scanpy as sc
import pandas as pd
import seaborn as sns

# 加载数据
adata = sc.read_h5ad("norman_2021.h5ad")

# 基本信息
print(f"细胞数: {adata.n_obs}")
print(f"基因数: {adata.n_vars}")
print(f"扰动数: {adata.obs['perturbation'].nunique()}")

# 扰动统计
perturb_counts = adata.obs['perturbation'].value_counts()
print("\nTop 10 扰动:")
print(perturb_counts.head(10))

# 可视化细胞分布
sc.pp.pca(adata, n_comps=50)
sc.pp.neighbors(adata)
sc.tl.umap(adata)
sc.pl.umap(adata, color='perturbation_type', save='_perturbation_type.png')
```

### 示例2: 扰动效应分析

```python
# 计算每个扰动的平均表达
perturbations = adata.obs['perturbation'].unique()
perturb_expression = {}

for pert in perturbations:
    if pert != 'control':
        cells = adata[adata.obs['perturbation'] == pert]
        perturb_expression[pert] = cells.X.mean(axis=0)

# 计算差异表达
control_mean = adata[adata.obs['perturbation'] == 'control'].X.mean(axis=0)

diff_expr = {}
for pert, expr in perturb_expression.items():
    diff_expr[pert] = expr - control_mean

# 找到最强响应的扰动
perturb_magnitude = {k: np.linalg.norm(v) for k, v in diff_expr.items()}
top_perturbs = sorted(perturb_magnitude.items(), key=lambda x: x[1], reverse=True)
print("最强效应扰动:", top_perturbs[:10])
```

### 示例3: 基因相互作用分析

```python
# 分析双基因扰动
double_perturbs = adata[adata.obs['perturbation_type'] == 'double']

# 提取基因对
def parse_double_perturb(pert_str):
    """解析双基因扰动字符串"""
    genes = pert_str.split('+')
    return genes[0], genes[1]

# 计算遗传相互作用
gi_scores = {}

for pert in double_perturbs.obs['perturbation'].unique():
    g1, g2 = parse_double_perturb(pert)
    
    # 获取单基因和双基因扰动的效应
    single1 = diff_expr.get(g1, 0)
    single2 = diff_expr.get(g2, 0)
    double = diff_expr.get(pert, 0)
    
    # 计算遗传相互作用分数 (GI score)
    gi_score = np.linalg.norm(double - (single1 + single2))
    gi_scores[pert] = gi_score

# 找出强相互作用的基因对
strong_gi = sorted(gi_scores.items(), key=lambda x: x[1], reverse=True)
print("强遗传相互作用:", strong_gi[:10])
```

### 示例4: 训练预测模型

```python
import torch
from torch.utils.data import Dataset, DataLoader

class PerturbDataset(Dataset):
    def __init__(self, adata):
        self.expression = adata.X.toarray()
        self.perturbations = adata.obs['perturbation'].values
        
        # 编码扰动为 one-hot
        self.perturb_to_idx = {p: i for i, p in enumerate(set(self.perturbations))}
        self.perturb_idx = [self.perturb_to_idx[p] for p in self.perturbations]
        
    def __len__(self):
        return len(self.expression)
    
    def __getitem__(self, idx):
        return {
            'expression': torch.FloatTensor(self.expression[idx]),
            'perturbation': torch.LongTensor([self.perturb_idx[idx]])
        }

# 创建数据加载器
dataset = PerturbDataset(adata)
dataloader = DataLoader(dataset, batch_size=64, shuffle=True)

# 定义简单预测模型
class PerturbPredictor(torch.nn.Module):
    def __init__(self, input_dim, hidden_dim, n_perturbations):
        super().__init__()
        self.encoder = torch.nn.Sequential(
            torch.nn.Linear(input_dim, hidden_dim),
            torch.nn.ReLU(),
            torch.nn.Linear(hidden_dim, hidden_dim)
        )
        self.perturb_embedding = torch.nn.Embedding(n_perturbations, hidden_dim)
        self.decoder = torch.nn.Linear(hidden_dim, input_dim)
    
    def forward(self, expression, perturb_idx):
        z = self.encoder(expression)
        p = self.perturb_embedding(perturb_idx).squeeze(1)
        z_perturbed = z + p  # 简单加法模型
        return self.decoder(z_perturbed)

# 训练模型
model = PerturbPredictor(input_dim=adata.n_vars, hidden_dim=128, 
                         n_perturbations=len(dataset.perturb_to_idx))
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
criterion = torch.nn.MSELoss()

for epoch in range(10):
    for batch in dataloader:
        pred = model(batch['expression'], batch['perturbation'])
        loss = criterion(pred, batch['expression'])
        
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
```

## 应用场景

### 1. 基因相互作用研究
- **合成致死**: 发现协同致死基因对
- **功能冗余**: 识别功能重叠的基因
- **通路分析**: 推断信号通路结构

### 2. 扰动预测模型基准
- **方法比较**: 评估不同预测方法的性能
- **模型开发**: 开发新的虚拟扰动方法
- **基准测试**: 标准化评估指标

### 3. 网络推断
- **基因调控网络**: 从扰动响应推断调控关系
- **因果推断**: 识别因果基因关系
- **模块发现**: 识别功能基因模块

## 评估指标

### 预测性能指标
| 指标 | 描述 | 公式 |
|------|------|------|
| **MSE** | 均方误差 | $\frac{1}{n}\sum(y - \hat{y})^2$ |
| **Pearson r** | 皮尔逊相关系数 | 基因水平相关性 |
| **R²** | 决定系数 | 解释方差比例 |
| **Top-k 准确率** | 差异基因预测 | 前k个差异基因重叠 |

### 下游任务指标
- **细胞类型分类准确率**
- **通路富集显著性**
- **遗传相互作用预测 AUC**

## 引用格式

```bibtex
@article{norman2019exploring,
  title={Exploring genetic interaction manifolds constructed from rich single-cell phenotypes},
  author={Norman, Thomas M and Horlbeck, Max A and Replogle, Joseph M and others},
  journal={Science},
  volume={365},
  number={6455},
  pages={786--793},
  year={2019}
}
```

## 相关资源

- **论文**: https://science.sciencemag.org/content/365/6455/786
- **代码**: https://github.com/thomasmaxwellnorman/Perturb-seq
- **数据**: GEO GSE146194
- **教程**: Weissman Lab 教程

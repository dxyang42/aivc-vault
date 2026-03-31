# GNN 在基因网络中的应用

## 概述

图神经网络 (Graph Neural Network, GNN) 是一类处理图结构数据的神经网络。在基因网络分析中，GNN 被用于建模基因间的相互作用、推断基因调控网络、预测基因功能和药物靶点。

## 核心概念

### 图表示
基因网络表示为图 G = (V, E)：
- **节点 (V)**: 基因、蛋白质或其他分子
- **边 (E)**: 相互作用关系（调控、结合、共表达等）

### 节点特征
每个节点 v_i 有特征向量 x_i ∈ R^d：
- 基因表达值
- 基因本体 (GO) 注释
- 序列特征
- 蛋白质结构特征

### 边类型
| 边类型 | 描述 | 数据来源 |
|--------|------|----------|
| **调控关系** | TF → 靶基因 | ChIP-seq, ATAC-seq |
| **蛋白质相互作用** | PPI | 质谱、酵母双杂交 |
| **共表达** | 表达相关性 | 大规模转录组 |
| **通路关系** | 同通路成员 | KEGG, Reactome |
| **空间邻近** | 基因组位置 | Hi-C, 空间转录组 |

## 消息传递机制

### 基本框架
GNN 通过迭代的消息传递更新节点表示：

```
h_i^(l+1) = UPDATE^(l)(h_i^(l), AGGREGATE^(l)({h_j^(l): j ∈ N(i)}))
```

其中：
- h_i^(l): 节点 i 在第 l 层的表示
- N(i): 节点 i 的邻居集合
- AGGREGATE: 聚合邻居信息（如求和、平均、最大池化）
- UPDATE: 更新节点表示

### 聚合函数

| 聚合方式 | 公式 | 特点 |
|----------|------|------|
| **Mean** | (1/|N(i)|) * Σ_{j ∈ N(i)} h_j | 平滑，常用 |
| **Sum** | Σ_{j ∈ N(i)} h_j | 保留度数信息 |
| **Max** | max_{j ∈ N(i)} h_j | 捕获最显著特征 |

## GNN 架构变体

### 1. Graph Convolutional Network (GCN)

#### 公式
```
H^(l+1) = σ(D̃^(-1/2) · Ã · D̃^(-1/2) · H^(l) · W^(l))
```

其中：
- Ã = A + I: 加自环的邻接矩阵
- D̃: 度矩阵（对角矩阵，D̃_ii = Σ_j Ã_ij）
- W^(l): 第 l 层的可学习参数矩阵
- σ: 激活函数（如 ReLU）

#### 特点
- 谱域图卷积的简化
- 计算高效
- 适合同质图

### 2. Graph Attention Network (GAT)

#### 注意力机制
```
e_ij = LeakyReLU(a^T · [Wh_i || Wh_j])
α_ij = exp(e_ij) / Σ_{k ∈ N(i)} exp(e_ik)
```

其中 a 是注意力参数向量，|| 表示向量拼接，α_ij 是节点 j 对节点 i 的注意力权重。

#### 多头注意力
```
h_i' = ||_{k=1}^{K} σ(Σ_{j ∈ N(i)} α_ij^(k) · W^(k) · h_j)
```

K 个注意力头并行计算，结果拼接后得到最终表示。

#### 在基因网络中的意义
- 学习基因间相互作用的权重
- 识别关键调控边
- 可解释性强

### 3. GraphSAGE

#### 采样与聚合
```python
def graphsage_forward(self, x, adj, num_samples):
    # 采样邻居
    neighbors = sample_neighbors(adj, num_samples)
    
    # 聚合
    h_neigh = aggregate(x[neighbors])
    
    # 拼接并变换
    h = torch.cat([x, h_neigh], dim=-1)
    h = self.linear(h)
    return F.relu(h)
```

#### 特点
- 归纳式学习
- 可处理大图
- 灵活的聚合函数

## 在基因网络中的应用

### 1. 基因功能预测
```python
class GeneFunctionGNN(nn.Module):
    def __init__(self, in_dim, hidden_dim, out_dim, num_layers):
        super().__init__()
        self.convs = nn.ModuleList()
        self.convs.append(GATConv(in_dim, hidden_dim))
        for _ in range(num_layers - 1):
            self.convs.append(GATConv(hidden_dim, hidden_dim))
        self.classifier = nn.Linear(hidden_dim, out_dim)
    
    def forward(self, x, edge_index):
        # 多层 GNN
        for conv in self.convs:
            x = conv(x, edge_index)
            x = F.relu(x)
        
        # 基因功能分类
        return self.classifier(x)
```

### 2. 基因调控网络推断
- 输入: 基因表达数据
- 输出: 基因间调控关系
- 方法: 边预测任务

### 3. 药物靶点预测
- 构建药物-靶点-疾病图
- 预测新的药物-靶点相互作用
- 药物重定位

### 4. 单细胞数据整合

#### 细胞-基因二分图
```
细胞 --[表达关系]--> 基因
```

#### 应用
- 细胞类型注释
- 批次校正
- 缺失值插补

## 空间转录组中的 GNN

### 空间图构建
- 节点: 空间位置/细胞
- 边: 空间邻近关系

### 应用
- 空间域识别
- 细胞间通讯推断
- 组织结构分析

### 代码示例
```python
class SpatialGNN(nn.Module):
    def __init__(self, in_dim, hidden_dim, out_dim):
        super().__init__()
        # 空间邻接图卷积
        self.conv1 = GCNConv(in_dim, hidden_dim)
        self.conv2 = GCNConv(hidden_dim, out_dim)
    
    def forward(self, x, spatial_adj):
        # x: 基因表达
        # spatial_adj: 空间邻接矩阵
        x = F.relu(self.conv1(x, spatial_adj))
        x = self.conv2(x, spatial_adj)
        return x
```

## 与 Transformer 的结合

### Graph Transformer
结合 GNN 和 Transformer 的优势：

```python
class GraphTransformerLayer(nn.Module):
    def __init__(self, d_model, n_heads):
        super().__init__()
        self.attention = MultiHeadAttention(d_model, n_heads)
        self.gnn = GATConv(d_model, d_model)
    
    def forward(self, x, edge_index, adj_mask):
        # 自注意力（全局）
        x = self.attention(x, x, x, adj_mask)
        
        # GNN（局部图结构）
        x = self.gnn(x, edge_index)
        return x
```

### 基因 Transformer
- 将基因视为序列（Transformer）
- 同时考虑基因网络结构（GNN）
- 结合序列和图信息

## 关键模型

| 模型 | 架构 | 应用 |
|------|------|------|
| **DeepWalk** | 随机游走 + Word2Vec | 网络嵌入 |
| **node2vec** | 有偏随机游走 | 基因功能预测 |
| **GCN** | 图卷积 | 基因分类 |
| **GAT** | 图注意力 | 调控网络 |
| **GraphSAGE** | 采样聚合 | 大规模网络 |
| **GNNExplainer** | 解释性 GNN | 调控关系解释 |

## 关键论文

1. **Kipf & Welling (2017)** "Semi-Supervised Classification with Graph Convolutional Networks" - *ICLR*
   - GCN 奠基论文

2. **Veličković et al. (2018)** "Graph Attention Networks" - *ICLR*
   - GAT，注意力机制引入

3. **Hamilton et al. (2017)** "Inductive Representation Learning on Large Graphs" - *NeurIPS*
   - GraphSAGE

4. **Zitnik et al. (2018)** "Modeling polypharmacy side effects with graph convolutional networks" - *Bioinformatics*
   - GNN 在生物医学中的应用

## 挑战与方向

### 挑战
1. **图构建**: 如何从数据构建高质量的基因网络
2. **异质性**: 基因网络包含多种类型的节点和边
3. **可解释性**: 理解 GNN 学到的生物学意义
4. **规模**: 全基因组网络规模巨大

### 发展方向
- **异质图网络**: 处理多类型节点/边
- **动态图**: 时序基因网络
- **因果推断**: 从相关性到因果性
- **多组学整合**: 结合基因组、转录组、蛋白质组

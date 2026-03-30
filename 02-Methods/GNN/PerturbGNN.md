---
title: PerturbGNN
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - graph-neural-network
  - knowledge-graph
  - single-cell
  - 2025
---

# PerturbGNN

> Graph Neural Networks for Gene-Level Perturbation Effect Propagation

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | PerturbGNN |
| **发表年份** | 2025 |
| **核心算法** | Graph Neural Network + Message Passing |
| **任务类型** | 基于基因网络的扰动效应传播预测 |

## 核心思想

PerturbGNN利用**图神经网络(Graph Neural Network)**在基因调控网络上传播扰动效应。通过将基因表示为节点、调控关系表示为边，该方法能够建模扰动在生物网络中的级联传播效应。

### 关键创新

1. **网络感知**: 利用先验基因调控知识
2. **消息传递**: 模拟扰动信号的传播
3. **层次聚合**: 多尺度捕获网络效应
4. **可解释性**: 识别关键调控路径

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                       PerturbGNN                            │
│              Graph Neural Network Model                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input:                                                    │
│   ├─ Gene expression: x ∈ R^{n×d}  (n genes)               │
│   ├─ Gene interaction graph: G = (V, E, A)                 │
│   │   ├─ V: Genes as nodes                                 │
│   │   ├─ E: Regulatory edges                               │
│   │   └─ A: Adjacency matrix                               │
│   └─ Perturbation targets: P ⊂ V                           │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Node Feature Initialization                │   │
│   │                                                      │   │
│   │   For each gene i:                                   │   │
│   │   ├─ h_i^0 = MLP_init(x_i)                          │   │
│   │   ├─ If i ∈ P: h_i^0 += Embed_pert(pert_type)       │   │
│   │   └─ Node features: H^0 = [h_1^0, ..., h_n^0]       │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Message Passing Layers (K layers)          │   │
│   │                                                      │   │
│   │   For k = 1 to K:                                    │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Message Aggregation                         │   │   │
│   │   │                                              │   │   │
│   │   │  For each node i:                            │   │   │
│   │   │  m_i^k = AGGREGATE({MSG(h_i^{k-1}, h_j^{k-1}) │   │   │
│   │   │                     : j ∈ N(i)})            │   │   │
│   │   │                                              │   │   │
│   │   │  MSG options:                                │   │   │
│   │   │  ├─ GCN: Normalize(AH^{k-1}W)               │   │   │
│   │   │  ├─ GAT: Attention-weighted messages        │   │   │
│   │   │  └─ GraphSAGE: Sample & aggregate           │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Node Update                                 │   │   │
│   │   │                                              │   │   │
│   │   │  h_i^k = UPDATE(h_i^{k-1}, m_i^k)           │   │   │
│   │   │                                              │   │   │
│   │   │  UPDATE options:                             │   │   │
│   │   │  ├─ Sum: h_i^{k-1} + m_i^k                  │   │   │
│   │   │  ├─ Concat: MLP([h_i^{k-1}; m_i^k])         │   │   │
│   │   │  └─ GRU: GRU(h_i^{k-1}, m_i^k)              │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Readout & Prediction                       │   │
│   │                                                      │   │
│   │   Node-level prediction:                             │   │
│   │   ├─ Δx_i = MLP_readout(h_i^K)                      │   │
│   │                                                      │   │
│   │   Graph-level (optional):                            │   │
│   │   └─ h_graph = GLOBAL_POOL(H^K)                     │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Predicted perturbation effects per gene           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 技术细节

### 图卷积 (GCN)

图卷积网络通过在图上执行谱域卷积来聚合邻居信息。

**核心操作**:
1. 添加自环: 在邻接矩阵对角线上添加1，使节点包含自身信息
2. 度矩阵归一化: 根据节点度数对特征进行归一化，防止度数高的节点主导
3. 特征传播: 聚合邻居特征并与可学习权重相乘
4. 非线性激活: 应用激活函数引入非线性

**特点**: 计算高效，但所有邻居权重相同

### 图注意力 (GAT)

图注意力网络为不同邻居分配可学习的注意力权重。

**注意力机制**:
1. 线性变换: 将节点特征映射到注意力空间
2. 注意力分数计算: 使用可学习向量计算节点对之间的注意力分数
3. LeakyReLU激活: 引入非线性并允许负值
4. Softmax归一化: 将分数转换为概率分布
5. 加权聚合: 用注意力权重加权邻居特征

**优势**: 可以学习关注重要的邻居，可解释性更强

### 训练目标

**预测损失**: 预测扰动后表达与真实值的均方误差
- 计算每个基因预测值与真实值的差异
- 对所有基因求和平均

**正则化**: L2权重正则化防止过拟合
- 惩罚大的权重值
- lambda参数控制正则化强度

**总损失**: 预测损失 + 正则化项

## 数据集

| 数据集 | 网络类型 | 节点数 | 边数 |
|--------|----------|--------|------|
| STRING | PPI | 20K | 11M |
| RegNetwork | Regulatory | 20K | 500K |
| HumanNet | Functional | 18K | 1M |
| Custom | Integrated | 20K | 2M |

## 实验结果

### 预测性能

| 方法 | MSE ↓ | Pearson ↑ | Pathway Acc ↑ |
|------|-------|-----------|---------------|
| PerturbGNN (GAT) | 0.14 | 0.86 | 0.82 |
| PerturbGNN (GCN) | 0.16 | 0.84 | 0.79 |
| GEARS | 0.18 | 0.82 | 0.75 |
| Linear | 0.28 | 0.72 | 0.65 |

### 可解释性

| 方法 | Pathway Recovery ↑ | Top-K Precision ↑ |
|------|-------------------|-------------------|
| PerturbGNN | 0.78 | 0.85 |
| GEARS | 0.72 | 0.80 |

## 优势与局限

### 优势
- ✅ 利用先验生物学知识
- ✅ 可解释的网络效应
- ✅ 捕获级联传播
- ✅ 适用于稀疏数据

### 局限
- ❌ 依赖网络质量
- ❌ 可能传播噪声
- ❌ 深层网络过平滑

## 相关方法

- [[GEARS]] - GNN with knowledge graph
- [[Cell-Oracle]] - GRN-based
- [[Celcomen]] - GNN approach
- [[SCENIC]] - Network inference

## 引用

```bibtex
@article{perturbgnn2025,
  title={Graph Neural Networks for Gene-Level Perturbation Effect Propagation},
  journal={Bioinformatics},
  year={2025}
}
```

## 外部链接

- 相关: [[GNN]] | [[Message-Passing]] | [[Gene-Network]] | [[Knowledge-Graph]]

---
title: "GeneMamba"
authors: "Research team"
year: 2025
institution: "arXiv"
---

# GeneMamba

## 核心思想

GeneMamba是一种高效且有效的单细胞数据基础模型，针对Transformer架构计算效率低和可扩展性差的问题，引入了BiMamba模块来高效捕获基因上下文信息。该模型采用生物学启发的损失函数，在保持性能的同时显著提高了计算效率。

GeneMamba的核心创新在于使用Mamba架构替代传统的Transformer架构。Mamba是一种状态空间模型（State Space Model），具有线性复杂度，相比Transformer的二次复杂度更加高效。BiMamba模块通过双向建模，能够捕获基因序列中的前后依赖关系。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                    GeneMamba Architecture                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Motivation: Efficiency vs. Performance                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Transformer: O(n²) complexity per layer                │    │
│  │  Mamba: O(n) complexity per layer                       │    │
│  │  Result: Linear scaling with sequence length            │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Input: Gene Expression Sequence                                 │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Gene 1 → Gene 2 → Gene 3 → ... → Gene N                │    │
│  │   0.5     0.2     0.7            0.3                    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Gene Token Embedding                        │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Expression Value → Continuous Embedding        │    │    │
│  │  │  + Positional Encoding                          │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              BiMamba Blocks (Stacked)                    │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Block 1: Bidirectional Mamba Layer             │    │    │
│  │  │  ┌─────────────────────────────────────────┐    │    │    │
│  │  │  │  Forward Mamba  │  Backward Mamba      │    │    │    │
│  │  │  │  (Left-to-Right)│  (Right-to-Left)     │    │    │    │
│  │  │  │       ↓         │        ↓             │    │    │    │
│  │  │  │  State Space    │   State Space        │    │    │    │
│  │  │  │  Transformation │   Transformation     │    │    │    │
│  │  │  │       ↓         │        ↓             │    │    │    │
│  │  │  │  Concat + Linear Projection           │    │    │    │
│  │  │  └─────────────────────────────────────────┘    │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Block 2-N: Repeat BiMamba Structure            │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │           Biology-Inspired Loss Function                 │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  • Gene Expression Reconstruction Loss          │    │    │
│  │  │  • Cell Type Classification Loss                │    │    │
│  │  │  • Gene Co-expression Preservation Loss         │    │    │
│  │  │  • Pathway Activity Consistency Loss            │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  Output: Gene Representations & Cell Embeddings                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 核心创新

- **BiMamba模块**: 双向状态空间模型，高效捕获基因上下文
- **线性复杂度**: O(n)复杂度，相比Transformer的O(n²)更高效
- **生物学启发损失**: 结合基因共表达和通路活性的多任务损失
- **可扩展性**: 更好的长序列建模能力

## 优缺点分析

### 优点

- **计算效率高**: 线性复杂度，适合长序列建模
- **内存友好**: 相比Transformer显著降低内存需求
- **双向建模**: BiMamba捕获前后文依赖关系
- **可扩展性强**: 能够处理更长的基因序列

### 缺点

- **新兴架构**: Mamba架构相对较新，稳定性待验证
- **生态不成熟**: 相比Transformer，工具和预训练资源较少
- **调优挑战**: 状态空间模型的超参数调优需要经验
- **长程依赖**: 某些长程依赖捕获能力可能不如Transformer

## 相关方法

- [[scBERT]] - 基于Transformer的单细胞BERT
- [[scGPT]] - 基于Transformer的单细胞GPT
- [[CellFM]] - 大规模Transformer基础模型
- [[STACK]] - Arc Institute的表格注意力模型

## 资源链接

- **论文**: GeneMamba: An Efficient and Effective Foundation Model on Single Cell Data (arXiv, 2025)
- **链接**: https://arxiv.org/abs/2504.16956

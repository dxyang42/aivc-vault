---
title: "CellCap"
authors: "Xu and Fleming et al."
year: 2025
institution: "Cell Systems"
---

# CellCap

## 核心思想

CellCap是一种基于注意力机制的可解释单细胞扰动数据建模方法。该方法通过稀疏字典学习在潜在空间中将细胞状态特异的扰动响应解构为一组转录响应程序（Transcriptional Response Programs, TRPs），利用注意力机制捕获细胞状态与扰动响应之间的对应关系。

CellCap的核心创新在于其可解释性设计。传统的深度学习方法通常是黑盒模型，难以解释预测结果。CellCap通过稀疏字典学习将扰动响应分解为可解释的转录程序，每个程序对应一组协同调控的基因，从而提供生物学可解释的预测结果。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                     CellCap Architecture                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Core Concept: Sparse Dictionary Learning                        │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Perturbation Response = Σ (Coefficient × Response Program)│   │
│  │  (Sparse linear combination of transcriptional programs)  │   │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Input: Control Cell + Perturbation                              │
│  ┌─────────────────┐    ┌─────────────────┐                     │
│  │  Control Cell   │    │ Perturbation    │                     │
│  │  Gene Expression│    │  Information    │                     │
│  └────────┬────────┘    └────────┬────────┘                     │
│           │                      │                              │
│           └──────────┬───────────┘                              │
│                      ↓                                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Encoder Network                             │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Input: Gene Expression Vector                  │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Neural Encoder (MLP/CNN)                       │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Latent Cell State Representation               │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                      │                                          │
│                      ▼                                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │           Sparse Dictionary of TRPs                      │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Dictionary: K Transcriptional Response Programs│    │    │
│  │  │  ┌─────────┐  ┌─────────┐  ┌─────────┐         │    │    │
│  │  │  │  TRP_1  │  │  TRP_2  │  │  TRP_K  │         │    │    │
│  │  │  │[Gene   ]│  │[Gene   ]│  │[Gene   ]│         │    │    │
│  │  │  │[Weights]│  │[Weights]│  │[Weights]│         │    │    │
│  │  │  └─────────┘  └─────────┘  └─────────┘         │    │    │
│  │  │  Each TRP = pattern of co-regulated genes      │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                      │                                          │
│                      ▼                                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Attention Mechanism                         │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Query: Cell State Representation               │    │    │
│  │  │  Keys: TRP Embeddings                           │    │    │
│  │  │  Values: TRP Gene Weight Vectors                │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Attention Weights (Sparse)                     │    │    │
│  │  │  [0.8, 0.0, 0.2, 0.0, ..., 0.0]  ← Sparse!     │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Weighted Sum of TRPs = Predicted Response      │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                      │                                          │
│                      ▼                                          │
│  Output: Predicted Perturbed Expression + Interpretable TRPs     │
│                                                                  │
│  Interpretability                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  • Which TRPs are activated? (Attention weights)        │    │
│  │  • Which genes are involved? (TRP composition)          │    │
│  │  • How strong is each program's contribution?           │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 核心机制

1. **稀疏字典学习**: 学习一组转录响应程序（TRPs）
2. **注意力机制**: 根据细胞状态选择相关的TRPs
3. **可解释性**: 每个预测都可分解为可解释的TRPs
4. **细胞状态感知**: 不同细胞类型激活不同的TRPs

## 优缺点分析

### 优点

- **高度可解释**: 预测结果可分解为生物学意义的TRPs
- **细胞状态感知**: 能够捕获细胞类型特异的响应
- **稀疏性**: 稀疏表示更符合生物学实际
- **模块化**: TRPs可跨不同扰动和细胞类型复用

### 缺点

- **字典大小**: TRP数量需要预先设定，可能影响性能
- **稀疏约束**: 过度稀疏可能丢失重要信息
- **训练稳定性**: 稀疏字典学习的训练可能不稳定
- **程序注释**: 需要人工注释TRPs的生物学意义

## 相关方法

- [[scPRAM]] - 基于注意力的扰动响应预测
- [[CellCap]] - 可解释的扰动建模
- [[scGen]] - 基于VAE的扰动预测
- [[GEARS]] - 利用基因关系的扰动预测

## 资源链接

- **论文**: Explainable modeling of single-cell perturbation data using attention (Cell Systems, 2025)
- **期刊**: https://www.cell.com/cell-systems

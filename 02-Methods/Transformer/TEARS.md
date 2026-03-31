---
title: "TEARS"
authors: "Stanford CS191"
year: 2025
institution: "Stanford University"
---

# TEARS

## 核心思想

TEARS（Transformer-Enhanced Genetic Perturbation Prediction）是斯坦福大学CS191课程项目开发的遗传扰动预测方法。该方法基于Transformer架构，在单基因扰动预测上实现了19%的Pearson相关性提升和17%的MSE降低，在跨细胞类型迁移学习基准上展示了48%的分布外泛化改进。

TEARS的核心创新在于针对单细胞扰动预测任务优化Transformer架构。通过精心设计的注意力机制和训练策略，模型能够捕获基因之间的复杂调控关系，并有效泛化到未见的细胞类型。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                     TEARS Architecture                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Performance Improvements                                        │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Single-gene perturbation: +19% Pearson correlation     │    │
│  │  MSE reduction: -17%                                    │    │
│  │  Cross-cell-type OOD: +48% improvement                  │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Input: Control Cell + Target Gene                               │
│  ┌─────────────────┐    ┌─────────────────┐                     │
│  │  Control Cell   │    │  Target Gene    │                     │
│  │  Gene Expression│    │  (Perturbation) │                     │
│  │  (20K genes)    │    │  (One gene)     │                     │
│  └────────┬────────┘    └────────┬────────┘                     │
│           │                      │                              │
│           └──────────┬───────────┘                              │
│                      ↓                                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Gene Expression Encoder                     │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Input: Normalized Gene Expression Vector       │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Linear Projection + Layer Norm                 │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Gene Embeddings (learned or pre-trained)       │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                      │                                          │
│                      ▼                                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Perturbation Embedding                      │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Target Gene ID → Embedding Lookup              │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Perturbation Type Embedding (KO/KD/OE)         │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Combined Perturbation Representation           │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                      │                                          │
│                      ▼                                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Transformer Encoder (Stacked)               │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  For each layer:                                │    │    │
│  │  │  ┌─────────────────────────────────────────┐    │    │    │
│  │  │  │  Multi-Head Self-Attention              │    │    │    │
│  │  │  │  • Gene-gene interactions               │    │    │    │
│  │  │  │  • Perturbation-aware attention bias    │    │    │    │
│  │  │  └─────────────────────────────────────────┘    │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  ┌─────────────────────────────────────────┐    │    │    │
│  │  │  │  Feed-Forward Network                   │    │    │    │
│  │  │  │  • Gene-specific transformations        │    │    │    │
│  │  │  └─────────────────────────────────────────┘    │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Layer Normalization + Residual                 │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  │  (Repeat for N layers)                                  │    │
│  └─────────────────────────────────────────────────────────┘    │
│                      │                                          │
│                      ▼                                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Prediction Head                             │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Input: Transformer output for each gene        │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Gene-wise MLP                                  │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Predicted Expression Change (log fold change)  │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                      │                                          │
│                      ▼                                          │
│  Output: Predicted Gene Expression Changes (All genes)           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 关键优化

1. **扰动感知注意力**: 注意力机制考虑扰动信息
2. **跨细胞类型泛化**: 针对分布外场景优化
3. **基因特异性头**: 每个基因有专门的预测头
4. **残差连接**: 稳定深层Transformer训练

## 优缺点分析

### 优点

- **性能提升显著**: 19% Pearson提升，48% OOD改进
- **架构简洁**: 标准Transformer，易于理解和实现
- **泛化能力强**: 跨细胞类型迁移学习表现优异
- **学术验证**: 经过斯坦福课程项目严格评估

### 缺点

- **单基因限制**: 主要针对单基因扰动优化
- **数据需求**: 需要大规模扰动数据进行训练
- **计算资源**: 深层Transformer需要较多计算资源
- **组合扰动**: 未明确优化组合扰动预测

## 相关方法

- [[GEARS]] - 基于GNN的组合扰动预测
- [[scPRAM]] - 基于VAE+OT+注意力的预测
- [[STATE]] - Arc Institute的大规模扰动预测
- [[scFoundation]] - 可作为预训练基础模型

## 资源链接

- **项目页面**: https://cs191.stanford.edu/projects/Fall2025/_Ayush___Agrawal_.pdf
- **论文**: TEARS: Transformer-Enhanced Genetic Perturbation Prediction (Stanford CS191, 2025)

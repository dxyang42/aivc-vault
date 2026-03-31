---
title: "PerturbNet"
authors: "Yu and Qian et al."
year: 2025
institution: "Nature Communications Medicine"
---

# PerturbNet

## 核心思想

PerturbNet是一种灵活的深度生成模型，用于预测未见化学或遗传扰动诱导的细胞状态分布。该模型的核心创新在于首次证明氨基酸序列嵌入可用于预测错义突变诱导的基因表达变化，为理解遗传变异的功能效应提供了新途径。

PerturbNet通过将扰动信息（化学结构或蛋白质序列）嵌入到潜在空间，学习从扰动嵌入到细胞状态变化的映射。这种设计使模型能够泛化到训练期间未见的扰动，实现零样本扰动预测。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                    PerturbNet Architecture                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Key Innovation: Amino Acid Sequence Embedding                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  First to show: AA sequence → Gene expression change    │    │
│  │  Enables zero-shot prediction of missense mutations     │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Input: Two Modalities                                           │
│  ┌──────────────────────────┐    ┌──────────────────────────┐   │
│  │   Chemical Perturbation  │    │   Genetic Perturbation   │   │
│  │   (Molecular Structure)  │    │   (Protein Sequence)     │   │
│  │  ┌────────────────────┐  │    │  ┌────────────────────┐  │   │
│  │  │  SMILES String     │  │    │  │  Amino Acid Seq    │  │   │
│  │  │  or Molecular Graph│  │    │  │  (with mutation)   │  │   │
│  │  └─────────┬──────────┘  │    │  └─────────┬──────────┘  │   │
│  └────────────┼─────────────┘    └────────────┼─────────────┘   │
│               │                               │                  │
│               ▼                               ▼                  │
│  ┌──────────────────────────┐    ┌──────────────────────────┐   │
│  │   Chemistry Encoder      │    │   Sequence Encoder       │   │
│  │  ┌────────────────────┐  │    │  ┌────────────────────┐  │   │
│  │  │  Graph Neural Net  │  │    │  │  Protein Language  │  │   │
│  │  │  or                │  │    │  │  Model (PLM)       │  │   │
│  │  │  Molecular         │  │    │  │  or                │  │   │
│  │  │  Transformer       │  │    │  │  CNN/RNN on AA     │  │   │
│  │  └─────────┬──────────┘  │    │  └─────────┬──────────┘  │   │
│  │            ↓              │    │            ↓              │   │
│  │  Chemical Embedding       │    │  Sequence Embedding       │   │
│  └────────────┬──────────────┘    └────────────┬──────────────┘   │
│               │                               │                  │
│               └───────────────┬───────────────┘                  │
│                               ▼                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Perturbation Embedding Space                │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Unified Latent Representation of Perturbations │    │    │
│  │  │  (Chemical or Genetic → Same embedding space)   │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                               │                                  │
│                               ▼                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Conditional Generator                       │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Input:                                         │    │    │
│  │  │  • Control Cell State (gene expression)         │    │    │
│  │  │  • Perturbation Embedding (from above)          │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Cross-Modal Fusion Network                     │    │    │
│  │  │  ┌─────────────────────────────────────────┐    │    │    │
│  │  │  │  Cell State Encoder                     │    │    │    │
│  │  │  │  + Perturbation-conditioned Decoder     │    │    │    │
│  │  │  └─────────────────────────────────────────┘    │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Predicted Perturbed Cell Distribution          │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                               │                                  │
│                               ▼                                  │
│  Output: Predicted Cell State Distribution                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 核心创新

1. **氨基酸序列嵌入**: 首次证明AA序列可用于预测基因表达变化
2. **零样本预测**: 能够预测未见的化学和遗传扰动
3. **统一框架**: 同时处理化学和遗传扰动
4. **错义突变预测**: 专门优化用于预测错义突变效应

## 优缺点分析

### 优点

- **零样本能力**: 能够预测训练时未见的扰动
- **双模态支持**: 同时处理化学和遗传扰动
- **序列理解**: 直接从序列学习扰动效应
- **临床相关**: 错义突变预测对疾病研究重要

### 缺点

- **序列质量依赖**: 需要准确的蛋白质序列信息
- **结构信息缺失**: 未显式利用蛋白质结构信息
- **训练数据需求**: 需要大量扰动-响应对进行训练
- **计算成本**: 序列编码器计算成本较高

## 相关方法

- [[GEARS]] - 基于基因关系的组合扰动预测
- [[CPA]] - 化学和遗传扰动预测
- [[scGen]] - 基于VAE的扰动预测
- [[CellOT]] - 基于最优传输的扰动预测

## 资源链接

- **论文**: PerturbNet predicts single-cell responses to unseen chemical and genetic perturbations (Nature Communications Medicine, 2025)

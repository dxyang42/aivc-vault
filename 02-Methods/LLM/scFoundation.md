---
title: "scFoundation"
authors: "BioMap, Tsinghua University, MBZUAI"
year: 2024
institution: "Nature Methods"
---

# scFoundation

## 核心思想

scFoundation是由BioMap、清华大学和MBZUAI联合开发的大规模单细胞基础模型。该模型拥有1亿参数，覆盖约2万个基因，在5000多万单细胞转录组数据上进行预训练。scFoundation支持基因表达增强、药物响应预测和单细胞扰动预测等多样化任务。

scFoundation的核心创新在于其针对单细胞数据的专门设计。模型采用非对称编码器-解码器架构，编码器处理完整的基因表达谱，解码器则专注于重建被掩码的基因。这种设计使模型能够学习到丰富的基因表达模式，并支持多种下游应用。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                    scFoundation Architecture                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Model Specifications                                            │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Parameters: 100 Million                                │    │
│  │  Genes Covered: ~20,000                                 │    │
│  │  Training Data: 50+ Million Cells                       │    │
│  │  Architecture: Asymmetric Encoder-Decoder               │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Input: Gene Expression Vector (20K genes)                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  [0.5, 0.2, 0.7, ..., 0.3]  ← Full expression profile  │    │
│  │       ↓                                                   │    │
│  │  Random Masking: Select subset of genes to predict      │    │
│  │  [0.5, MASK, 0.7, ..., MASK]  ← Masked input            │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Encoder (Non-autoregressive)                │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Input: All genes (masked + unmasked)           │    │    │
│  │  │  ┌─────────────────────────────────────────┐    │    │    │
│  │  │  │  Transformer Layers (Large capacity)    │    │    │    │
│  │  │  │  • Self-attention over all genes        │    │    │    │
│  │  │  │  • Captures global expression patterns  │    │    │    │
│  │  │  │  • Learns gene-gene relationships       │    │    │    │
│  │  │  └─────────────────────────────────────────┘    │    │    │
│  │  │  Output: Contextualized gene representations    │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Decoder (Lightweight)                       │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Input: Encoder outputs for masked positions    │    │    │
│  │  │  ┌─────────────────────────────────────────┐    │    │    │
│  │  │  │  Prediction Head                        │    │    │    │
│  │  │  │  • Reconstructs masked gene values      │    │    │    │
│  │  │  │  • Gene expression prediction           │    │    │    │
│  │  │  └─────────────────────────────────────────┘    │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  Output: Reconstructed Gene Expression                           │
│                                                                  │
│  Downstream Tasks                                                │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │Gene Expression│  │Drug Response│  │ Perturbation│              │
│  │ Enhancement  │  │ Prediction  │  │ Prediction  │              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 训练数据与规模

- **模型参数量**: 1亿
- **基因覆盖**: 约20,000个基因
- **训练数据量**: 5000多万个单细胞
- **架构**: 非对称编码器-解码器

## 下游任务

1. **基因表达增强**: 提高低质量数据的基因表达估计
2. **药物响应预测**: 预测细胞对药物处理的响应
3. **扰动预测**: 预测基因扰动后的细胞状态变化
4. **细胞注释**: 自动识别细胞类型

## 优缺点分析

### 优点

- **多任务支持**: 单一模型支持多种下游任务
- **非对称设计**: 编码器-解码器的非对称设计提高效率
- **大规模预训练**: 5000万细胞的训练提供丰富的先验知识
- **基因覆盖广**: 覆盖约2万个基因，涵盖大部分人类基因组

### 缺点

- **掩码策略依赖**: 性能受掩码策略影响
- **计算资源**: 1亿参数模型需要一定计算资源
- **物种局限**: 主要在人类数据上训练
- **任务适配**: 某些任务需要额外的微调

## 相关方法

- [[CellFM]] - 8亿参数的大规模基础模型
- [[GeneCompass]] - 跨物种基础模型
- [[scGPT]] - 单细胞GPT模型
- [[Geneformer]] - 基因Transformer模型

## 资源链接

- **模型仓库**: https://huggingface.co/genbio-ai/scFoundation
- **论文**: Large-scale foundation model on single-cell transcriptomics (Nature Methods, 2024)

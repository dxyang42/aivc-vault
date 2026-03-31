---
title: "scLAMBDA"
authors: "Wang et al., Yale University"
year: 2024
institution: "Yale University"
---

# scLAMBDA

## 核心思想

scLAMBDA（Single-cell Language Model for perturbation Analysis with Decoupled Architecture）是一种用于建模和预测单细胞多基因扰动响应的方法。该方法利用大语言模型（LLM）的嵌入能力，有效整合先验基因知识，将基础细胞状态与扰动特异性表示解耦。

scLAMBDA的核心创新在于其解耦架构设计。模型将细胞表示分解为两部分：基础细胞状态（与扰动无关）和扰动特异性表示。这种解耦使模型能够更好地泛化到未见的扰动组合，并理解扰动之间的相互作用。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                    scLAMBDA Architecture                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Core Concept: Decoupled Representation                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Cell Representation = Base State ⊕ Perturbation Effect │    │
│  │  (Disentangled factors enable better generalization)    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Input: Control Cell + Multi-gene Perturbation                   │
│  ┌─────────────────┐    ┌─────────────────────────────┐         │
│  │  Control Cell   │    │  Multi-gene Perturbation    │         │
│  │  Gene Expression│    │  [Gene A, Gene B, ...]      │         │
│  └────────┬────────┘    └─────────────┬───────────────┘         │
│           │                          │                          │
│           │                          ▼                          │
│           │            ┌─────────────────────────┐              │
│           │            │  LLM Gene Embeddings    │              │
│           │            │  (Pre-trained LLM)      │              │
│           │            │  ┌─────────────────┐    │              │
│           │            │  │ Gene A → Embed  │    │              │
│           │            │  │ Gene B → Embed  │    │              │
│           │            │  │ ...             │    │              │
│           │            │  └─────────────────┘    │              │
│           │            └───────────┬─────────────┘              │
│           │                        │                            │
│           │                        ▼                            │
│           │            ┌─────────────────────────┐              │
│           │            │  Perturbation Encoder   │              │
│           │            │  (Aggregation + MLP)    │              │
│           │            │  ↓                      │              │
│           │            │  Perturbation Embedding │              │
│           │            └─────────────────────────┘              │
│           │                        │                            │
│           └────────────────────────┼────────────────────────────┘
│                                    ▼                            │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Dual-Branch Encoder                         │    │
│  │  ┌────────────────────────┐  ┌────────────────────────┐ │    │
│  │  │   Base State Branch    │  │ Perturbation Branch    │ │    │
│  │  │  ┌──────────────────┐  │  │  ┌──────────────────┐  │ │    │
│  │  │  │ Control Cell     │  │  │  │ Perturbation     │  │ │    │
│  │  │  │ Expression       │  │  │  │ Embedding        │  │ │    │
│  │  │  │ ↓                │  │  │  │ ↓                │  │ │    │
│  │  │  │ Base Encoder     │  │  │  │ Effect Encoder   │  │ │    │
│  │  │  │ ↓                │  │  │  │ ↓                │  │ │    │
│  │  │  │ Base State z_b   │  │  │  │ Perturb Effect   │  │ │    │
│  │  │  │                  │  │  │  │ z_p              │  │ │    │
│  │  │  └──────────────────┘  │  │  └──────────────────┘  │ │    │
│  │  └────────────────────────┘  └────────────────────────┘ │    │
│  └─────────────────────────────────────────────────────────┘    │
│                      │            │                             │
│                      └─────┬──────┘                             │
│                            ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Representation Fusion                       │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Combined Representation: [z_b || z_p]          │    │    │
│  │  │  (Concatenation or learned fusion)              │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                            │                                    │
│                            ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │              Decoder / Predictor                         │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Input: Fused Representation                    │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  MLP / Transformer Decoder                      │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Predicted Perturbed Gene Expression            │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                            │                                    │
│                            ▼                                    │
│  Output: Predicted Multi-gene Perturbation Response              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 核心机制

1. **LLM嵌入**: 利用预训练LLM的基因嵌入
2. **解耦表示**: 分离基础状态和扰动效应
3. **多基因支持**: 支持多基因组合扰动预测
4. **知识整合**: LLM提供丰富的基因功能知识

## 优缺点分析

### 优点

- **解耦设计**: 分离基础和扰动表示，提高泛化能力
- **LLM知识**: 利用预训练LLM的丰富知识
- **多基因支持**: 支持组合扰动预测
- **可解释性**: 解耦表示提供一定的可解释性

### 缺点

- **LLM依赖**: 性能受预训练LLM质量影响
- **解耦挑战**: 完全解耦基础和扰动表示存在挑战
- **融合策略**: 表示融合策略需要仔细设计
- **训练复杂**: 多任务训练增加复杂度

## 相关方法

- [[GEARS]] - 基于GNN的组合扰动预测
- [[Geneformer]] - 基于LLM的基因表示
- [[GenePT]] - 基因预训练Transformer
- [[scFoundation]] - 单细胞基础模型

## 资源链接

- **代码仓库**: https://github.com/gefeiwang/scLAMBDA
- **论文**: Modeling and predicting single-cell multi-gene perturbation responses with scLAMBDA (Yale, 2024)

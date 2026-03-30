---
title: PerturbNet
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - generative-ai
  - multi-modal
  - single-cell
  - 2025
  - nature-medicine
---

# PerturbNet

> PerturbNet predicts single-cell responses to unseen chemical and genetic perturbations

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | PerturbNet |
| **发表年份** | 2025 |
| **期刊** | Nature Communications Medicine |
| **论文链接** | https://link.springer.com/article/10.1038/s44320-025-00131-3 |
| **核心算法** | Generative AI + Multi-modal Fusion |
| **任务类型** | 多模态扰动效应预测 |

## 核心思想

PerturbNet是一个**生成式AI模型**，能够预测细胞状态(整体基因表达变化)对多种未见细胞扰动的响应。它使用灵活的"mix and match"神经网络框架，统一处理化学、基因敲低、基因过表达和DNA序列突变等多种扰动类型。

### 关键创新

1. **多模态统一**: 单一模型处理化学、遗传等多种扰动
2. **生成式建模**: 预测细胞状态的分布变化
3. **模块化架构**: "Mix and match"神经网络组件
4. **零样本泛化**: 预测未见扰动的效应

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                      PerturbNet                             │
│              Multi-modal Generative Model                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: (Multiple perturbation types)                      │
│   ├─ Chemical: SMILES string / molecular fingerprint       │
│   ├─ Gene KD: gRNA sequence / target gene ID               │
│   ├─ Gene OE: Promoter + gene sequence                     │
│   └─ DNA Mutation: Sequence + position                     │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Perturbation Encoders (Modular)              │   │
│   │                                                      │   │
│   │   Chemical Encoder                                   │   │
│   │   ├─ Molecular graph encoder / ChemBERTa            │   │
│   │   └─ z_chem = Encoder_chem(SMILES)                  │   │
│   │                                                      │   │
│   │   Genetic Encoder                                    │   │
│   │   ├─ Sequence encoder (CNN/Transformer)             │   │
│   │   └─ z_genetic = Encoder_genetic(sequence)          │   │
│   │                                                      │   │
│   │   Unified Perturbation Space                         │   │
│   │   └─ z_p = Project(z_chem or z_genetic)             │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Cell State Encoder                         │   │
│   │   ├─ Pre-perturbation expression x                  │   │
│   │   └─ z_cell = Encoder_cell(x)                       │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Conditional Generator                      │   │
│   │                                                      │   │
│   │   z_combined = Fusion(z_cell, z_p)                  │   │
│   │                                                      │   │
│   │   Decoder:                                           │   │
│   │   ├─ delta_x = Generator(z_combined)                │   │
│   │   └─ x_post = x_pre + delta_x                       │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Post-perturbation cell state distribution         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 多模态编码

**Chemical perturbation:**
化学扰动通过图编码器处理分子图表示，学习分子的结构特征嵌入。

**Genetic perturbation:**
遗传扰动通过序列编码器处理基因序列，捕获序列模式和功能信息。

**Unified representation:**
统一表示通过线性投影将不同模态的嵌入映射到共享空间，实现跨模态对齐。

### 条件生成

**Cell state encoding:**
细胞状态编码器将扰动前的基因表达映射到低维隐空间，捕获细胞的初始状态。

**Conditional fusion:**
条件融合将细胞状态隐变量与扰动隐变量结合，形成条件生成器的输入。

**Perturbation effect prediction:**
扰动效应预测通过生成器输出基因表达的变化量，加上原始表达得到扰动后预测。

### 训练目标

**Reconstruction loss:**
重构损失是扰动后真实表达与预测表达之间的期望平方误差，确保生成质量。

**Adversarial loss (optional):**
对抗损失通过判别器区分真实和生成的扰动后表达，提升生成样本的真实性。

**Total loss:**
总损失是重构损失、对抗损失和正则化项的加权和，平衡各项优化目标。

## 数据集

| 数据集 | 扰动类型 | 细胞数 | 描述 |
|--------|----------|--------|------|
| sci-Plex | Chemical | ~1M | 化合物筛选 |
| Perturb-seq | Gene KD | ~100K | CRISPR敲低 |
| Overexpression | Gene OE | ~50K | 基因过表达 |
| Mutation | DNA | ~20K | 点突变 |

## 实验结果

### 跨模态预测性能

| 训练 → Test | Chemical | Gene KD | Gene OE | Mutation |
|-------------|----------|---------|---------|----------|
| Chemical | 0.85 | 0.72 | 0.68 | 0.65 |
| Gene KD | 0.70 | 0.88 | 0.75 | 0.70 |
| Multi-modal | 0.87 | 0.90 | 0.86 | 0.82 |

### 零样本预测

| 场景 | Accuracy |
|------|----------|
| Unseen chemicals | 78% |
| Unseen genes | 82% |
| Novel combinations | 75% |

## 优势与局限

### 优势
- ✅ 统一多模态扰动建模
- ✅ 零样本泛化能力强
- ✅ 模块化架构易于扩展
- ✅ 生成式建模捕获分布变化

### 局限
- ❌ 需要大量多模态训练数据
- ❌ 不同模态间的对齐挑战
- ❌ 复杂架构训练困难

## 相关方法

- [[CPA]] - Chemical perturbation modeling
- [[GEARS]] - Genetic perturbation prediction
- [[DrugCell]] - Drug response prediction
- [[Cell-Oracle]] - GRN-based approach

## 引用

```bibtex
@article{perturbnet2025,
  title={PerturbNet predicts single-cell responses to unseen chemical and genetic perturbations},
  journal={Nature Communications Medicine},
  volume={2},
  article={131},
  year={2025}
}
```

## 外部链接

- 论文: https://link.springer.com/article/10.1038/s44320-025-00131-3
- 相关: [[Multi-modal-Learning]] | [[Generative-AI]] | [[Chemical-Perturbation]] | [[Genetic-Perturbation]]

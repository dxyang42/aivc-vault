---
title: CellCap
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - generative-model
  - probabilistic-model
  - single-cell
  - 2025
  - cell-systems
---

# CellCap

> Explainable modeling of single-cell perturbation data using attention mechanisms

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | CellCap |
| **发表年份** | 2025 |
| **期刊** | Cell Systems |
| **论文链接** | https://www.cell.com/cell-systems/fulltext/S2405-4712(25)00078-X |
| **核心算法** | Probabilistic Generative Model + Attention |
| **任务类型** | 单细胞扰动响应分析与预测 |

## 核心思想

CellCap是一个**概率生成模型**，用于分析单细胞扰动数据。它能够建模细胞状态特异性的扰动响应，并将扰动响应分解为转录程序(transcriptional programs)的集合。

### 关键创新

1. **状态特异性建模**: 捕获不同细胞状态下的差异化扰动响应
2. **转录程序分解**: 将复杂的扰动响应分解为可解释的转录程序
3. **注意力机制**: 使用注意力实现可解释性
4. **概率框架**: 提供不确定性估计

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                        CellCap                              │
│              Probabilistic Generative Model                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input: Single-cell perturbation dataset                   │
│   ├─ Gene expression matrix X (n cells × g genes)           │
│   ├─ Perturbation labels P                                  │
│   └─ Cell state annotations S (optional)                    │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Latent Variable Model                      │   │
│   │                                                      │   │
│   │   Cell State Encoder                                 │   │
│   │   ├─ z_s ~ q(z_s | x)   [Cell state latent]        │   │
│   │                                                      │   │
│   │   Perturbation Encoder                               │   │
│   │   ├─ z_p ~ q(z_p | p)   [Perturbation latent]      │   │
│   │                                                      │   │
│   │   Attention Fusion                                   │   │
│   │   ├─ A = Attention(z_s, z_p)                        │   │
│   │   └─ z_f = Fusion(z_s, z_p, A)                      │   │
│   │                                                      │   │
│   │   Transcriptional Program Decoder                    │   │
│   │   ├─ T_1, T_2, ..., T_k = Programs(z_f)             │   │
│   │   └─ x' = sum over i of w_i · T_i                   │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output:                                                   │
│   ├─ Predicted post-perturbation expression                 │
│   ├─ Transcriptional programs {T_i}                         │
│   ├─ Program weights {w_i}                                  │
│   └─ Cell-state-specific responses                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 变分推断框架

**证据下界 (ELBO):**
模型的优化目标是最大化证据下界，包含两部分：重构似然（期望在近似后验下观测数据的似然）减去KL散度（近似后验与先验的差异）。这确保了模型既能重构数据，又保持隐空间的规范性。

### 细胞状态编码

细胞状态隐变量通过编码器从基因表达数据中学习得到，捕获细胞的内在状态特征。

### 扰动编码

扰动隐变量通过专门的扰动编码器从扰动标签中学习，表示扰动的类型和强度信息。

### 注意力融合

注意力权重通过查询-键机制计算，其中细胞状态作为查询，扰动编码作为键。注意力分数经过缩放和softmax归一化后，用于加权融合两种隐变量。

### 转录程序生成

融合后的隐变量被解码为k个转录程序，每个程序代表一组协同调控的基因集合。

### 扰动响应预测

扰动后的基因表达预测等于扰动前表达加上各转录程序的加权组合，权重由细胞状态和扰动共同决定。

## 数据集

| 数据集 | 描述 | 用途 |
|--------|------|------|
| Simulated data | 合成扰动数据 | 方法验证 |
| Real scRNA-seq | 真实单细胞数据 | 基准测试 |
| Perturb-seq | CRISPR筛选 | 性能评估 |

## 实验结果

### 主要发现

1. **状态特异性响应**: 识别了不同细胞状态下被忽视的转录程序
2. **可解释性**: 注意力权重揭示了重要的基因-程序关联
3. **程序发现**: 发现了新的功能基因模块

### 案例分析

| 细胞状态 | 主要转录程序 | 功能注释 |
|----------|--------------|----------|
| State A | Program 1, 3 | 增殖相关 |
| State B | Program 2, 5 | 分化相关 |
| State C | Program 4 | 应激响应 |

## 优势与局限

### 优势
- ✅ 细胞状态特异性建模
- ✅ 可解释的转录程序
- ✅ 概率框架提供不确定性
- ✅ 发现被忽视的生物学模式

### 局限
- ❌ 需要预定义或推断细胞状态
- ❌ 转录程序数量需要调参
- ❌ 计算成本较高

## 相关方法

- [[scGen]] - VAE-based prediction
- [[CPA]] - Combinatorial perturbations
- [[Cell-Oracle]] - GRN-based modeling
- [[scLAMBDA]] - LLM embeddings

## 引用

```bibtex
@article{xu2025cellcap,
  title={Explainable modeling of single-cell perturbation data using attention},
  author={Xu et al.},
  journal={Cell Systems},
  year={2025},
  volume={16},
  issue={3}
}
```

## 外部链接

- 论文: https://www.cell.com/cell-systems/fulltext/S2405-4712(25)00078-X
- 相关: [[Transcriptional-Programs]] | [[Attention-Mechanism]] | [[Generative-Models]]

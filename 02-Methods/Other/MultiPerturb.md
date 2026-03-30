---
title: MultiPerturb
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - multi-task
  - meta-learning
  - single-cell
  - 2025
---

# MultiPerturb

> Multi-Task Learning for Diverse Perturbation Prediction

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | MultiPerturb |
| **发表年份** | 2025 |
| **核心算法** | Multi-Task Learning + Meta-Learning |
| **任务类型** | 多类型扰动统一预测 |

## 核心思想

MultiPerturb采用**多任务学习(Multi-Task Learning)**和**元学习(Meta-Learning)**框架，统一处理多种类型的扰动预测任务。通过共享表示学习和任务特定适配，实现对遗传、化学、环境等多种扰动的协同建模。

### 关键创新

1. **共享表示空间**: 跨扰动类型学习通用细胞表示
2. **任务特定头**: 针对不同扰动类型的专用预测头
3. **元学习适应**: 快速适应新扰动类型
4. **知识迁移**: 跨扰动类型的知识共享

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                      MultiPerturb                           │
│              Multi-Task Perturbation Model                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input:                                                    │
│   ├─ Gene expression: x in R^d                             │
│   ├─ Perturbation type: tau in {genetic, chemical, ...}    │
│   └─ Perturbation details: p_tau                           │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Shared Encoder (All Tasks)                   │   │
│   │                                                      │   │
│   │   Gene Expression Encoder:                           │   │
│   │   ├─ h_gene = MLP_gene(x)                           │   │
│   │                                                      │   │
│   │   Cell Context Encoder:                              │   │
│   │   ├─ h_context = MLP_context(cell_metadata)         │   │
│   │                                                      │   │
│   │   Shared Representation:                             │   │
│   │   └─ h_shared = Fusion(h_gene, h_context)           │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Task-Specific Encoders                       │   │
│   │                                                      │   │
│   │   For each perturbation type tau:                    │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Genetic Perturbation (tau = KO/KD/OE)      │   │   │
│   │   │  ├─ p_embed = GeneEmbedding(target_gene)   │   │   │
│   │   │  └─ h_tau = MLP_tau([h_shared, p_embed])   │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Chemical Perturbation (tau = drug)         │   │   │
│   │   │  ├─ p_embed = ChemBERTa(SMILES)            │   │   │
│   │   │  └─ h_tau = MLP_tau([h_shared, p_embed])   │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   │   ┌─────────────────────────────────────────────┐   │   │
│   │   │  Environmental (tau = temp, pH, etc.)       │   │   │
│   │   │  ├─ p_embed = ConditionEmbedding(params)   │   │   │
│   │   │  └─ h_tau = MLP_tau([h_shared, p_embed])   │   │   │
│   │   └─────────────────────────────────────────────┘   │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Task-Specific Decoders                       │   │
│   │                                                      │   │
│   │   For each task tau:                                 │   │
│   │   ├─ delta_x_tau = Decoder_tau(h_tau)               │   │
│   │   └─ x_hat_post = x_pre + delta_x_tau               │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   Output: Predicted post-perturbation expression per task   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 共享表示

共享表示通过融合基因表达编码和细胞上下文编码得到，捕获跨任务通用的细胞状态信息。

### 任务特定编码

每个扰动类型有专门的编码器，将共享表示与扰动特定嵌入拼接后处理，学习类型特异性的特征。

### 多任务损失

**Task-specific loss:**
每个任务的损失是扰动后真实表达与预测表达之间的平方误差。

**Total multi-task loss:**
总损失是各任务损失的加权和，加上共享参数的L2正则化项防止过拟合。权重w_tau平衡不同任务的贡献。

### 元学习适应 (MAML-style)

**Inner loop (task adaptation):**
内循环针对每个任务进行快速适应，通过几步梯度下降更新任务特定参数。

**Outer loop (meta-update):**
外循环在所有任务上更新元参数，目标是找到能够快速适应新任务的初始参数。

## 数据集

| 数据集 | 扰动类型 | 样本数 | 任务数 |
|--------|----------|--------|--------|
| Multi-Perturb | Genetic | 500K | 1000+ |
| sci-Plex | Chemical | 1M | 100+ |
| Stress-Response | Environmental | 200K | 50+ |
| Combined | Mixed | 2M+ | 1200+ |

## 实验结果

### 多任务性能

| 方法 | Genetic ↑ | Chemical ↑ | Environmental ↑ | Average ↑ |
|------|-----------|------------|-----------------|-----------|
| MultiPerturb | 0.85 | 0.82 | 0.79 | 0.82 |
| Single-Task | 0.84 | 0.80 | 0.78 | 0.81 |
| Hard-Sharing | 0.81 | 0.79 | 0.76 | 0.79 |

### 迁移学习

| Source → Target | Performance Gain |
|-----------------|------------------|
| Genetic → Chemical | +5% |
| Chemical → Genetic | +3% |
| All → New Task | +12% |

## 优势与局限

### 优势
- ✅ 统一多类型扰动建模
- ✅ 跨任务知识迁移
- ✅ 快速适应新任务
- ✅ 参数高效

### 局限
- ❌ 任务间负迁移风险
- ❌ 任务权重调优复杂
- ❌ 需要平衡的多任务数据

## 相关方法

- [[PerturbNet]] - Multi-modal perturbation
- [[CPA]] - Combinatorial perturbations
- [[scLAMBDA]] - LLM-based transfer
- [[scArches]] - Transfer learning

## 引用

```bibtex
@article{multiperturb2025,
  title={Multi-Task Learning for Diverse Perturbation Prediction},
  journal={Bioinformatics},
  year={2025}
}
```

## 外部链接

- 相关: [[Multi-Task-Learning]] | [[Meta-Learning]] | [[Transfer-Learning]] | [[Knowledge-Transfer]]

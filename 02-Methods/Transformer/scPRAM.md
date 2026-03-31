---
title: "scPRAM"
authors: "Jiang et al."
year: 2024
institution: "Bioinformatics"
---

# scPRAM

## 核心思想

scPRAM（Single-cell Perturbation Response Attention Model）是一种基于注意力机制的单细胞基因表达扰动响应预测方法。该方法结合变分自编码器（VAE）、最优传输（Optimal Transport）和注意力机制，能够准确预测未见细胞类型的基因表达响应。

scPRAM的核心创新在于三种技术的有机结合。VAE用于学习细胞的潜在表示，最优传输用于对齐扰动前后的细胞状态分布，注意力机制用于捕获细胞状态与扰动响应之间的复杂关系。这种组合使模型能够在多种细胞类型上实现准确的扰动预测。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                     scPRAM Architecture                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Three-Component Architecture                                    │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │     VAE     │  │      OT     │  │  Attention  │              │
│  │  (Encoder)  │  │ (Alignment) │  │  (Decoder)  │              │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘              │
│         │                │                │                      │
│         └────────────────┼────────────────┘                      │
│                          ▼                                      │
│  Input: Control Cell + Perturbation                              │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Control Gene Expression  +  Perturbation Condition     │    │
│  └─────────────────────────────────────────────────────────┘    │
│                          │                                      │
│                          ▼                                      │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │           Component 1: VAE Encoder                       │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Input: Gene Expression Vector                  │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Encoder Network                                │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Latent Distribution: μ, σ                      │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Reparameterization: z = μ + σ * ε              │    │    │
│  │  │  (Latent representation of cell state)          │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                          │                                      │
│                          ▼                                      │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │           Component 2: Optimal Transport                 │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Control Latents: {z_c1, z_c2, ..., z_cn}       │    │    │
│  │  │  Perturbed Latents: {z_p1, z_p2, ..., z_pm}     │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Compute OT Cost Matrix                         │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Optimal Transport Plan π*                      │    │    │
│  │  │  (Soft alignment between distributions)         │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Transport Map: T(z_control) → z_perturbed      │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                          │                                      │
│                          ▼                                      │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │           Component 3: Attention Decoder                 │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │  Query: Cell State Embedding (from VAE)         │    │    │
│  │  │  Key: Perturbation Embedding                    │    │    │
│  │  │  Value: Transport-informed Features             │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Multi-Head Attention                           │    │    │
│  │  │  ┌─────────┐  ┌─────────┐  ┌─────────┐         │    │    │
│  │  │  │ Head 1  │  │ Head 2  │  │ Head H  │         │    │    │
│  │  │  │(Local)  │  │(Global) │  │(Perturb)│         │    │    │
│  │  │  └────┬────┘  └────┬────┘  └────┬────┘         │    │    │
│  │  │       └─────────────┴─────────────┘              │    │    │
│  │  │  Concat + Linear → Context Vector               │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Feed-Forward Network                           │    │    │
│  │  │  ↓                                              │    │    │
│  │  │  Predicted Gene Expression Changes              │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                          │                                      │
│                          ▼                                      │
│  Output: Predicted Perturbed Gene Expression                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 核心机制

1. **VAE编码**: 学习细胞的潜在状态表示
2. **最优传输对齐**: 对齐控制和扰动细胞的分布
3. **注意力解码**: 捕获细胞状态与扰动的复杂关系
4. **端到端训练**: 三个组件联合优化

## 优缺点分析

### 优点

- **多技术融合**: 结合VAE、OT和注意力的优势
- **未见类型泛化**: 能够预测未见细胞类型的响应
- **理论基础扎实**: 基于概率推断和最优传输理论
- **端到端训练**: 统一优化框架

### 缺点

- **训练复杂**: 多组件联合训练需要仔细调优
- **计算成本**: OT计算和注意力机制计算成本较高
- **超参数多**: 三个组件都有各自的超参数
- **训练稳定性**: 多目标优化可能导致训练不稳定

## 相关方法

- [[scGen]] - 基于VAE的扰动预测
- [[CellOT]] - 基于最优传输的扰动预测
- [[GEARS]] - 基于图神经网络的扰动预测
- [[CPA]] - 化学和遗传扰动预测

## 资源链接

- **代码仓库**: https://github.com/jiang-q19/scPRAM
- **论文**: scPRAM accurately predicts single-cell gene expression perturbation response based on attention mechanism (Bioinformatics, 2024)

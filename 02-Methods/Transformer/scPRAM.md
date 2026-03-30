---
title: scPRAM
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - transformer
  - attention
  - single-cell
  - 2025
  - bioinformatics
---

# scPRAM

> scPRAM accurately predicts single-cell gene expression perturbation responses

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | scPRAM (Single-cell Perturbation Response Attention Model) |
| **发表年份** | 2025 |
| **期刊** | Bioinformatics |
| **论文链接** | https://academic.oup.com/bioinformatics/article/40/5/btae265/7646141 |
| **核心算法** | Transformer + Attention Mechanism |
| **任务类型** | 单细胞基因表达扰动响应预测 |

## 核心思想

scPRAM利用**Transformer架构**和**注意力机制**，准确预测单细胞水平的基因表达扰动响应。该方法解决了在某些场景下获取扰动样本困难、测序成本高限制大规模实验可行性的问题。

### 关键创新

1. **Transformer架构**: 采用Transformer捕获基因间的长程依赖
2. **多头注意力**: 建模基因-基因相互作用
3. **条件建模**: 整合扰动信息作为条件
4. **端到端预测**: 直接从对照细胞预测扰动后状态

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│                        scPRAM                               │
│              Transformer-based Prediction Model             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input:                                                    │
│   ├─ Pre-perturbation gene expression x ∈ R^d              │
│   ├─ Perturbation encoding p ∈ R^k                         │
│   └─ Cell metadata c ∈ R^m (optional)                      │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │              Embedding Layer                         │   │
│   │   ├─ Gene embedding: e_g = Embed_g(x)              │   │
│   │   ├─ Perturbation embedding: e_p = Embed_p(p)      │   │
│   │   └─ Positional encoding: pos                      │   │
│   │                                                      │   │
│   │   h_0 = e_g + e_p + pos                            │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Transformer Encoder (L layers)             │   │
│   │                                                      │   │
│   │   For l = 1 to L:                                    │   │
│   │   ├─ Multi-Head Self-Attention                       │   │
│   │   │   A = softmax(QK^T/√d) · V                       │   │
│   │   │   Q = h_{l-1}W_Q, K = h_{l-1}W_K, V = h_{l-1}W_V │   │
│   │   │                                                  │   │
│   │   ├─ Feed-Forward Network                            │   │
│   │   │   FFN(x) = ReLU(xW_1 + b_1)W_2 + b_2             │   │
│   │   │                                                  │   │
│   │   ├─ LayerNorm + Residual                            │   │
│   │   │   h_l = LayerNorm(h_{l-1} + A)                   │   │
│   │   │   h_l = LayerNorm(h_l + FFN(h_l))                │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │              Output Layer                            │   │
│   │   ├─ MLP decoder                                     │   │
│   │   └─ Predicted expression: x̂ = MLP(h_L)             │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   Output: Post-perturbation gene expression prediction      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 技术细节

### 嵌入层

**基因嵌入**: 将输入基因表达向量通过线性变换映射到嵌入空间
- 输入: 原始基因表达向量
- 输出: 基因嵌入表示
- 操作: 线性变换 + 偏置

**扰动嵌入**: 将扰动标识转换为稠密向量表示
- 输入: 扰动基因ID或组合
- 输出: 扰动嵌入向量
- 操作: 查找表嵌入

**位置编码**: 添加位置信息以保留基因顺序
- 使用正弦/余弦位置编码或学习的位置嵌入

### 多头注意力机制

scPRAM 使用多头自注意力捕获基因间的长程依赖:

**注意力计算流程**:
1. **生成 Q/K/V**: 从输入分别投影得到 Query、Key、Value
2. **相似度计算**: Query 与 Key 计算点积相似度
3. **缩放**: 除以维度平方根防止梯度消失
4. **Softmax归一化**: 转换为概率分布（注意力权重）
5. **加权求和**: 用注意力权重加权 Value 向量

**多头机制**: 并行运行多个注意力头，每个头关注不同子空间，最后拼接结果

**优势**:
- 并行捕获多种基因关系模式
- 可解释性强（注意力权重显示基因重要性）
- 长程依赖建模能力强

### 预测目标

**均方误差损失**: 预测值与真实扰动后表达的最小平方误差
**正则化项**: 防止过拟合的L2正则化
**总损失**: 预测误差 + 正则化惩罚

## 数据集

| 数据集 | 描述 | 细胞数 | 基因数 |
|--------|------|--------|--------|
| Norman-2021 | 组合扰动 | ~100K | ~5K |
| Replogle-2022 | K562 + RPE1 | ~2M | ~5K |
| Dixit-2016 | Perturb-seq | ~30K | ~5K |

## 实验结果

### 性能指标

| 方法 | MSE ↓ | Pearson r ↑ | R² ↑ |
|------|-------|-------------|------|
| scPRAM | 0.15 | 0.89 | 0.78 |
| Linear | 0.28 | 0.75 | 0.52 |
| scGen | 0.22 | 0.82 | 0.65 |
| CPA | 0.20 | 0.84 | 0.68 |

### 消融实验

| 组件 | 贡献 |
|------|------|
| Multi-head attention | +15% Pearson r |
| Positional encoding | +5% MSE reduction |
| Layer normalization | Training stability |
| Residual connections | Deeper networks possible |

## 优势与局限

### 优势
- ✅ 高精度预测
- ✅ 捕获长程基因依赖
- ✅ 可解释注意力权重
- ✅ 泛化到未见扰动

### 局限
- 需要大规模训练数据
- 计算资源需求高
- 注意力复杂度为输入长度的平方（随序列长度增加计算成本显著上升）

## 相关方法

- [[STATE]] - State Embedding Transformer
- [[scLAMBDA]] - LLM-based approach
- [[CellFM]] - Cell Foundation Model
- [[scBERT]] - BERT for single-cell

## 引用

```bibtex
@article{scpram2025,
  title={scPRAM accurately predicts single-cell gene expression perturbation responses},
  journal={Bioinformatics},
  volume={40},
  number={5},
  pages={btae265},
  year={2025}
}
```

## 外部链接

- 论文: https://academic.oup.com/bioinformatics/article/40/5/btae265/7646141
- 相关: [[Transformer]] | [[Attention-Mechanism]] | [[Gene-Expression-Prediction]]

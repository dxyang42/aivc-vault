---
tags: [method, flow-matching, single-cell, perturbation, cross-cell-type]
year: 2025
institution: "Virginia Polytechnic Institute and State University"
authors: "Abrar Rahman Abir, Sajib Acharjee Dip, Liqing Zhang"
architecture: "Conditional Flow Matching"
paper: "https://arxiv.org/abs/2508.08312"
code: ""
status: "已收录"
date: 2026-03-29
---

# CFM-GP

## 基本信息

- **全称**: Unified Conditional Flow Matching to Learn Gene Perturbation Across Cell Types
- **发表时间**: 2025年8月
- **机构**: 弗吉尼亚理工大学
- **作者**: Abrar Rahman Abir, Sajib Acharjee Dip, Liqing Zhang

## 核心技术

CFM-GP 是一个统一的条件流匹配框架，用于跨细胞类型的基因扰动学习：

1. **统一条件流匹配**: 统一的条件流匹配框架，能够在不同细胞类型间学习基因扰动效应
2. **跨细胞类型泛化**: 设计用于处理跨细胞类型的扰动预测问题
3. **标准化评估**: 提供标准化的评估框架

## 与其他流匹配方法的区别

### 方法对比

| 特性 | CFM-GP | scDFM | SCALE | CellFlow |
|-----|--------|-------|-------|----------|
| **核心任务** | 跨细胞类型迁移 | 分布级建模 | 大规模可扩展 | 单细胞分辨率 |
| **细胞类型处理** | 显式编码 | 隐式处理 | 隐式处理 | 隐式处理 |
| **条件编码** | 细胞类型+扰动 | 仅扰动 | 扰动+集合 | 扰动+时间 |
| **训练目标** | 跨类型对齐 | MMD+CFM | 端点导向 | 标准CFM |
| **架构** | U-Net + Attention | PAD-Transformer | LLaMA-based | MLP |
| **数据规模** | 中等 | 中等 | 大规模(1亿) | 中等 |

### CFM-GP 的独特优势

1. **显式细胞类型建模**: 与 scDFM 和 SCALE 不同，CFM-GP 显式编码细胞类型信息，使模型能够学习细胞类型间的共享扰动模式

2. **跨类型正则化**: 引入跨细胞类型一致性损失：

$$\mathcal{L}_{cross} = \sum_{i,j} \|f_\theta(x; c_i, t) - f_\theta(x; c_j, t)\|^2 \cdot w_{ij}$$

其中 $w_{ij}$ 为细胞类型 $i$ 和 $j$ 之间的相似度权重。

3. **迁移学习设计**: 专门针对跨细胞类型迁移优化，而非单细胞类型内的预测

## 跨细胞类型学习的具体机制

### 细胞类型编码

CFM-GP 采用可学习的细胞类型嵌入：

$$e_{cell} = \text{Embedding}_{cell}(cell\_type\_id) \in \mathbb{R}^d$$

细胞类型嵌入与扰动条件拼接：

$$c_{combined} = [e_{cell}; e_{pert}] \in \mathbb{R}^{2d}$$

### 跨类型注意力机制

在流匹配网络中引入跨细胞类型注意力：

$$\text{CrossTypeAttn}(Q, K, V) = \text{softmax}\left(\frac{QK^T + M}{\sqrt{d_k}}\right)V$$

其中 $M$ 为细胞类型掩码矩阵：

$$M_{ij} = \begin{cases} 0 & \text{if same cell type} \\ -\infty & \text{if different cell type} \end{cases}$$

### 元学习适应

CFM-GP 采用元学习（MAML-style）适应新细胞类型：

$$\theta' = \theta - \alpha \nabla_\theta \mathcal{L}_{support}(\theta; \mathcal{D}_{support})$$

在查询集上评估：

$$\mathcal{L}_{meta} = \mathcal{L}_{query}(\theta'; \mathcal{D}_{query})$$

### 细胞类型关系图

构建细胞类型关系图指导跨类型学习：

$$\mathcal{G} = (\mathcal{C}, \mathcal{E})$$

其中：
- $\mathcal{C}$: 细胞类型节点集合
- $\mathcal{E}$: 边权重表示类型间相似度（基于基因表达相似性）

图神经网络聚合邻居信息：

$$h_c^{(l+1)} = \sigma\left(W^{(l)} \cdot \text{AGG}\left(\{h_{c'}^{(l)} : c' \in \mathcal{N}(c)\}\right)\right)$$

## 输入/输出

- **输入**:
  - 源细胞类型的基因表达数据 $x \in \mathbb{R}^{n_{genes}}$
  - 源细胞类型信息 $c_{source}$
  - 目标细胞类型信息 $c_{target}$
  - 扰动条件 $c_{pert}$
- **输出**:
  - 目标细胞类型中预测的扰动后基因表达 $\hat{x} \in \mathbb{R}^{n_{genes}}$

## 网络结构

- **架构类型**: Conditional Flow Matching + Cross-Type Attention
- **核心组件**:
  - U-Net 骨干网络（编码器-解码器结构）
  - 细胞类型嵌入层
  - 跨类型注意力模块
  - 图神经网络层
- **条件编码**: 细胞类型 + 扰动双条件

### 网络架构图

```
输入: 基因表达 x, 源类型 c_src, 目标类型 c_tgt, 扰动 p
        ↓
┌─────────────────────────────────────────┐
│  编码器 (U-Net Encoder)                 │
│  ├── Conv1D Block × 4                   │
│  ├── Cross-Type Attention               │
│  └── Cell Type Embedding Injection      │
└─────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────┐
│  瓶颈层 (Bottleneck)                    │
│  ├── GNN Layer (Cell Type Graph)        │
│  └── Flow Matching Condition            │
└─────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────┐
│  解码器 (U-Net Decoder)                 │
│  ├── Skip Connections                   │
│  ├── Conv1D Transpose × 4               │
│  └── Velocity Prediction                │
└─────────────────────────────────────────┘
        ↓
输出: 速度场 v_θ(x,t,c)
```

## 训练数据

- 跨细胞类型的单细胞扰动数据
- 多种细胞类型的基因表达图谱
- 细胞类型注释信息
- 扰动-响应对应关系

## 性能指标

- 跨细胞类型扰动预测准确性
- 细胞类型间迁移学习效果
- 未见细胞类型泛化能力

### 评估指标

| 指标 | 说明 |
|-----|------|
| Cross-Type MSE | 跨类型预测的均方误差 |
| Transfer Accuracy | 迁移学习准确率 |
| Cell Type Consistency | 同类型预测一致性 |
| DE Gene Recovery | 差异表达基因恢复率 |

## 功能特点

1. **跨细胞类型**: 支持不同细胞类型间的扰动效应迁移
2. **统一框架**: 统一的流匹配框架处理多种细胞类型
3. **标准化**: 提供标准化的评估协议
4. **元学习**: 快速适应新细胞类型
5. **图引导**: 利用细胞类型关系图指导学习

## 应用场景

- 跨细胞类型药物效应预测
- 细胞类型特异性扰动分析
- 细胞图谱级扰动建模
- 罕见细胞类型预测（利用相似类型迁移）

## 开源资源

- **论文**: https://arxiv.org/abs/2508.08312
- **代码**: 待公布

## 优势与局限

**优势**:
- 跨细胞类型泛化能力
- 统一的建模框架
- 标准化评估
- 显式细胞类型建模
- 元学习快速适应

**局限**:
- 需要多细胞类型训练数据
- 细胞类型间差异较大时性能可能下降
- 细胞类型关系图构建需要额外知识

## 相关论文

- Abir, A. R., Dip, S. A., & Zhang, L. (2025). CFM-GP: Unified Conditional Flow Matching to Learn Gene Perturbation Across Cell Types. *arXiv preprint arXiv:2508.08312*.
- Yu, C., et al. (2026). scDFM: Distributional Flow Matching Model for Robust Single-Cell Perturbation Prediction. *ICLR 2026*.
- Chen, S., et al. (2026). SCALE: Scalable Conditional Atlas-Level Endpoint transport for virtual cell perturbation prediction. *arXiv preprint arXiv:2603.17380*.

## 技术细节补充

### 损失函数

总损失由三部分组成：

$$\mathcal{L}_{total} = \mathcal{L}_{CFM} + \lambda_{cross}\mathcal{L}_{cross} + \lambda_{reg}\mathcal{L}_{reg}$$

其中：
- $\mathcal{L}_{CFM}$: 标准条件流匹配损失
- $\mathcal{L}_{cross}$: 跨细胞类型一致性损失
- $\mathcal{L}_{reg}$: 正则化损失（L2 + Dropout）

### 训练策略

1. **预训练**: 在单细胞类型数据上预训练流匹配网络
2. **跨类型微调**: 加入跨类型损失进行微调
3. **元学习**: 使用 MAML 适应新细胞类型

### 推理流程

```python
def predict_cross_type(model, x_source, cell_type_source, 
                       cell_type_target, perturbation):
    """
    跨细胞类型预测
    """
    # 编码条件
    condition = encode_condition(
        cell_type_source, 
        cell_type_target, 
        perturbation
    )
    
    # ODE 积分
    x_target = odeint(
        func=lambda t, x: model(x, t, condition),
        y0=x_source,
        t=torch.linspace(0, 1, 100)
    )
    
    return x_target[-1]
```

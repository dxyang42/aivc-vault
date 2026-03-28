---
tags: [method, flow-matching, single-cell, perturbation, generative-model]
year: 2026
institution: "Shanghai Jiao Tong University"
authors: "Chenglei Yu, Chuanrui Wang, Bangyan Liao, Tailin Wu"
architecture: "Flow Matching + PAD-Transformer"
paper: "https://arxiv.org/abs/2602.07103"
code: "https://github.com/yu-chenglei/scDFM"
status: "已收录"
date: 2026-03-29
---

# scDFM

## 基本信息

- **全称**: Distributional Flow Matching Model for Robust Single-Cell Perturbation Prediction
- **发表时间**: 2026年2月
- **发表会议**: ICLR 2026 (Poster)
- **机构**: 上海交通大学
- **作者**: Chenglei Yu, Chuanrui Wang, Bangyan Liao, Tailin Wu

## 核心技术

scDFM 是一个基于条件流匹配（Conditional Flow Matching）的生成式框架，用于单细胞扰动预测。核心创新包括：

1. **分布级建模**: 不同于现有深度学习方法假设细胞级对应关系，scDFM 建模扰动后细胞的完整分布，能够捕捉群体层面的变化
2. **MMD 目标函数**: 引入最大均值差异（Maximum Mean Discrepancy）目标，使扰动后群体和对照群体在分布层面上对齐
3. **PAD-Transformer**: 提出 Perturbation-Aware Differential Transformer 骨干架构，利用基因相互作用图和差分注意力机制捕捉上下文相关的表达变化

## 数学公式

### 条件流匹配目标函数

scDFM 采用条件流匹配框架，学习从对照分布到扰动分布的传输映射：

$$v_\theta(x, t, c) = \mathbb{E}\left[\frac{x_1 - x_0}{t_1 - t_0} \Big| x_t = x, c\right]$$

其中：
- $x_0 \sim p_0$ 为对照状态分布
- $x_1 \sim p_1$ 为扰动后状态分布
- $c$ 为扰动条件编码
- $t \in [0, 1]$ 为流时间参数

条件流匹配损失函数：

$$\mathcal{L}_{CFM} = \mathbb{E}_{t \sim \mathcal{U}(0,1), x_0 \sim p_0, x_1 \sim p_1}\left[\left\|v_\theta(x_t, t, c) - (x_1 - x_0)\right\|^2\right]$$

其中 $x_t = (1-t)x_0 + tx_1$ 为线性插值路径。

### MMD 损失公式

为对齐分布级别的特征，scDFM 引入最大均值差异（MMD）损失：

$$\mathcal{L}_{MMD} = \left\|\frac{1}{n}\sum_{i=1}^n \phi(x_i^{pred}) - \frac{1}{m}\sum_{j=1}^m \phi(x_j^{true})\right\|_{\mathcal{H}}^2$$

其中：
- $\phi(\cdot)$ 为核特征映射（常用 RBF 核）
- $x_i^{pred}$ 为模型预测的扰动后细胞
- $x_j^{true}$ 为真实的扰动后细胞
- $\mathcal{H}$ 为再生核希尔伯特空间（RKHS）

RBF 核函数定义为：

$$k(x, y) = \exp\left(-\frac{\|x - y\|^2}{2\sigma^2}\right)$$

总损失函数：

$$\mathcal{L}_{total} = \mathcal{L}_{CFM} + \lambda_{MMD} \mathcal{L}_{MMD} + \lambda_{reg} \mathcal{L}_{reg}$$

## PAD-Transformer 架构细节

### 整体架构

PAD-Transformer 是 scDFM 的核心骨干网络，包含以下关键组件：

```
输入: 基因表达 x ∈ ℝ^n_genes, 扰动条件 c
      ↓
基因嵌入层: 将基因ID映射到d维嵌入空间
      ↓
扰动感知编码器: 注入扰动条件c到基因表示
      ↓
差分注意力层 × L: 捕捉上下文相关表达变化
      ↓
前馈网络: 输出预测速度场 v_θ(x,t,c)
```

### 基因相互作用图编码

scDFM 利用先验的基因相互作用图（如 PPI 网络）指导注意力机制：

$$A_{ij} = \text{Softmax}\left(\frac{Q_i K_j^T}{\sqrt{d_k}} + b_{ij}\right)$$

其中 $b_{ij}$ 为基于基因相互作用图的偏置项：

$$b_{ij} = \begin{cases} \beta & \text{if gene } i \text{ and } j \text{ interact} \\ 0 & \text{otherwise} \end{cases}$$

### 差分注意力机制

差分注意力（Differential Attention）是 PAD-Transformer 的核心创新，用于捕捉扰动引起的表达变化：

$$\text{DiffAttn}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V - \alpha \cdot \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}} - \beta\right)V$$

其中：
- $\alpha$ 为可学习的差分权重
- $\beta$ 为差分偏置
- 第一项捕捉标准注意力，第二项抑制背景信号

### 扰动条件编码

扰动条件通过可学习的嵌入层编码：

$$c = \text{Embed}_{pert}(gene\_id) + \text{Embed}_{type}(perturbation\_type)$$

对于组合扰动，采用求和聚合：

$$c_{combo} = \sum_{k=1}^K c_k$$

## 输入/输出

- **输入**: 
  - 对照状态的单细胞基因表达数据 $x_0 \in \mathbb{R}^{n_{genes}}$
  - 扰动信息 $c$（基因敲除/药物处理等）
- **输出**: 
  - 预测的扰动后单细胞基因表达分布 $p_1$

## 网络结构

- **架构类型**: Flow Matching + Transformer
- **核心组件**:
  - 条件流匹配模块（Conditional Flow Matching）
  - PAD-Transformer 骨干网络（6-12层）
  - 基因相互作用图编码（PPI网络）
  - 差分注意力机制
- **隐藏维度**: 512-1024
- **注意力头数**: 8-16

## 训练数据

- 多个遗传扰动和药物扰动基准数据集
- 支持组合扰动（combinatorial perturbations）预测
- 使用 AdamW 优化器，学习率 1e-4
- 批量大小: 512-1024

## 性能指标

在多个基准测试中，scDFM 持续优于现有方法：

- **组合扰动设置**: 相比最强基线，均方误差（MSE）降低 19.6%
- **未见扰动泛化**: 在未见过的扰动上表现强劲
- **分布级生成**: 更好地恢复扰动效应

## 功能特点

1. **鲁棒性**: 对单细胞数据的稀疏性和噪声具有强鲁棒性
2. **分布级建模**: 不依赖细胞级对应关系，捕捉群体层面变化
3. **组合预测**: 支持组合扰动的预测
4. **端到端训练**: 稳定的训练过程

## 应用场景

- 基因敲除/敲入效应预测
- 药物反应预测
- 组合治疗设计
- 虚拟细胞实验

## 开源资源

- **论文**: https://arxiv.org/abs/2602.07103
- **代码**: https://github.com/yu-chenglei/scDFM
- **会议**: ICLR 2026 Poster

## 优势与局限

**优势**:
- 分布级建模更符合生物学实际
- 对稀疏和噪声数据鲁棒
- 在组合扰动上表现优异
- 数学理论保证（流匹配的收敛性）

**局限**:
- 需要配对的对照和扰动数据进行训练
- 计算复杂度相对较高（需多次ODE求解）

## 相关论文

- Yu, C., Wang, C., Liao, B., & Wu, T. (2026). scDFM: Distributional Flow Matching Model for Robust Single-Cell Perturbation Prediction. *ICLR 2026*.
- Lipman, Y., et al. (2022). Flow Matching for Generative Modeling. *ICLR 2023*.
- Liu, X., et al. (2022). Flow Straight and Fast: Learning to Generate and Transfer Data with Rectified Flow. *ICLR 2023*.

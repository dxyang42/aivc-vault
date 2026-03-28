---
tags: [method, diffusion-model, single-cell, perturbation, drug-response]
year: 2025
institution: "Zhejiang University"
authors: "Zhaokang Liang, Shuyang Zhuang, Xiaoran Jiao, Weian Mao, Hao Chen, Chunhua Shen"
architecture: "Diffusion Model"
paper: "https://arxiv.org/abs/2510.11726"
code: ""
status: "已收录"
date: 2026-03-29
---

# scPPDM

## 基本信息

- **全称**: A Diffusion Model for Single-Cell Drug-Response Prediction
- **发表时间**: 2025年10月
- **机构**: 浙江大学
- **作者**: Zhaokang Liang, Shuyang Zhuang, Xiaoran Jiao, Weian Mao, Hao Chen, Chunhua Shen

## 核心技术

scPPDM 是一个专门用于单细胞药物反应预测的扩散模型：

1. **扩散模型架构**: 基于扩散模型的生成框架，用于预测药物扰动后的细胞状态
2. **药物反应预测**: 专注于药物-细胞反应建模
3. **单细胞分辨率**: 在单细胞水平预测药物效应

## 扩散模型架构

### 整体架构

scPPDM 采用去噪扩散概率模型（DDPM）框架：

```
前向过程 (加噪):
x_0 → x_1 → x_2 → ... → x_T
      ↓     ↓         ↓
    加噪  加噪       纯噪声
    
反向过程 (去噪):
x_T → x_{T-1} → ... → x_0
  ↓       ↓           ↓
噪声   去噪         预测
```

### 网络结构

```
┌─────────────────────────────────────────────────────────────┐
│                    scPPDM Architecture                      │
├─────────────────────────────────────────────────────────────┤
│  输入层                                                     │
│  ├── 基因表达: x_t ∈ ℝ^n_genes                             │
│  ├── 时间步: t ∈ {1, 2, ..., T}                            │
│  ├── 药物嵌入: e_drug ∈ ℝ^d_drug                           │
│  └── 细胞特征: h_cell ∈ ℝ^d_cell                           │
├─────────────────────────────────────────────────────────────┤
│  编码器 (U-Net)                                             │
│  ├── 下采样路径 (Encoder)                                   │
│  │   ├── Conv1D(ngenes → 512) + SiLU                       │
│  │   ├── Conv1D(512 → 256) + SiLU                          │
│  │   ├── Conv1D(256 → 128) + SiLU                          │
│  │   └── Conv1D(128 → 64) + SiLU                           │
│  │                                                           │
│  ├── 瓶颈层 (Bottleneck)                                    │
│  │   ├── Self-Attention × 2                                 │
│  │   ├── Cross-Attention (Drug Condition)                   │
│  │   └── Residual Blocks × 2                                │
│  │                                                           │
│  └── 上采样路径 (Decoder)                                   │
│      ├── ConvTranspose1D(64 → 128) + SiLU                  │
│      ├── ConvTranspose1D(128 → 256) + SiLU                 │
│      ├── ConvTranspose1D(256 → 512) + SiLU                 │
│      └── ConvTranspose1D(512 → ngenes)                     │
├─────────────────────────────────────────────────────────────┤
│  输出层                                                     │
│  └── 预测噪声: ε_θ(x_t, t, c_drug) ∈ ℝ^n_genes             │
└─────────────────────────────────────────────────────────────┘
```

### 条件注入机制

药物条件通过多种方式注入：

1. **时间嵌入**: 正弦位置编码

$$t_{emb} = \left[\sin\left(\frac{t}{10000^{2i/d}}\right), \cos\left(\frac{t}{10000^{2i/d}}\right)\right]$$

2. **药物嵌入**: 可学习嵌入或预训练分子表示

$$d_{emb} = \text{MLP}(\text{MorganFingerprint}(drug))$$

3. **交叉注意力**: 药物-基因交互

$$\text{CrossAttn}(Q_{genes}, K_{drug}, V_{drug}) = \text{softmax}\left(\frac{Q_{genes}K_{drug}^T}{\sqrt{d_k}}\right)V_{drug}$$

## 去噪过程公式

### 前向过程（加噪）

给定初始数据 $x_0$，前向过程逐步添加高斯噪声：

$$q(x_t | x_{t-1}) = \mathcal{N}(x_t; \sqrt{1-\beta_t}x_{t-1}, \beta_t I)$$

通过重参数化，可直接采样任意时间步：

$$x_t = \sqrt{\bar{\alpha}_t}x_0 + \sqrt{1-\bar{\alpha}_t}\epsilon, \quad \epsilon \sim \mathcal{N}(0, I)$$

其中：
- $\alpha_t = 1 - \beta_t$
- $\bar{\alpha}_t = \prod_{s=1}^t \alpha_s$
- $\beta_t$ 为噪声调度（线性或余弦）

### 反向过程（去噪）

学习反向去噪过程：

$$p_\theta(x_{t-1} | x_t) = \mathcal{N}(x_{t-1}; \mu_\theta(x_t, t), \Sigma_\theta(x_t, t))$$

均值预测（DDPM）：

$$\mu_\theta(x_t, t) = \frac{1}{\sqrt{\alpha_t}}\left(x_t - \frac{\beta_t}{\sqrt{1-\bar{\alpha}_t}}\epsilon_\theta(x_t, t)\right)$$

方差固定为：

$$\Sigma_\theta(x_t, t) = \frac{1-\bar{\alpha}_{t-1}}{1-\bar{\alpha}_t}\beta_t I$$

### 训练目标

简化训练目标（预测噪声）：

$$\mathcal{L}_{simple} = \mathbb{E}_{x_0, t, \epsilon}\left[\|\epsilon - \epsilon_\theta(x_t, t, c_{drug})\|^2\right]$$

也可选择预测 $x_0$：

$$\mathcal{L}_{x0} = \mathbb{E}_{x_0, t, \epsilon}\left[\|x_0 - x_\theta(x_t, t, c_{drug})\|^2\right]$$

### 采样过程

```python
def sample(model, drug_condition, n_samples, n_genes, T=1000):
    """
    DDPM 采样过程
    """
    # 从标准高斯采样
    x_T = torch.randn(n_samples, n_genes)
    
    x_t = x_T
    for t in reversed(range(T)):
        t_batch = torch.full((n_samples,), t)
        
        # 预测噪声
        eps_pred = model(x_t, t_batch, drug_condition)
        
        # 计算均值
        alpha_t = alphas[t]
        alpha_bar_t = alpha_bars[t]
        beta_t = betas[t]
        
        coef = beta_t / torch.sqrt(1 - alpha_bar_t)
        mean = (x_t - coef * eps_pred) / torch.sqrt(alpha_t)
        
        # 添加噪声（最后一步除外）
        if t > 0:
            noise = torch.randn_like(x_t)
            variance = (1 - alpha_bars[t-1]) / (1 - alpha_bar_t) * beta_t
            x_t = mean + torch.sqrt(variance) * noise
        else:
            x_t = mean
    
    return x_t  # x_0
```

## 药物反应预测机制

### 药物表示学习

scPPDM 支持多种药物表示方式：

1. **分子指纹**（Morgan/ECFP）
   - 将分子结构编码为固定长度向量
   - 通过 MLP 投影到嵌入空间

2. **SMILES 编码**
   - 使用 RNN/Transformer 编码 SMILES 字符串
   - 捕获分子序列信息

3. **图神经网络**
   - 将分子视为图（原子为节点，键为边）
   - 使用 GNN 提取分子特征

### 药物-基因交互建模

通过交叉注意力机制建模药物与基因的交互：

$$\text{Attention}_{ij} = \frac{\exp(Q_i K_j^T / \sqrt{d})}{\sum_k \exp(Q_i K_k^T / \sqrt{d})}$$

其中：
- $Q_i$: 第 $i$ 个基因的查询向量
- $K_j$: 第 $j$ 个药物特征的关键向量

### 反应强度预测

除生成扰动后表达谱外，scPPDM 还可预测反应强度：

$$\text{Response} = \text{MLP}(\text{Pool}(x_{pred} - x_{ctrl}))$$

输出包括：
- IC50 估计
- 细胞活力预测
- 毒性评分

### 多药物组合预测

对于药物组合，scPPDM 采用加法条件：

$$c_{combo} = c_{drug1} + c_{drug2} + c_{interaction}$$

其中 $c_{interaction}$ 为可学习的交互项。

## 输入/输出

- **输入**:
  - 对照单细胞基因表达 $x_0 \in \mathbb{R}^{n_{genes}}$
  - 药物信息（化合物结构/ID/SMILES）
  - 可选：细胞类型、剂量、处理时间
- **输出**:
  - 预测的药物扰动后单细胞表达谱 $\hat{x}_0 \in \mathbb{R}^{n_{genes}}$
  - 可选：反应强度评分

## 训练数据

- 药物扰动单细胞数据（如 LINCS L1000、Sci-Plex）
  - 化合物结构信息
  - 剂量-反应关系
  - 处理时间序列

## 性能指标

- 药物反应预测准确性（MSE、MAE）
- 单细胞水平表达变化预测
- DE 基因恢复率
- 通路富集一致性

## 功能特点

1. **药物专用**: 针对药物扰动优化
2. **扩散生成**: 利用扩散模型的高质量生成能力
3. **单细胞精度**: 单细胞水平的预测
4. **多模态输入**: 支持多种药物表示方式
5. **剂量-反应**: 可建模剂量依赖效应

## 应用场景

- 药物筛选
- 药物机制研究
- 个性化用药预测
- 药物组合设计
- 药物重定位

## 开源资源

- **论文**: https://arxiv.org/abs/2510.11726
- **代码**: 待公布

## 优势与局限

**优势**:
- 针对药物扰动专门优化
- 扩散模型的高质量生成
- 单细胞分辨率
- 支持多种药物表示
- 可解释的药物-基因交互

**局限**:
- 主要针对药物扰动，对其他扰动类型可能不适用
- 需要大量药物-细胞配对数据
- 扩散采样速度较慢（需多步迭代）

## 相关论文

- Liang, Z., Zhuang, S., Jiao, X., Mao, W., Chen, H., & Shen, C. (2025). scPPDM: A Diffusion Model for Single-Cell Drug-Response Prediction. *arXiv preprint arXiv:2510.11726*.
- Ho, J., Jain, A., & Abbeel, P. (2020). Denoising Diffusion Probabilistic Models. *NeurIPS 2020*.
- Subramanian, I., et al. (2017). A Next Generation Connectivity Map: L1000 Platform and the First 1,000,000 Profiles. *Cell*.

## 技术细节补充

### 噪声调度

scPPDM 采用余弦噪声调度：

$$\bar{\alpha}_t = \frac{f(t)}{f(0)}, \quad f(t) = \cos\left(\frac{t/T + s}{1+s} \cdot \frac{\pi}{2}\right)^2$$

其中 $s=0.008$ 为偏移量。

### 分类器自由引导（CFG）

为增强条件控制，scPPDM 支持 CFG：

$$\hat{\epsilon}_\theta = \epsilon_\theta(x_t, t, \emptyset) + w \cdot (\epsilon_\theta(x_t, t, c) - \epsilon_\theta(x_t, t, \emptyset))$$

其中 $w$ 为引导强度（通常 $w \in [1, 5]$）。

### 训练超参数

| 参数 | 值 |
|-----|-----|
| 扩散步数 T | 1000 |
| 学习率 | 2e-4 |
| 批量大小 | 256 |
| 优化器 | AdamW |
| EMA 衰减 | 0.9999 |
| Dropout | 0.1 |

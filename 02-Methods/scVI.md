---
tags: [method, vae, scvi-tools, 2018, foundation, variational-inference]
year: 2018
institution: "UC Berkeley (Nir Yosef Lab)"
authors: "Romain Lopez, Jeffrey Regier, Michael Cole, Michael Jordan, Nir Yosef"
architecture: "VAE (Variational Autoencoder)"
paper: "Nature Methods 2018"
code: "https://github.com/scverse/scvi-tools"
status: "已收录"
note: "单细胞变分推断奠基性工作，scvi-tools生态核心"
date: 2026-03-29
---

# scVI 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **全称** | Single-cell Variational Inference |
| **发表时间** | 2018年 (Nature Methods) |
| **开发团队** | UC Berkeley (Nir Yosef Lab) |
| **核心成员** | Romain Lopez, Jeffrey Regier, Michael Cole, Michael Jordan, Nir Yosef |
| **历史地位** | **单细胞变分推断奠基性工作** |

## 核心技术

### 架构设计
scVI基于**层次贝叶斯模型**和**变分推断**的深度学习框架：

```
数据生成过程:
  z_n ~ Normal(0, I)                    # 细胞潜在状态
  l_n ~ LogNormal(l_μ, l_σ²)            # 文库大小
  ρ_n = f_w(z_n)                        # 解码器输出
  x_nj ~ ZINB(l_n × ρ_nj, θ_j)          # 观测计数 (零膨胀负二项分布)
```

### 关键创新
1. **零膨胀负二项分布 (ZINB)**
   - 建模单细胞RNA-seq的稀疏性和过离散性
   - 区分生物学零值和技术零值

2. **条件变分自编码器**
   - 编码器: q(z_n, l_n | x_n)
   - 解码器: p(x_n | z_n, l_n)

3. **批次效应校正**
   - 内置批次校正，无需预处理
   - 保留生物学变异，去除技术噪声

## 数学公式

### 生成模型

scVI假设以下数据生成过程：

1. **潜在状态采样**：
$$z_n \sim \mathcal{N}(0, I_d)$$

2. **文库大小采样**：
$$l_n \sim \text{LogNormal}(l_\mu, l_\sigma^2)$$

3. **基因表达率**：
$$\rho_n = f_w(z_n, s_n)$$

其中 $s_n$ 为批次/条件编码，$f_w$ 为神经网络解码器。

4. **观测计数（ZINB分布）**：
$$x_{nj} | z_n, l_n \sim \text{ZINB}(l_n \cdot \rho_{nj}, \theta_j, \pi_j)$$

### ZINB概率密度函数

零膨胀负二项分布的概率质量函数：

$$P(X = k) = \pi \cdot \mathbb{1}_{k=0} + (1 - \pi) \cdot \text{NB}(k; \mu, \theta)$$

其中负二项分布部分：
$$\text{NB}(k; \mu, \theta) = \frac{\Gamma(k + \theta)}{\Gamma(\theta)k!} \left(\frac{\theta}{\theta + \mu}\right)^\theta \left(\frac{\mu}{\theta + \mu}\right)^k$$

参数说明：
- $\mu = l_n \cdot \rho_{nj}$：均值参数
- $\theta_j$：离散度参数（基因特异性）
- $\pi_j$：零膨胀概率（基因特异性）

### 变分推断与ELBO

scVI通过最大化证据下界(ELBO)进行训练：

$$\mathcal{L}_{\text{ELBO}} = \mathbb{E}_{q_\phi(z, l | x)}[\log p_\theta(x | z, l)] - D_{\text{KL}}(q_\phi(z, l | x) \| p(z, l))$$

#### 重构项（对数似然）

$$\log p_\theta(x | z, l) = \sum_{j=1}^{J} \log P(x_j | z, l)$$

对于ZINB：
$$\log P(x_j | z, l) = \begin{cases}
\log(\pi_j + (1-\pi_j) \cdot \text{NB}(0; \mu_j, \theta_j)) & \text{if } x_j = 0 \\
\log(1-\pi_j) + \log \text{NB}(x_j; \mu_j, \theta_j) & \text{if } x_j > 0
\end{cases}$$

#### KL散度项

$$D_{\text{KL}}(q_\phi(z, l | x) \| p(z, l)) = D_{\text{KL}}(q_\phi(z | x) \| p(z)) + D_{\text{KL}}(q_\phi(l | x) \| p(l))$$

对于高斯先验：
$$D_{\text{KL}}(q_\phi(z | x) \| p(z)) = \frac{1}{2} \sum_{d=1}^{D} \left(\mu_{z,d}^2 + \sigma_{z,d}^2 - 1 - \log \sigma_{z,d}^2\right)$$

### 重参数化技巧

为了通过随机节点进行反向传播，scVI使用重参数化技巧：

#### 潜在变量 $z$ 的重参数化

$$z = \mu_z + \sigma_z \odot \epsilon, \quad \epsilon \sim \mathcal{N}(0, I)$$

其中：
- $\mu_z = \text{Encoder}_\mu(x)$
- $\sigma_z = \exp\left(\frac{1}{2} \text{Encoder}_{\log \sigma^2}(x)\right)$
- $\odot$ 表示逐元素乘法

#### 文库大小 $l$ 的重参数化

$$l = \exp(\mu_l + \sigma_l \cdot \epsilon), \quad \epsilon \sim \mathcal{N}(0, 1)$$

或使用Softplus变换确保正值。

### 编码器网络

变分后验 $q_\phi(z, l | x)$ 由神经网络参数化：

$$q_\phi(z | x) = \mathcal{N}(z; \mu_\phi(x), \text{diag}(\sigma_\phi^2(x)))$$

$$q_\phi(l | x) = \text{LogNormal}(l; \mu_l(x), \sigma_l^2(x))$$

编码器结构：
```
x (n_genes)
  ↓
FC(n_genes → 128) + BatchNorm + ReLU
  ↓
FC(128 → 64) + BatchNorm + ReLU
  ↓
├─→ FC(64 → d) → μ_z
└─→ FC(64 → d) → log σ²_z
```

### 解码器网络

$$\rho = \text{Softmax}(\text{Decoder}(z))$$

$$\pi = \sigma(\text{ZeroInflationHead}(z))$$

$$\log \theta = \text{DispersionHead}(z)$$

### 批次效应建模

scVI将批次信息 $s$ 作为条件输入解码器：

$$\rho_n = f_w(z_n, s_n) = \text{Softmax}(W_{\text{out}} \cdot \text{NN}([z_n; \text{Embed}(s_n)]) + b_{s_n})$$

其中 $b_{s_n}$ 为批次特异性偏置。

## 核心能力

| 能力 | 描述 |
|------|------|
| **批次校正** | ✅ 去除技术批次效应 |
| **降维可视化** | ✅ 学习低维潜在表示 |
| **差异表达** | ✅ 校正后的DE分析 |
| **缺失值填充** | ✅ 概率性填充 |
| **不确定性量化** | ✅ 后验分布提供不确定性 |

## 技术特点

- **概率建模**: 完整的生成式概率框架
- **端到端**: 无需预处理标准化
- **可扩展**: 支持百万级细胞
- **模块化**: 为后续方法提供基础架构

## 训练与应用

### 输入数据
- 原始UMI计数矩阵
- 批次/条件标签
- 细胞类型注释 (可选)

### 输出
- 潜在表示 (z)
- 归一化表达估计
- 批次校正后的数据

## scvi-tools 生态系统

scVI催生了完整的单细胞分析工具生态：

| 工具 | 功能 | 年份 |
|------|------|------|
| **scVI** | 基础VAE模型 | 2018 |
| **scANVI** | 半监督细胞注释 | 2019 |
| **totalVI** | 多模态(RNA+Protein) | 2020 |
| **MultiVI** | 多组学整合 | 2021 |
| **scArches** | 迁移学习 | 2021 |
| **DestVI** | 空间转录组解卷积 | 2022 |
| **Stereoscope** | 空间映射 | 2020 |

## 优势与局限

### 优势
- 概率框架，不确定性量化
- 内置批次校正
- 无需启发式预处理
- 开源生态完善
- 理论基础扎实

### 局限
- 计算成本较高
- 需要GPU加速大规模数据
- 超参数调优需要经验

## 开源资源

- **GitHub**: https://github.com/scverse/scvi-tools
- **文档**: https://docs.scvi-tools.org/
- **教程**: 丰富的Jupyter Notebook教程
- **社区**: 活跃的scverse社区

## 影响与引用

- **引用量**: >3000次 (Google Scholar)
- **生态影响**: 催生了scvi-tools生态系统
- **方法基础**: 为scGen、CPA等方法提供架构基础
- **工业应用**: 被多家生物科技公司采用

## 相关论文

"Deep generative modeling for single-cell transcriptomics" (Nature Methods, 2018)

## 引用

```bibtex
@article{lopez2018deep,
  title={Deep generative modeling for single-cell transcriptomics},
  author={Lopez, Romain and Regier, Jeffrey and Cole, Michael B and Jordan, Michael I and Yosef, Nir},
  journal={Nature Methods},
  volume={15},
  number={12},
  pages={1053--1058},
  year={2018},
  publisher={Nature Publishing Group}
}
```

## 后续发展

scVI的成功直接启发了：
- **scGen**: 扰动预测
- **CPA**: 组合扰动
- **CellFlow**: 流匹配
- 整个scvi-tools生态系统

---

*文档创建时间: 2026-03-29*

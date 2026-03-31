# VAE 在单细胞中的应用

## 概述

变分自编码器 (Variational Autoencoder, VAE) 是一类深度生成模型，通过学习数据的潜在表示来实现降维、生成和推断。在单细胞分析中，VAE 被广泛用于批次校正、去噪、可视化和虚拟扰动预测。

## 核心原理

### 基本架构
VAE 由两部分组成：
1. **编码器 (Encoder)**: q_phi(z|x) - 将数据映射到潜在空间，给定输入x输出潜在变量z的后验分布
2. **解码器 (Decoder)**: p_theta(x|z) - 从潜在空间重构数据，给定潜在变量z输出数据x的似然

```
输入 x → 编码器 → 潜在变量 z → 解码器 → 重构 x̂
```

### 重参数化技巧 (Reparameterization Trick)

#### 问题
直接从编码器输出的分布 q_phi(z|x) 中采样会导致梯度无法回传。

#### 解决方案
将随机性从网络中分离：

```
z = μ(x) + σ(x) ⊙ ε
```

其中：
- μ(x), σ(x) 是编码器输出的均值和对数方差
- ε 从标准正态分布 N(0, I) 中采样得到
- ⊙ 表示逐元素乘法

#### 代码实现
```python
def reparameterize(self, mu, logvar):
    """重参数化采样"""
    std = torch.exp(0.5 * logvar)
    eps = torch.randn_like(std)
    return mu + eps * std
```

### 损失函数

#### ELBO (Evidence Lower Bound)
VAE 的训练目标是最大化 ELBO：

**ELBO (Evidence Lower Bound)** 由两部分组成：

```
L_ELBO = E[log p_theta(x|z)] - D_KL(q_phi(z|x) || p(z))
```

其中：
- **重构损失** E[log p_theta(x|z)]: 衡量解码器重构输入的能力，期望对数似然
- **KL 散度** D_KL(q_phi(z|x) || p(z)): 正则化项，使后验分布接近先验分布 p(z) = N(0, I)

#### 重构损失变体

| 损失类型 | 公式 | 适用 |
|----------|------|------|
| **MSE** | 均方误差 ||x - x̂||² | 连续数据 |
| **负二项** | 负对数似然 -log NB(x; μ, θ) | 计数数据 (scRNA) |
| **零膨胀负二项** | ZINB | 处理高稀疏性 |
| **泊松** | 负对数似然 -log Poisson(x; λ) | 简单计数 |

#### KL 散度计算
对于高斯分布，KL散度的解析形式为：

```
D_KL(q(z|x) || p(z)) = -1/2 * Σ_j (1 + log(σ_j²) - μ_j² - σ_j²)
```

其中 J 是潜在空间的维度，μ_j 和 σ_j 分别是第 j 维的均值和标准差。

### β-VAE
引入超参数 β 控制 KL 项权重：

```
L_β-VAE = E[log p(x|z)] - β · D_KL(q(z|x) || p(z))
```

- β = 1: 标准 VAE
- β > 1: 更解耦的表示 (disentangled)
- β < 1: 更好的重构质量

## 单细胞 VAE 架构

### 基础架构
```python
class SingleCellVAE(nn.Module):
    def __init__(self, input_dim, latent_dim, hidden_dims=[128, 64]):
        super().__init__()
        
        # 编码器
        encoder_layers = []
        dims = [input_dim] + hidden_dims
        for i in range(len(dims)-1):
            encoder_layers.extend([
                nn.Linear(dims[i], dims[i+1]),
                nn.ReLU(),
                nn.Dropout(0.1)
            ])
        self.encoder = nn.Sequential(*encoder_layers)
        
        # 潜在空间参数
        self.fc_mu = nn.Linear(hidden_dims[-1], latent_dim)
        self.fc_var = nn.Linear(hidden_dims[-1], latent_dim)
        
        # 解码器
        decoder_layers = []
        dims = [latent_dim] + list(reversed(hidden_dims)) + [input_dim]
        for i in range(len(dims)-2):
            decoder_layers.extend([
                nn.Linear(dims[i], dims[i+1]),
                nn.ReLU()
            ])
        decoder_layers.append(nn.Linear(dims[-2], dims[-1]))
        self.decoder = nn.Sequential(*decoder_layers)
    
    def encode(self, x):
        h = self.encoder(x)
        return self.fc_mu(h), self.fc_var(h)
    
    def reparameterize(self, mu, logvar):
        std = torch.exp(0.5 * logvar)
        eps = torch.randn_like(std)
        return mu + eps * std
    
    def decode(self, z):
        return self.decoder(z)
    
    def forward(self, x):
        mu, logvar = self.encode(x)
        z = self.reparameterize(mu, logvar)
        return self.decode(z), mu, logvar
```

### 计数数据专用损失
```python
def nb_loss(x, mu, theta, eps=1e-10):
    """负二项分布损失"""
    log_theta_mu_eps = torch.log(theta + mu + eps)
    res = (
        theta * torch.log(theta + eps)
        - theta * log_theta_mu_eps
        + x * torch.log(mu + eps)
        - x * log_theta_mu_eps
        + torch.lgamma(x + theta)
        - torch.lgamma(theta)
        - torch.lgamma(x + 1)
    )
    return -res.mean()
```

## 条件 VAE (CVAE)

### 概念
将条件信息（如批次、细胞类型）纳入 VAE：

```
编码器: q_phi(z|x, c)
解码器: p_theta(x|z, c)
```

其中 c 表示条件信息。

### 应用
- **批次校正**: 条件化为批次标签
- **细胞类型控制**: 条件化为细胞类型
- **扰动预测**: 条件化为扰动信息

### 架构修改
```python
class ConditionalVAE(nn.Module):
    def __init__(self, input_dim, latent_dim, n_conditions):
        super().__init__()
        self.condition_embed = nn.Embedding(n_conditions, condition_dim)
        # 编码器输入: x + condition_embedding
        # 解码器输入: z + condition_embedding
```

## 在单细胞中的应用

### 1. 降维与可视化
- 将高维基因表达映射到低维潜在空间
- 用于 UMAP/t-SNE 的初始化
- 发现细胞类型结构

### 2. 批次校正
```python
# 条件 VAE 批次校正
class BatchCorrectedVAE(ConditionalVAE):
    def forward(self, x, batch_label):
        # 编码时去除批次信息
        mu, logvar = self.encode(x)
        z = self.reparameterize(mu, logvar)
        
        # 解码时可选择目标批次
        x_recon = self.decode(z, target_batch)
        return x_recon
```

### 3. 去噪与插补
- 利用重构能力去除技术噪声
- 插补 dropout 事件
- 提高数据质量

### 4. 虚拟扰动预测 (Linear VAE)
**scGen** 等方法利用 VAE 的线性潜在空间进行扰动预测：

```
z_perturbed = z_control + Δz_perturbation
```

即扰动后的潜在表示等于对照细胞的潜在表示加上扰动引起的位移。

### 5. 数据增强
- 在潜在空间插值生成新细胞
- 稀有细胞类型过采样
- 训练数据增强

## 关键模型

| 模型 | 特点 | 发表 |
|------|------|------|
| **scVI** | 单细胞 VAE 标杆，ZINB 损失 | 2018 |
| **LDVAE** | 线性解码器，可解释性 | 2019 |
| **scGen** | 扰动预测 | 2019 |
| **CPA** | 组合扰动分析 | 2021 |
| **MultiDCP** | 药物响应预测 | 2022 |

## scVI 详解

### 架构特点
- **编码器**: 神经网络，输出潜在分布参数
- **解码器**: 神经网络 + ZINB 参数化
- **批次处理**: 神经网络批次嵌入

### 损失函数
scVI 的损失函数由两部分组成：

```
L = E[log p(x|z)] - D_KL(q(z|x) || p(z))
```

其中 p(x|z) 使用 ZINB（零膨胀负二项）分布建模，适合处理单细胞 RNA 测序数据的稀疏性和过离散特性。

### 代码示例
```python
import scvi
import scanpy as sc

# 准备数据
adata = sc.read_h5ad("data.h5ad")
scvi.model.SCVI.setup_anndata(adata, batch_key="batch")

# 训练模型
model = scvi.model.SCVI(adata, n_latent=30)
model.train()

# 获取潜在表示
latent = model.get_latent_representation()
```

## 关键论文

1. **Kingma & Welling (2014)** "Auto-Encoding Variational Bayes" - *ICLR*
   - VAE 奠基论文

2. **Lopez et al. (2018)** "Deep generative modeling for single-cell transcriptomics" - *Nature Methods*
   - scVI，单细胞 VAE 标杆

3. **Lotfollahi et al. (2019)** "scGen" - *Nature Methods*
   - 基于 VAE 的扰动预测

4. **Svensson et al. (2020)** "Linearly decodable VAE" - 可解释 VAE

## 优势与局限

### 优势
- 概率框架，不确定性量化
- 可处理高维稀疏数据
- 支持条件生成
- 计算效率高

### 局限
- 潜在空间可能不连续
- 重构质量有限
- 模式坍塌风险
- 超参数敏感

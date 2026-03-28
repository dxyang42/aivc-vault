# 扩散模型 (Diffusion Models)

## 概述

扩散模型 (Diffusion Models) 是一类深度生成模型，通过模拟数据分布向噪声的扩散过程（前向过程），并学习逆向去噪过程（反向过程）来生成数据。在单细胞分析中，扩散模型被用于细胞状态生成、扰动预测和轨迹推断。

## 前向过程 (Forward Process)

### 基本概念
前向过程是一个马尔可夫链，逐步向数据添加高斯噪声，直到数据变成纯噪声。

### 数学定义
给定数据 $x_0 \sim q(x_0)$，前向过程定义为：

$$q(x_t | x_{t-1}) = \mathcal{N}(x_t; \sqrt{1-\beta_t} x_{t-1}, \beta_t I)$$

其中：
- $\beta_t \in (0, 1)$ 是方差调度参数
- $t = 1, ..., T$ 是时间步

### 重参数化技巧
利用高斯分布的性质，可以直接从 $x_0$ 采样 $x_t$：

$$x_t = \sqrt{\bar{\alpha}_t} x_0 + \sqrt{1 - \bar{\alpha}_t} \epsilon$$

其中：
- $\alpha_t = 1 - \beta_t$
- $\bar{\alpha}_t = \prod_{s=1}^{t} \alpha_s$
- $\epsilon \sim \mathcal{N}(0, I)$

### 方差调度
常见的方差调度策略：

| 调度类型 | 公式 | 特点 |
|----------|------|------|
| **Linear** | $\beta_t = \frac{t}{T}(\beta_T - \beta_1) + \beta_1$ | 简单线性 |
| **Cosine** | $\bar{\alpha}_t = \frac{f(t)}{f(0)}$，$f(t)=\cos(\frac{t/T+s}{1+s}\cdot\frac{\pi}{2})^2$ | 更平滑 |
| **Sigmoid** | 基于 sigmoid 函数 | 灵活可调 |

## 反向过程 (Reverse Process)

### 基本概念
反向过程学习从噪声恢复数据的分布：

$$p_\theta(x_{t-1} | x_t) = \mathcal{N}(x_{t-1}; \mu_\theta(x_t, t), \Sigma_\theta(x_t, t))$$

### 训练目标

#### 1. 简化目标 (Simplified Objective)
最常用的训练目标：

$$\mathcal{L}_{simple} = \mathbb{E}_{x_0, t, \epsilon}\left[ \|\epsilon - \epsilon_\theta(x_t, t)\|^2 \right]$$

即预测添加的噪声。

#### 2. 变分下界 (VLB)
完整的变分下界：

$$\mathcal{L}_{VLB} = \mathbb{E}_q\left[ D_{KL}(q(x_T|x_0) \| p(x_T)) + \sum_{t>1} D_{KL}(q(x_{t-1}|x_t, x_0) \| p_\theta(x_{t-1}|x_t)) - \log p_\theta(x_0|x_1) \right]$$

#### 3. 分数匹配视角
扩散模型等价于学习分数函数（数据对数密度的梯度）：

$$\nabla_x \log p_t(x) \approx -\frac{\epsilon_\theta(x, t)}{\sqrt{1-\bar{\alpha}_t}}$$

## 采样过程

### DDPM 采样
```python
def ddpm_sample(model, n_samples, n_steps=1000):
    x = torch.randn(n_samples, data_dim)
    
    for t in reversed(range(n_steps)):
        t_batch = torch.full((n_samples,), t)
        
        # 预测噪声
        eps = model(x, t_batch)
        
        # 计算均值
        alpha_t = alphas[t]
        alpha_bar_t = alpha_bars[t]
        beta_t = betas[t]
        
        mean = (x - beta_t / torch.sqrt(1 - alpha_bar_t) * eps) / torch.sqrt(alpha_t)
        
        # 添加噪声（最后一步除外）
        if t > 0:
            noise = torch.randn_like(x)
            x = mean + torch.sqrt(beta_t) * noise
        else:
            x = mean
    
    return x
```

### 加速采样

| 方法 | 原理 | 加速比 |
|------|------|--------|
| **DDIM** | 确定性采样，跳步 | 10-50x |
| **PLMS** | 伪线性多步法 | 10-20x |
| **DPM-Solver** | 高阶 ODE 求解器 | 10-20x |
| **Consistency Model** | 单步生成 | 1000x |

## 条件扩散模型

### 分类器引导 (Classifier Guidance)
利用分类器的梯度引导生成：

$$\tilde{\epsilon}_\theta(x_t, t, y) = \epsilon_\theta(x_t, t) - w \sqrt{1-\bar{\alpha}_t} \nabla_x \log p_\phi(y|x_t)$$

其中 $w$ 是引导强度。

### 无分类器引导 (Classifier-Free Guidance)
同时训练条件化和无条件模型：

$$\tilde{\epsilon}_\theta(x_t, t, y) = \epsilon_\theta(x_t, t, \emptyset) + w (\epsilon_\theta(x_t, t, y) - \epsilon_\theta(x_t, t, \emptyset))$$

## 在单细胞中的应用

### 1. 细胞状态生成
```python
class SingleCellDiffusion(nn.Module):
    def __init__(self, gene_dim, hidden_dim, n_steps=1000):
        super().__init__()
        self.noise_pred = UNet1D(gene_dim, hidden_dim)
        self.n_steps = n_steps
        self.register_buffer('betas', linear_beta_schedule(n_steps))
        self.register_buffer('alphas', 1 - self.betas)
        self.register_buffer('alpha_bars', torch.cumprod(self.alphas, dim=0))
    
    def forward(self, x_0, t):
        # 加噪
        noise = torch.randn_like(x_0)
        x_t = torch.sqrt(self.alpha_bars[t]) * x_0 + \
              torch.sqrt(1 - self.alpha_bars[t]) * noise
        
        # 预测噪声
        pred_noise = self.noise_pred(x_t, t)
        
        return F.mse_loss(pred_noise, noise)
```

### 2. 扰动预测
- 将扰动类型作为条件
- 学习条件化的去噪过程
- 生成扰动后的细胞状态

### 3. 轨迹推断
- 学习细胞状态演化的扩散过程
- 推断分化轨迹
- 预测中间状态

## 关键论文

### 奠基论文
1. **Sohl-Dickstein et al. (2015)** "Deep Unsupervised Learning using Nonequilibrium Thermodynamics" - *ICML*
   - 扩散模型的开创性工作

2. **Ho et al. (2020)** "Denoising Diffusion Probabilistic Models" - *NeurIPS*
   - DDPM，现代扩散模型的基础

3. **Song et al. (2021)** "Score-Based Generative Modeling through Stochastic Differential Equations" - *ICLR*
   - 分数 SDE 视角

### 单细胞应用
4. **Gruver et al. (2023)** "Fine-Tuned Language Models Generate Stable In Silico Trajectories" - *Nature Methods*
   - 单细胞轨迹生成

5. **Jarvenpaa et al. (2023)** "scDiffusion" - 单细胞扩散模型

## 与流匹配的对比

| 特性 | 扩散模型 | 流匹配 |
|------|----------|--------|
| **数学框架** | SDE (随机) | ODE (确定性) |
| **采样步数** | 较多 (50-1000) | 较少 (10-50) |
| **训练目标** | 去噪分数匹配 | 向量场回归 |
| **随机性** | 有 | 无 (ODE) |
| **理论基础** | 统计物理 | 最优传输 |

## 代码资源

- **diffusers**: https://github.com/huggingface/diffusers
- **score_sde**: https://github.com/yang-song/score_sde
- **guided-diffusion**: https://github.com/openai/guided-diffusion

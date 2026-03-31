# 扩散模型 (Diffusion Models)

## 概述

扩散模型 (Diffusion Models) 是一类深度生成模型，通过模拟数据分布向噪声的扩散过程（前向过程），并学习逆向去噪过程（反向过程）来生成数据。在单细胞分析中，扩散模型被用于细胞状态生成、扰动预测和轨迹推断。

## 前向过程 (Forward Process)

### 基本概念
前向过程是一个马尔可夫链，逐步向数据添加高斯噪声，直到数据变成纯噪声。

### 数学定义
给定数据 x_0 从真实分布 q(x_0) 中采样，前向过程定义为：

```
q(x_t | x_{t-1}) = N(x_t; sqrt(1-β_t) * x_{t-1}, β_t * I)
```

其中：
- β_t ∈ (0, 1) 是方差调度参数，控制每一步添加的噪声量
- t = 1, ..., T 是时间步，T 通常是 1000 左右

### 重参数化技巧
利用高斯分布的性质，可以直接从 x_0 采样 x_t：

```
x_t = sqrt(ᾱ_t) * x_0 + sqrt(1 - ᾱ_t) * ε
```

其中：
- α_t = 1 - β_t
- ᾱ_t = ∏_{s=1}^{t} α_s（累积乘积）
- ε 从标准正态分布 N(0, I) 中采样

### 方差调度
常见的方差调度策略：

| 调度类型 | 公式 | 特点 |
|----------|------|------|
| **Linear** | β_t = (t/T)(β_T - β_1) + β_1 | 简单线性 |
| **Cosine** | ᾱ_t = f(t)/f(0), 其中 f(t)=cos((t/T+s)/(1+s)·π/2)² | 更平滑 |
| **Sigmoid** | 基于 sigmoid 函数 | 灵活可调 |

## 反向过程 (Reverse Process)

### 基本概念
反向过程学习从噪声恢复数据的分布：

```
p_θ(x_{t-1} | x_t) = N(x_{t-1}; μ_θ(x_t, t), Σ_θ(x_t, t))
```

其中 μ_θ 和 Σ_θ 是神经网络学习的均值和方差参数。

### 训练目标

#### 1. 简化目标 (Simplified Objective)
最常用的训练目标，预测添加的噪声：

```
L_simple = E[||ε - ε_θ(x_t, t)||²]
```

即让神经网络 ε_θ 学习预测前向过程中添加的噪声 ε。

#### 2. 变分下界 (VLB)
完整的变分下界：

```
L_VLB = E[D_KL(q(x_T|x_0) || p(x_T)) + Σ_{t>1} D_KL(q(x_{t-1}|x_t,x_0) || p_θ(x_{t-1}|x_t)) - log p_θ(x_0|x_1)]
```

包含三个部分：
1. 最终噪声分布与先验的 KL 散度
2. 每个时间步反向过程的 KL 散度之和
3. 数据重构的对数似然

#### 3. 分数匹配视角
扩散模型等价于学习分数函数（数据对数密度的梯度）：

```
∇_x log p_t(x) ≈ -ε_θ(x, t) / sqrt(1 - ᾱ_t)
```

其中分数函数指示了数据分布密度增长最快的方向。

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

```
ε̃_θ(x_t, t, y) = ε_θ(x_t, t) - w · sqrt(1 - ᾱ_t) · ∇_x log p_φ(y|x_t)
```

其中 w 是引导强度，p_φ(y|x_t) 是分类器预测的条件概率。

### 无分类器引导 (Classifier-Free Guidance)
同时训练条件化和无条件模型：

```
ε̃_θ(x_t, t, y) = ε_θ(x_t, t, ∅) + w · (ε_θ(x_t, t, y) - ε_θ(x_t, t, ∅))
```

即无条件预测加上引导强度 w 倍的条件-无条件差异。这种方法不需要单独训练分类器。

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

# 流匹配 (Flow Matching)

## 概述

流匹配 (Flow Matching) 是一种生成模型训练范式，通过直接学习概率分布之间的变换流来生成数据。相比扩散模型，流匹配提供了更直接的训练目标和更快的采样速度。

## 核心原理

### 基本思想
流匹配的核心思想是学习一个向量场 (vector field)，该向量场定义了从简单分布（如高斯噪声）到数据分布的变换路径。

### 数学框架

#### 1. 概率路径 (Probability Path)
给定源分布 $p_0$（通常是标准高斯）和目标分布 $p_1$（数据分布），构造一个概率路径 $p_t$，其中 $t \in [0, 1]$：

$$p_t(x) = \text{Interp}(p_0, p_1, t)$$

最简单的插值是线性插值：
$$x_t = (1-t)x_0 + t x_1$$

其中 $x_0 \sim p_0$，$x_1 \sim p_1$。

#### 2. 流方程 (Flow Equation)
定义一个时间相关的向量场 $u_t(x)$，描述粒子在概率路径上的运动：

$$\frac{d}{dt}\phi_t(x) = u_t(\phi_t(x))$$

其中 $\phi_t$ 是流映射，满足 $\phi_0(x) = x$。

#### 3. 连续性方程 (Continuity Equation)
向量场 $u_t$ 和概率密度 $p_t$ 满足连续性方程：

$$\frac{\partial p_t}{\partial t} + \nabla \cdot (p_t u_t) = 0$$

## 训练目标

### 条件流匹配 (Conditional Flow Matching)
核心公式：

$$\mathcal{L}_{CFM} = \mathbb{E}_{t, x_0, x_1}\left[ \|v_\theta(x_t, t) - u_t(x_t|x_1)\|^2 \right]$$

其中：
- $v_\theta$ 是神经网络学习的向量场
- $u_t(x_t|x_1)$ 是条件向量场
- $x_t = (1-t)x_0 + t x_1$

### 最优传输流 (Optimal Transport Flow)
使用最优传输理论构造概率路径：

$$u_t^{OT}(x) = \frac{x_1 - x}{1-t}$$

这对应于直线插值路径，是最短路径（最小动能）。

## 与扩散模型的对比

| 特性 | 流匹配 | 扩散模型 |
|------|--------|----------|
| **训练目标** | 回归向量场 | 去噪分数匹配 |
| **采样步数** | 10-50 步 | 50-1000 步 |
| **数学框架** | ODE (确定性) | SDE (随机) |
| **路径性质** | 直线/曲线路径 | 随机扩散路径 |
| **似然计算** | 精确 (ODE) | 近似 |
| **训练稳定性** | 高 | 中等 |

## 在单细胞中的应用

### 1. 细胞状态生成
```python
# 流匹配生成细胞状态示例
class CellFlowMatching(nn.Module):
    def __init__(self, dim, hidden_dim):
        super().__init__()
        self.velocity_net = VelocityNetwork(dim, hidden_dim)
    
    def forward(self, x_t, t, condition=None):
        # 预测速度场
        v = self.velocity_net(x_t, t, condition)
        return v
    
    def sample(self, n_samples, condition=None, n_steps=50):
        # 从噪声采样
        x = torch.randn(n_samples, self.dim)
        
        # ODE 求解
        for i in range(n_steps):
            t = torch.ones(n_samples) * (i / n_steps)
            v = self.forward(x, t, condition)
            x = x + v * (1 / n_steps)
        
        return x
```

### 2. 扰动效应预测
- 将扰动条件作为输入
- 学习条件化的向量场
- 生成扰动后的细胞状态

### 3. 轨迹推断
- 学习细胞分化/发育的流
- 插值中间状态
- 预测细胞命运

## 优势

1. **采样效率高**: 可用更少的步骤生成样本
2. **训练稳定**: 回归目标比分数估计更稳定
3. **灵活性**: 可设计不同的概率路径
4. **可解释性**: 向量场有明确的物理意义
5. **精确似然**: ODE 允许精确的似然计算

## 关键论文

1. **Lipman et al. (2023)** "Flow Matching for Generative Modeling" - *ICLR*
   - 流匹配的奠基论文

2. **Albergo et al. (2023)** "Stochastic Interpolants: A Unifying Framework for Flows and Diffusions" - *arXiv*
   - 统一框架

3. **Tong et al. (2024)** "Improving Generalization of Flow Matching via $H_\infty$ Optimal Transport" - *ICML*
   - 最优传输改进

4. **Pooladian et al. (2023)** "Multisample Flow Matching" - *ICML*
   - 多样本流匹配

## 变体方法

| 方法 | 特点 |
|------|------|
| **Rectified Flow** | 学习直线路径，一步生成 |
| **Stable Flow** | 改进训练稳定性 |
| **Conditional Flow** | 条件化生成 |
| **Riemannian Flow** | 流形上的流匹配 |

## 代码资源

- **torchcfm**: https://github.com/atong01/conditional-flow-matching
- **Flow Matching 官方**: https://github.com/facebookresearch/flow_matching
- **Diffrax**: JAX 微分方程求解器

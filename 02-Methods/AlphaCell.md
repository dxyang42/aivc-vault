---
tags: [method, world-model, tongji, 2026, flow-matching]
year: 2026
institution: "同济大学 DELTA Lab"
authors: "啜国晖, 陈晓涵, 杨兴博, 高溢骋, 汪伟旭, 赵宇恒, 董科竟, 何斌, 刘琦, Fabian J. Theis"
architecture: "World Model + Flow Matching"
paper: "bioRxiv 2026.03"
code: "https://github.com/xcompbio/AlphaCell"
status: "已收录"
date: 2026-03-28
---

# AlphaCell 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[同济 DELTA Lab]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **方法名称** | AlphaCell |
| **发表时间** | 2026年3月 (预印本) |
| **开发团队** | 同济大学数字生命智能体实验室 (DELTA Lab) |
| **核心成员** | 啜国晖, 陈晓涵, 杨兴博, 高溢骋, 汪伟旭, 赵宇恒, 董科竟, 何斌, 刘琦, Fabian J. Theis |
| **合作机构** | 亥姆霍兹慕尼黑中心 |

## 核心技术

### 架构设计
AlphaCell是**虚拟细胞世界模型**，由两个核心组件组成：

1. **基础模型 (Foundation Model)**
   - 学习细胞状态表征
   - 全基因组表征学习

2. **流模型 (Flow Model)**
   - 基于**最优传输条件流匹配**
   - 模拟细胞状态转换

### 三大创新

| 创新点 | 描述 |
|--------|------|
| **1. 潜流形校正** | 通过掩码自监督学习、域对抗神经网络和弧面损失优化潜流形结构 |
| **2. 生物现实重建** | 高保真全基因组表达谱重建 |
| **3. 通用状态转换** | 基于最优传输条件流匹配模拟细胞状态转换 |

## 数学公式

### 条件流匹配目标函数

AlphaCell基于条件流匹配(Conditional Flow Matching, CFM)框架，学习从对照分布到扰动分布的传输映射：

$$v_\theta(x, t, c) = \mathbb{E}\left[\frac{x_1 - x_0}{1 - 0} \Big| x_t = x, c\right]$$

其中：
- $x_0 \sim p_0$ 为对照状态分布
- $x_1 \sim p_1$ 为扰动后状态分布
- $c = [c_{\text{pert}}; c_{\text{cell}}]$ 为扰动条件和细胞类型条件的拼接
- $t \in [0, 1]$ 为流时间参数

**条件流匹配损失函数**：

$$\mathcal{L}_{\text{CFM}} = \mathbb{E}_{t \sim \mathcal{U}(0,1), x_0 \sim p_0, x_1 \sim p_1}\left[\left\|v_\theta(x_t, t, c) - (x_1 - x_0)\right\|^2\right]$$

其中 $x_t = (1-t)x_0 + tx_1$ 为线性插值路径。

### 最优传输目标

AlphaCell采用最优传输理论进行状态转换建模：

$$\min_{T} \int_{\mathcal{X}} c(x, T(x)) \, dp_0(x) + \lambda \cdot \text{Reg}(T)$$

其中：
- $T: \mathcal{X} \rightarrow \mathcal{X}$ 为传输映射
- $c(x, y) = \|x - y\|^2$ 为传输成本（欧氏距离）
- $\text{Reg}(T)$ 为正则化项

### 潜流形校正公式

#### 1. 掩码自监督学习

$$\mathcal{L}_{\text{mask}} = \mathbb{E}_{\mathcal{M}} \left[ \sum_{i \in \mathcal{M}} \|x_i - \hat{x}_i\|^2 \right]$$

其中 $\mathcal{M}$ 为随机掩码的基因集合，$\hat{x}_i = \text{Decoder}(\text{Encoder}(x_{\setminus \mathcal{M}}))$。

#### 2. 域对抗训练

对抗损失确保潜流形的生物学一致性：

$$\min_G \max_D \mathcal{L}_{\text{adv}} = \mathbb{E}_{x \sim p_{\text{real}}} [\log D(x)] + \mathbb{E}_{z \sim p_z} [\log(1 - D(G(z)))]$$

其中：
- $G$ 为生成器（解码器）
- $D$ 为域判别器
- $p_{\text{real}}$ 为真实数据分布
- $p_z$ 为潜在空间分布

#### 3. 弧面损失 (ArcFace Loss)

用于优化细胞类型在潜流形上的分离：

$$\mathcal{L}_{\text{arc}} = -\frac{1}{N} \sum_{i=1}^{N} \log \frac{e^{s \cdot \cos(\theta_{y_i} + m)}}{e^{s \cdot \cos(\theta_{y_i} + m)} + \sum_{j \neq y_i} e^{s \cdot \cos(\theta_j)}}$$

其中：
- $s$ 为缩放因子
- $m$ 为边际参数
- $\theta_{y_i}$ 为细胞嵌入与细胞类型中心的角度

### 全基因组重建目标

$$\mathcal{L}_{\text{recon}} = \mathbb{E}_{x} \left[ \|x - \hat{x}\|^2 + \lambda_{\text{corr}} \cdot \text{Corr}(x, \hat{x}) \right]$$

其中 $\text{Corr}(\cdot, \cdot)$ 为基因间相关性保持项。

### 总损失函数

$$\mathcal{L}_{\text{total}} = \mathcal{L}_{\text{CFM}} + \lambda_1 \mathcal{L}_{\text{mask}} + \lambda_2 \mathcal{L}_{\text{adv}} + \lambda_3 \mathcal{L}_{\text{arc}} + \lambda_4 \mathcal{L}_{\text{recon}}$$

## 技术特点

- **世界模型 (World Model) 架构**: 构建虚拟细胞世界模型
- **全基因组表征**: 融合全基因组信息
- **连续状态转换建模**: 模拟细胞状态的连续变化
- **最优传输**: 使用最优传输理论进行状态转换

## 功能特点

| 能力 | 描述 |
|------|------|
| **零样本预测** | ✅ 支持对未见扰动的预测 |
| **组合泛化** | ✅ 在组合泛化场景中稳健预测扰动响应 |
| **高保真预测** | ✅ 全基因组高保真表达谱预测 |

## 性能评估

### 测试数据集
在三个多样化数据集上进行基准测试：

1. **OTF数据集**
2. **Sciplex数据集**
3. **Tahoe-100M数据集**

### 评估指标
- 定量保真度
- 调控逻辑

## 与CellHermes的关系

AlphaCell与**CellHermes**是同济DELTA Lab同时发布的两个互补方法：

| 方法 | 定位 | 核心技术 |
|------|------|---------|
| **AlphaCell** | 虚拟细胞世界模型 | 流匹配，全基因组表征 |
| **CellHermes** | 细胞语言模型 | 自然语言桥梁，LoRA微调 |

## 开源状态

- **预印本**: 已发布
- **开源代码**: 即将开源 (请关注相关发布)

## 应用前景

1. **虚拟药物筛选**
   - 在计算机上测试药物组合
   - 预测药物响应

2. **疾病建模**
   - 模拟疾病状态下的细胞行为
   - 理解疾病机制

3. **细胞工程**
   - 预测细胞重编程结果
   - 优化细胞分化

## 优势与局限

### 优势
- 世界模型架构，可解释性强
- 全基因组表征，信息完整
- 零样本预测能力
- 组合泛化能力强

### 局限
- 训练数据多样性相对有限
- 预印本阶段，待同行评审

## 相关论文

- AlphaCell预印本 (2026年3月)
- CellHermes预印本 (2026年3月)

## 团队背景

**同济大学数字生命智能体实验室 (DELTA Lab)**
- 专注于数字生命和虚拟细胞研究
- 与Fabian Theis等国际团队合作
- 在单细胞分析和虚拟细胞领域有重要贡献

---

*文档创建时间: 2026-03-28*

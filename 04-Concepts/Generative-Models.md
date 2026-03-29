---
tags: [concept, generative-model, deep-learning, vae, gan, diffusion]
date: 2026-03-30
---

# Generative Models

> [[概念地图]] | [[Deep-Learning]] | [[Generative-AI]]

## 概述

**生成模型 (Generative Models)** 是学习数据分布并生成新样本的机器学习模型。在单细胞分析中，生成模型用于数据增强、扰动预测和细胞状态生成。

## 主要类型

### 1. 变分自编码器 (VAE)

```
结构:
  输入 x → 编码器 → 隐变量 z → 解码器 → 重构 x'
  
目标:
  最大化证据下界 (ELBO)
  ELBO = E[log p(x|z)] - KL(q(z|x) || p(z))
  
在单细胞中的应用:
  - 降维和可视化
  - 批次校正
  - 扰动预测 (scGen, CPA)
```

### 2. 生成对抗网络 (GAN)

```
结构:
  生成器 G: z → x
  判别器 D: x → [0,1] (真假概率)
  
目标 ( minimax game ):
  min_G max_D E[log D(x)] + E[log(1 - D(G(z)))]
  
在单细胞中的应用:
  - 数据增强
  - 稀有细胞类型生成
```

### 3. 扩散模型 (Diffusion Models)

```
前向过程 (加噪):
  x_0 → x_1 → ... → x_T (纯噪声)
  
反向过程 (去噪):
  x_T → x_{T-1} → ... → x_0 (生成样本)
  
在单细胞中的应用:
  - 高质量细胞生成
  - 扰动效应建模
  - 见 [[Diffusion-Models]]
```

### 4. 流匹配/归一化流 (Flow-based)

```
核心思想:
  学习可逆变换 f: x ↔ z
  
优势:
  - 精确似然计算
  - 可逆变换
  
在单细胞中的应用:
  - 最优传输
  - 扰动轨迹建模
  - 见 [[Flow-Matching]]
```

### 5. 自回归模型

```
核心思想:
  p(x) = ∏ p(x_i | x_{<i})
  
在单细胞中的应用:
  - 基因表达序列建模
  - 细胞状态生成
```

## 在单细胞扰动预测中的应用

### 1. 条件生成

```
输入: 对照细胞 + 扰动条件
输出: 扰动后细胞

方法:
  - 条件VAE (CVAE)
  - 条件扩散模型
  - 流匹配模型
```

### 2. 反事实预测

```
问题: "如果施加扰动X，会怎样？"

生成模型回答:
  - 学习对照和扰动的映射
  - 生成反事实样本
```

### 3. 数据增强

```
应用:
  - 生成稀有扰动的合成数据
  - 平衡数据集
  - 提高模型泛化
```

## 评估指标

### 样本质量
- **真实性**: 样本是否像真实数据
- **多样性**: 生成的样本是否多样

### 分布匹配
- **FID (Fréchet Inception Distance)**: 分布距离
- **MMD (Maximum Mean Discrepancy)**: 核距离
- **Wasserstein Distance**: 最优传输距离

### 下游任务
- **分类准确率**: 生成数据的分类性能
- **聚类质量**: 生成数据的聚类效果

## 相关方法

- [[VAE-in-Single-Cell]] - VAE方法
- [[Diffusion-Models]] - 扩散模型
- [[Flow-Matching]] - 流匹配
- [[scGen]] - VAE扰动预测
- [[CPA]] - 条件VAE
- [[scDiffusion-Perturb]] - 扩散模型

## 优势

1. **数据增强**: 生成合成训练数据
2. **不确定性建模**: 捕获数据分布
3. **生成能力**: 创造新样本
4. **隐空间学习**: 有意义的表示

## 局限

1. **训练不稳定**: 特别是GAN
2. **模式崩溃**: 生成样本多样性不足
3. **计算成本**: 训练和采样成本高
4. **评估困难**: 生成质量难以量化

## 引用

```
Auto-Encoding Variational Bayes
Kingma & Welling, 2014

Denoising Diffusion Probabilistic Models
Ho et al., NeurIPS 2020

Generative Adversarial Networks
Goodfellow et al., NeurIPS 2014
```

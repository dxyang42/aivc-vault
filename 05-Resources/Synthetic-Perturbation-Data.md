---
tags: [resource, synthetic-data, data-augmentation, simulation]
date: 2026-03-30
---

# Synthetic Perturbation Data

> [[数据集资源]] | [[合成数据]] | [[数据增强]]

## 概述

合成扰动数据生成器和模拟框架，用于数据增强、方法开发和基准测试。

## 合成数据生成器

### 1. scDesign2

| 属性 | 内容 |
|------|------|
| **名称** | scDesign2 |
| **类型** | 单细胞数据模拟器 |
| **发表** | 2021 |
| **方法** |  copula-based生成模型 |

**功能**:
```
输入: 真实scRNA-seq数据
输出: 合成scRNA-seq数据

特点:
  - 保持基因间相关性
  - 可模拟批次效应
  - 可模拟不同细胞类型比例
```

**应用**:
- 方法基准测试
- 样本量规划
- 批次效应校正评估

### 2. SymSim

| 属性 | 内容 |
|------|------|
| **名称** | SymSim |
| **类型** | 单细胞模拟器 |
| **发表** | 2019 |
| **方法** | 生物物理模型 |

**特点**:
```
基于转录动力学模型:
  - 转录速率
  - 翻译速率
  - mRNA降解速率
  - 蛋白质降解速率

优势: 生物学可解释
```

### 3. SPARSim

| 属性 | 内容 |
|------|------|
| **名称** | SPARSim |
| **类型** | scRNA-seq模拟器 |
| **发表** | 2020 |
| **方法** | 统计模型 |

**特点**:
- 基于真实数据参数估计
- 模拟dropout效应
- 模拟文库大小差异

### 4. SERGIO

| 属性 | 内容 |
|------|------|
| **名称** | SERGIO |
| **类型** | 基因调控网络模拟器 |
| **发表** | 2020 |
| **方法** | 随机转录模型 |

**功能**:
```
输入: 基因调控网络 (GRN)
输出: 单细胞基因表达数据

应用:
  - GRN推断方法评估
  - 扰动效应模拟
```

## 扰动效应模拟

### 1. 简单扰动模型

```python
# 基础扰动效应模拟
def simulate_perturbation(control_expr, perturbation_strength, target_genes):
    """
    模拟基因敲低/敲除效应
    
    Args:
        control_expr: 对照组表达矩阵
        perturbation_strength: 扰动强度 (0-1)
        target_genes: 目标基因列表
    
    Returns:
        perturbed_expr: 扰动后表达矩阵
    """
    perturbed_expr = control_expr.copy()
    
    # 目标基因表达下调
    for gene in target_genes:
        perturbed_expr[gene] *= (1 - perturbation_strength)
    
    # 下游基因效应 (基于GRN)
    downstream_genes = get_downstream_genes(target_genes, grn)
    for gene in downstream_genes:
        perturbed_expr[gene] *= (1 - perturbation_strength * 0.5)
    
    return perturbed_expr
```

### 2. 基于网络传播

```python
def network_propagation_perturbation(control_expr, perturbed_gene, grn, alpha=0.5):
    """
    基于网络传播的扰动效应模拟
    
    使用随机游走模型传播扰动效应
    """
    n_genes = len(control_expr)
    perturbation_vector = np.zeros(n_genes)
    perturbation_vector[perturbed_gene] = 1.0
    
    # 网络传播
    influence = np.linalg.inv(np.eye(n_genes) - alpha * grn) @ perturbation_vector
    
    # 应用到表达
    perturbed_expr = control_expr * (1 - influence)
    
    return perturbed_expr
```

### 3. 基于VAE的生成

```python
# 使用预训练VAE生成扰动数据
def vae_generate_perturbation(vae_model, control_latent, perturbation_embedding):
    """
    在隐空间中应用扰动，解码生成扰动后数据
    """
    # 扰动隐空间表示
    perturbed_latent = control_latent + perturbation_embedding
    
    # 解码
    perturbed_expr = vae_model.decoder(perturbed_latent)
    
    return perturbed_expr
```

## 合成Benchmark

### 1. 已知Ground Truth

优势:
```
- 知道真实的扰动效应
- 可以评估方法准确性
- 可以控制数据复杂度
```

### 2. 参数化控制

可调节参数:
```
- 细胞数
- 基因数
- 扰动强度
- 批次效应强度
- dropout率
- 细胞类型异质性
```

### 3. 应用场景

| 场景 | 应用 |
|------|------|
| 方法开发 | 快速迭代测试 |
| 基准测试 | 公平比较不同方法 |
| 教学演示 | 理解方法原理 |
| 隐私保护 | 共享合成数据而非真实数据 |

## 相关工具

| 工具 | 功能 | 链接 |
|------|------|------|
| scDesign2 | 单细胞数据模拟 | R包 |
| SymSim | 生物物理模拟 | Python |
| SPARSim | 统计模拟 | R包 |
| SERGIO | GRN模拟 | Python |
| Splatter | 综合模拟平台 | Bioconductor |

## 使用建议

### 方法开发阶段
```
1. 使用合成数据快速原型
2. 调试算法
3. 参数调优
```

### 验证阶段
```
1. 在真实数据上验证
2. 与合成数据结果对比
3. 分析差异原因
```

### 注意事项
```
⚠️ 合成数据不能完全替代真实数据
⚠️ 注意合成数据与真实数据的分布差异
⚠️ 报告使用合成数据的局限性
```

## 相关资源

- [[scPerturb]] - 真实扰动数据
- [[Perturbation-Benchmark]] - 评估框架
- [[GEO-Perturbation]] - 公开数据集

## 引用

```
scDesign2: a user-friendly tool for generating synthetic scRNA-seq data
Sun et al., Bioinformatics 2021

SymSim: simulating multi-level and multi-scale single-cell gene expression
Zhang et al., Nature Communications 2019
```

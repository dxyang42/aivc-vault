---
title: sc_Perturbation_Transformer
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - transformer
  - generative-model
  - single-cell
  - 2025
  - github
---

# sc_Perturbation_Transformer

> Single-Cell Perturbation Transformer Model - GitHub Implementation

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | sc_Perturbation_Transformer |
| **发布年份** | 2025 |
| **代码平台** | GitHub |
| **代码链接** | https://github.com/NicoHadas/sc_Perturbation_Transformer |
| **核心算法** | Transformer-based Generative Model |
| **任务类型** | 单细胞遗传扰动效应预测 |

## 核心思想

这是一个基于**Transformer的生成模型**，用于预测单细胞数据中未见遗传扰动后的全基因组表达变化。该项目提供了扰动预测的开源实现，便于研究者复现和扩展。

### 关键特性

1. **生成式建模**: 预测扰动后的基因表达分布
2. **Transformer架构**: 利用自注意力捕获基因间关系
3. **零样本预测**: 对未见过的扰动进行预测
4. **开源实现**: 代码公开，易于使用和改进

## 模型结构

```
┌─────────────────────────────────────────────────────────────┐
│              sc_Perturbation_Transformer                    │
│              Transformer-based Generative Model             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Input:                                                    │
│   ├─ Pre-perturbation expression: x_pre in R^d             │
│   ├─ Perturbation target: p in {gene_ids}                  │
│   └─ Perturbation type: t in {KO, OE, KD}                  │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Input Embeddings                           │   │
│   │                                                      │   │
│   │   Gene expression embedding:                         │   │
│   │   ├─ e_expr = Linear(x_pre)                         │   │
│   │                                                      │   │
│   │   Perturbation embedding:                            │   │
│   │   ├─ e_pert = Embedding(p, t)                       │   │
│   │                                                      │   │
│   │   Combined input:                                    │   │
│   │   H_0 = Concat(e_expr, e_pert)                      │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Transformer Blocks                         │   │
│   │                                                      │   │
│   │   For i = 1 to N:                                    │   │
│   │   ├─ MultiHeadSelfAttention                          │   │
│   │   │   Q = K = V = H_{i-1}                           │   │
│   │   │   A = softmax(QK^T/sqrt(d)) · V                 │   │
│   │   │   H' = LayerNorm(H_{i-1} + A)                   │   │
│   │   │                                                  │   │
│   │   ├─ FeedForward                                     │   │
│   │   │   F = GELU(H'W_1 + b_1)W_2 + b_2                │   │
│   │   │   H_i = LayerNorm(H' + F)                       │   │
│   │                                                      │   │
│   │   H_N = Transformer(H_0)                            │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ↓                                                         │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │           Output Generation                          │   │
│   │                                                      │   │
│   │   Expression change prediction:                      │   │
│   │   ├─ delta_x = Linear(H_N)                          │   │
│   │                                                      │   │
│   │   Post-perturbation prediction:                      │   │
│   │   └─ x_post = x_pre + delta_x                       │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   Output: Predicted post-perturbation gene expression       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 数学公式

### 输入表示

基因表达嵌入通过线性变换将原始表达映射到模型维度，扰动嵌入通过查找表获取扰动目标基因和扰动类型的表示，两者拼接形成Transformer的初始输入。

### Transformer编码

输入经过N层Transformer编码，每层包含多头自注意力和前馈网络，通过层归一化和残差连接稳定训练。

### 输出预测

表达变化通过线性层从最终隐状态解码，扰动后预测等于原始表达加上预测的变化量。

### 训练损失

**Mean Squared Error:**
均方误差损失是预测扰动后表达与真实表达之间的平均平方差，衡量预测精度。

**Optional regularization:**
可选的L2正则化项防止模型过拟合，lambda控制正则化强度。

## 使用方法

### 安装

```bash
git clone https://github.com/NicoHadas/sc_Perturbation_Transformer.git
cd sc_Perturbation_Transformer
pip install -r requirements.txt
```

### 数据格式

```python
# Input format
{
    'expression': np.array([n_cells, n_genes]),  # Pre-perturbation
    'perturbation': list[str],                    # Target genes
    'perturbation_type': list[str]                # KO/OE/KD
}

# Output format
{
    'predicted_expression': np.array([n_cells, n_genes])
}
```

### 快速开始

```python
from sc_perturbation_transformer import PerturbationTransformer

# Initialize model
model = PerturbationTransformer(
    n_genes=5000,
    hidden_dim=512,
    n_layers=6,
    n_heads=8
)

# Train
model.fit(train_data, epochs=100)

# Predict
predictions = model.predict(test_data)
```

## 数据集支持

| 数据集 | 格式 | 预处理 |
|--------|------|--------|
| AnnData (.h5ad) | Native | Supported |
| 10x Genomics | mtx | Converter included |
| CSV | Custom | Loader provided |
| loom | Native | Supported |

## 性能基准

### 在标准数据集上的表现

| 数据集 | MSE | Pearson r | Spearman rho |
|--------|-----|-----------|--------------|
| Norman-2021 | 0.18 | 0.84 | 0.82 |
| Replogle-2022 | 0.22 | 0.81 | 0.79 |
| Dixit-2016 | 0.15 | 0.87 | 0.85 |

### 计算效率

| 配置 | Training Time | Inference (1k cells) | GPU Memory |
|------|---------------|----------------------|------------|
| Small (6L, 512D) | 2h | 5s | 4GB |
| Base (12L, 768D) | 6h | 12s | 8GB |
| Large (24L, 1024D) | 18h | 30s | 16GB |

## 优势与局限

### 优势
- ✅ 开源代码，易于使用
- ✅ 模块化设计，易于扩展
- ✅ 支持多种数据格式
- ✅ 详细的文档和示例

### 局限
- ❌ 需要GPU进行训练
- ❌ 大规模数据集需要较长训练时间
- ❌ 超参数调优需要经验

## 相关方法

- [[scPRAM]] - Similar transformer approach
- [[STATE]] - State embedding transformer
- [[scLAMBDA]] - LLM-based method
- [[scGen]] - VAE-based prediction
- [[GEARS]] - GNN-based approach

## 引用

```bibtex
@software{scperturbationtransformer2025,
  title={Single-Cell Perturbation Transformer Model},
  author={NicoHadas},
  year={2025},
  url={https://github.com/NicoHadas/sc_Perturbation_Transformer}
}
```

## 外部链接

- GitHub: https://github.com/NicoHadas/sc_Perturbation_Transformer
- 相关: [[Transformer]] | [[GitHub-Implementation]] | [[Open-Source]]

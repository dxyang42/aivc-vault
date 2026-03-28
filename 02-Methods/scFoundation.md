---
tags: [method, foundation-model, large-scale, single-cell, transformer]
year: 2024
institution: "Tsinghua University, Alibaba DAMO Academy"
authors: "Minghao Hao, Jing Gong, Xiangzhe Zeng, Chiming Liu, Yixin Wang, Xingyi Cheng, Taifeng Wang, Jian Zhang, Le Song"
architecture: "Transformer (xTrimoGene)"
paper: "https://www.nature.com/articles/s41592-024-02305-7"
code: "https://github.com/biomap-research/scFoundation"
status: "已收录"
date: 2026-03-29
---

# scFoundation

## 基本信息

- **全称**: Large-scale foundation model on single-cell transcriptomics
- **发表时间**: 2024年6月
- **机构**: 清华大学、阿里巴巴达摩院
- **作者**: Minghao Hao, Jing Gong, Xiangzhe Zeng, Chiming Liu, Yixin Wang, Xingyi Cheng, Taifeng Wang, Jian Zhang, Le Song

## 核心技术

scFoundation 是单细胞转录组学领域的大规模基础模型，具有以下特点：

1. **超大规模**: 1亿参数，覆盖约2万个蛋白编码基因
2. **海量数据**: 在超过5000万个人类单细胞转录组数据上预训练
3. **全基因组建模**: 创新的模型架构支持全基因组范围统一建模
4. **测序深度增强**: 独特的预训练任务可直接提升细胞数据质量

## 架构细节

### xTrimoGene 架构

scFoundation 采用 xTrimoGene 架构，专为单细胞数据设计：

```
┌─────────────────────────────────────────────────────────────┐
│                   xTrimoGene Architecture                   │
├─────────────────────────────────────────────────────────────┤
│  输入层                                                     │
│  ├── 基因表达值 (非零元素)                                  │
│  └── 基因 ID 嵌入                                           │
├─────────────────────────────────────────────────────────────┤
│  编码器 (Transformer × 24)                                  │
│  ├── Self-Attention (Gene-Gene Interaction)                 │
│  ├── Feed-Forward Network                                   │
│  └── LayerNorm + Residual                                   │
├─────────────────────────────────────────────────────────────┤
│  解码器                                                     │
│  ├── 非零基因预测头                                         │
│  └── 表达值回归头                                           │
├─────────────────────────────────────────────────────────────┤
│  输出层                                                     │
│  └── 细胞嵌入 (1024-dim)                                    │
└─────────────────────────────────────────────────────────────┘
```

### 关键创新

1. **非零元素注意力**: 利用单细胞数据的稀疏性，仅对非零基因计算注意力
2. **基因位置编码**: 可学习的位置编码替代标准位置编码
3. **双任务预训练**: 同时预测基因存在性和表达量

## 预训练任务

### 任务1: 掩码基因建模 (Masked Gene Modeling)

随机掩码部分基因，预测其表达值：

$$\mathcal{L}_{MGM} = \mathbb{E}\left[\|x_{masked} - \hat{x}_{masked}\|^2\right]$

### 任务2: 基因存在性预测 (Gene Presence Prediction)

预测基因是否表达（二分类）：

$$\mathcal{L}_{GPP} = -\mathbb{E}\left[y \log(\hat{y}) + (1-y)\log(1-\hat{y})\right]$$

### 总损失

$$\mathcal{L}_{total} = \mathcal{L}_{MGM} + \lambda \mathcal{L}_{GPP}$$

## 模型配置

| 参数 | 值 |
|-----|-----|
| 参数量 | 100M |
| 层数 | 24 |
| 隐藏维度 | 1024 |
| 注意力头数 | 16 |
| FFN 维度 | 4096 |
| 最大基因数 | ~20,000 |
| 预训练数据 | 50M+ 细胞 |

## 输入/输出

- **输入**:
  - 单细胞基因表达数据（支持可变长度）
- **输出**:
  - 1024 维细胞嵌入
  - 基因表达预测
  - 测序深度增强后的表达谱

## 性能指标

scFoundation 在多个下游任务上表现优异：

| 任务 | 指标 | 性能 |
|-----|------|------|
| 细胞类型注释 | Accuracy | 94.2% |
| 批次校正 | kBET | 0.91 |
| 测序深度增强 | MSE | 优于现有方法 |
| 药物响应预测 | AUC | 0.87 |

## 功能特点

1. **开箱即用**: 预训练模型可直接用于数据增强
2. **微调支持**: 支持下游任务的微调
3. **测序深度增强**: 独特的预训练任务可提升低质量数据
4. **细胞嵌入**: 提取的嵌入可用于多种分析
5. **基因模块识别**: 可识别细胞类型特异基因模块

## 应用场景

- 细胞类型注释
- 批次效应校正
- 测序深度增强
- 药物响应预测
- 基因调控网络推断

## 开源资源

- **论文**: https://www.nature.com/articles/s41592-024-02305-7
- **代码**: https://github.com/biomap-research/scFoundation
- **模型权重**: 提供 3M、10M、100M 三种规模

## 优势与局限

**优势**:
- 大规模预训练（5000万细胞）
- 全基因组建模能力
- 测序深度增强功能
- 多种模型规模可选

**局限**:
- 需要较大计算资源
- 主要针对人类数据
- 推理速度相对较慢

## 相关论文

- Hao, M., Gong, J., Zeng, X., Liu, C., Wang, Y., Cheng, X., Wang, T., Zhang, J., & Song, L. (2024). Large-scale foundation model on single-cell transcriptomics. *Nature Methods*, 21, 1481–1491.

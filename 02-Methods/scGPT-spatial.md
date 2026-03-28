---
tags: [method, foundation-model, spatial-transcriptomics, single-cell, transformer]
year: 2025
institution: "University of Toronto, Vector Institute"
authors: "Chloe Xueqi Wang, Haotian Cui, Andrew Hanzhuo Zhang, Ronald Xie, Hani Goodarzi, Bo Wang"
architecture: "Transformer + Spatial Encoding"
paper: "https://www.biorxiv.org/content/10.1101/2025.02.05.636714v1"
code: "https://github.com/bowang-lab/scGPT-spatial"
status: "已收录"
date: 2026-03-29
---

# scGPT-spatial

## 基本信息

- **全称**: scGPT-spatial: Continual Pretraining of Single-Cell Foundation Model for Spatial Transcriptomics
- **发表时间**: 2025年2月
- **机构**: 多伦多大学、Vector Institute
- **作者**: Chloe Xueqi Wang, Haotian Cui, Andrew Hanzhuo Zhang, Ronald Xie, Hani Goodarzi, Bo Wang

## 核心技术

scGPT-spatial 是 scGPT 的空间转录组学扩展版本，通过持续预训练将单细胞基础模型的能力迁移到空间转录组数据分析：

1. **持续预训练**: 在 scGPT 基础上继续预训练，保留单细胞知识的同时学习空间信息
2. **空间感知编码**: 引入空间位置编码，建模细胞间的空间关系
3. **MoE 解码器**: 采用混合专家（Mixture of Experts）解码器处理空间数据的异质性

## 架构细节

### 整体架构

```
┌─────────────────────────────────────────────────────────────┐
│                 scGPT-spatial Architecture                  │
├─────────────────────────────────────────────────────────────┤
│  输入层                                                     │
│  ├── 基因表达: x ∈ ℝ^n_genes                               │
│  ├── 空间坐标: (s_x, s_y) ∈ ℝ²                             │
│  └── 组织区域: r ∈ {1, ..., n_regions}                     │
├─────────────────────────────────────────────────────────────┤
│  空间编码器                                                 │
│  ├── 基因嵌入 (来自 scGPT)                                  │
│  ├── 空间位置编码 (Spatial PE)                              │
│  │   └── 2D Sinusoidal / Learnable                          │
│  └── 区域嵌入 (Region Embedding)                            │
├─────────────────────────────────────────────────────────────┤
│  Transformer 骨干 (scGPT 预训练权重)                        │
│  ├── Self-Attention × 12                                    │
│  └── Cross-Spatial Attention                                │
├─────────────────────────────────────────────────────────────┤
│  MoE 解码器                                                 │
│  ├── Expert 1: 肿瘤区域专家                                 │
│  ├── Expert 2: 正常组织专家                                 │
│  ├── Expert 3: 边界区域专家                                 │
│  └── Gating Network: 动态路由                               │
├─────────────────────────────────────────────────────────────┤
│  输出层                                                     │
│  └── 空间感知基因表达预测                                   │
└─────────────────────────────────────────────────────────────┘
```

### 空间位置编码

scGPT-spatial 采用二维正弦位置编码编码空间坐标：

$$PE_{(s_x, s_y)}^{(2i)} = \sin\left(\frac{s_x}{10000^{2i/d_{model}}}\right) + \sin\left(\frac{s_y}{10000^{2i/d_{model}}}\right)$$

$$PE_{(s_x, s_y)}^{(2i+1)} = \cos\left(\frac{s_x}{10000^{2i/d_{model}}}\right) + \cos\left(\frac{s_y}{10000^{2i/d_{model}}}\right)$$

### 空间感知注意力

引入空间距离到注意力计算：

$$\text{SpatialAttn}_{ij} = \frac{Q_i K_j^T}{\sqrt{d_k}} - \lambda \cdot \|s_i - s_j\|_2$$

其中 $\lambda$ 控制空间衰减强度。

### MoE 解码器

混合专家解码器处理空间异质性：

$$h_{out} = \sum_{k=1}^{K} g_k(x) \cdot E_k(x)$$

其中：
- $E_k$: 第 $k$ 个专家网络
- $g_k$: 门控网络输出的权重
- $K$: 专家数量（通常 4-8）

门控网络采用 Top-K 稀疏路由：

$$g_k(x) = \frac{\exp(w_k^T x)}{\sum_{j \in TopK} \exp(w_j^T x)} \cdot \mathbb{1}_{k \in TopK}$$

## 持续预训练策略

### 预训练任务

1. **掩码基因建模 (MGM)**
   - 随机掩码部分基因表达
   - 预测被掩码基因的值

2. **空间上下文预测**
   - 给定中心细胞，预测邻居细胞

3. **组织区域分类**
   - 预测细胞所属的组织区域

### 训练数据

- 空间转录组数据集（Visium、Slide-seq、MERFISH 等）
- 包含多种组织类型（肿瘤、脑、胚胎等）
- 总计数百万个空间位置

## 输入/输出

- **输入**:
  - 空间转录组数据（基因表达 + 空间坐标）
  - 组织区域信息
- **输出**:
  - 空间感知的基因表达嵌入
  - 细胞类型预测
  - 空间聚类

## 性能指标

- 空间聚类准确性
- 细胞类型注释准确率
- 空间域识别性能

## 应用场景

- 肿瘤微环境分析
- 发育生物学研究
- 神经科学（脑图谱）
- 药物响应空间异质性

## 开源资源

- **论文**: https://www.biorxiv.org/content/10.1101/2025.02.05.636714v1
- **代码**: https://github.com/bowang-lab/scGPT-spatial

## 优势与局限

**优势**:
- 继承 scGPT 的单细胞知识
- 空间感知建模
- 处理空间数据异质性

**局限**:
- 依赖空间转录组数据质量
- 计算成本较高

## 相关论文

- Wang, C.X., et al. (2025). scGPT-spatial: Continual Pretraining of Single-Cell Foundation Model for Spatial Transcriptomics. *bioRxiv*.
- Cui, H., et al. (2024). scGPT: toward building a foundation model for single-cell multi-omics using generative AI. *Nature Methods*.

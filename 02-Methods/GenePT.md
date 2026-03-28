---
tags: [method, foundation-model, gene-embedding, language-model, zero-shot]
year: 2023
institution: "Stanford University"
authors: "Yiqun T. Chen, Jingyi Z. Cui, James Zou"
architecture: "Language Model Embeddings"
paper: "https://arxiv.org/abs/2310.07265"
code: "https://github.com/yiqunchen/GenePT"
status: "已收录"
date: 2026-03-29
---

# GenePT

## 基本信息

- **全称**: GenePT: A Simple But Hard-to-Beat Foundation Model for Genes and Cells Built From ChatGPT
- **发表时间**: 2023年10月
- **机构**: 斯坦福大学
- **作者**: Yiqun T. Chen, Jingyi Z. Cui, James Zou

## 核心技术

GenePT 是一个简单但有效的基因和细胞基础模型，核心思想是利用大型语言模型（ChatGPT/GPT-3.5）的隐含生物学知识：

1. **文本嵌入基因**: 使用 NCBI 基因文本描述的 GPT 嵌入作为基因表示
2. **加权平均构建细胞嵌入**: 根据基因表达量加权平均基因嵌入
3. **零样本能力**: 无需训练即可用于多种下游任务

## 方法原理

### 基因嵌入生成

对于每个基因，从 NCBI 获取其文本描述，使用 GPT-3.5 生成嵌入：

$$e_{gene} = \text{GPT-Embed}(\text{TextDescription}_{NCBI}(gene)) \in \mathbb{R}^{1536}$$

### 细胞嵌入构建

给定单细胞基因表达向量 $x \in \mathbb{R}^{n_{genes}}$，细胞嵌入为基因嵌入的加权平均：

$$e_{cell} = \frac{\sum_{i=1}^{n_{genes}} x_i \cdot e_{gene_i}}{\sum_{i=1}^{n_{genes}} x_i} \in \mathbb{R}^{1536}$$

也可使用对数变换：

$$e_{cell} = \frac{\sum_{i=1}^{n_{genes}} \log(1 + x_i) \cdot e_{gene_i}}{\sum_{i=1}^{n_{genes}} \log(1 + x_i)}$$

## 架构特点

### 极简设计

```
基因文本描述 (NCBI)
        ↓
GPT-3.5 Embedding API
        ↓
基因嵌入矩阵 E ∈ ℝ^(n_genes × 1536)
        ↓
细胞表达 x ∈ ℝ^n_genes
        ↓
加权平均: e_cell = x^T E / sum(x)
        ↓
下游任务 (聚类/分类/可视化)
```

### 无需训练

GenePT 的核心优势在于无需任何训练：
- 不需要预训练数据
- 不需要 GPU 资源
- 不需要调参

## 输入/输出

- **输入**:
  - 单细胞基因表达数据
  - NCBI 基因描述文本（预计算）
- **输出**:
  - 1536 维细胞嵌入向量
  - 可用于聚类、分类、可视化

## 性能指标

在多个基准测试中，GenePT 表现优异：

| 任务 | GenePT | scBERT | scGPT | Geneformer |
|-----|--------|--------|-------|------------|
| 细胞类型聚类 | 0.85 | 0.82 | 0.88 | 0.83 |
| 批次校正 | 0.78 | 0.75 | 0.80 | 0.76 |
| 通路分析 | 0.72 | 0.68 | 0.74 | 0.70 |

## 功能特点

1. **零样本学习**: 无需训练即可使用
2. **计算高效**: 仅需要矩阵乘法
3. **可解释性**: 基因嵌入来自文本描述
4. **跨物种**: 支持人类、小鼠等多种物种
5. **持续更新**: 可随 LLM 升级而更新

## 应用场景

- 细胞类型注释
- 批次效应校正
- 通路富集分析
- 药物靶点预测
- 基因功能预测

## 开源资源

- **论文**: https://arxiv.org/abs/2310.07265
- **代码**: https://github.com/yiqunchen/GenePT
- **预计算嵌入**: 已提供人类和小鼠基因嵌入

## 优势与局限

**优势**:
- 极简设计，易于使用
- 无需训练，零计算成本
- 利用 LLM 的隐含知识
- 跨物种泛化能力强

**局限**:
- 依赖 LLM 的文本理解能力
- 对未见基因的泛化有限
- 无法捕捉复杂的基因-基因交互

## 相关论文

- Chen, Y.T., Cui, J.Z., & Zou, J. (2023). GenePT: A Simple But Hard-to-Beat Foundation Model for Genes and Cells Built From ChatGPT. *arXiv preprint arXiv:2310.07265*.
- Chen, Y.T., & Zou, J. (2024). GenePT: a foundation model for genomics and transcriptomics. *Nature Methods*.

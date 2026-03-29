---
tags: [method, multicellular-programs, cell-cell-interaction, tissue-analysis, 2022]
year: 2022
version: "1.0"
institution: "Stanford University / Weizmann Institute"
authors: "Livnat Jerby-Arnon, et al."
architecture: "Penalized Canonical Variates Analysis"
paper: "Nature Biotechnology 2022"
code: "https://github.com/livnatje/DIALOGUE"
status: "已收录"
date: 2026-03-29
---

# DIALOGUE

## 基本信息

| 属性 | 内容 |
|------|------|
| **名称** | DIALOGUE |
| **作者/团队** | Livnat Jerby-Arnon (Stanford/Weizmann Institute) |
| **发表时间** | 2022 |
| **发表期刊** | Nature Biotechnology |
| **代码仓库** | https://github.com/livnatje/DIALOGUE |
| **论文链接** | https://doi.org/10.1038/s41587-022-01288-0 |

## 核心思想

DIALOGUE 是一种**多细胞程序（Multicellular Programs, MCPs）**发现方法，用于系统性地识别不同细胞类型间协调的基因表达程序。这些 MCPs 代表组织水平的功能单元，揭示细胞间如何协同响应扰动。

核心创新：
1. 跨细胞类型的关联分析，而非单独分析每种细胞类型
2. 将细胞转录组建模为其微环境的函数
3. 识别与表型相关的多细胞功能单元

## 方法架构

```
         Input: Single-cell data from multiple samples
                (with cell type labels)
                    |
                    v
         +-------------------------+
         |  Sample-level Aggregation|
         |  (Mean expr per cell type|
         |   per sample)            |
         +------------+------------+
                      |
        +-------------+-------------+
        |                           |
        v                           v
+----------------+        +----------------+
| Cell Type A    |        | Cell Type B    |
| Expression     |        | Expression     |
| Matrix (samples|        | Matrix (samples|
| x genes)       |        | x genes)       |
+--------+-------+        +--------+-------+
         |                           |
         +-------------+-------------+
                       |
                       v
         +-------------------------+
         |  Penalized Canonical      |
         |  Variates Analysis (CVA)  |
         |                           |
         |  Finds correlated gene    |
         |  programs across cell     |
         |  types                    |
         +------------+------------+
                      |
                      v
         +-------------------------+
         |  Multilevel Modeling      |
         |                           |
         |  - Fixed effects: MCP     |
         |  - Random effects: Sample |
         +------------+------------+
                      |
                      v
         +-------------------------+
         |  Multicellular Program    |
         |  (MCP) Definition         |
         |                           |
         |  MCP = {Gene sets for     |
         |    each cell type}        |
         +------------+------------+
                      |
          +-----------+-----------+
          |                       |
          v                       v
+----------------+      +------------------+
| MCP Scoring    |      | Phenotype Assoc. |
| (Cell-level)   |      | (Sample-level)   |
+----------------+      +------------------+
```

## 算法流程

1. **样本聚合**: 计算每个样本中每种细胞类型的平均表达
2. **惩罚 CVA**: 识别跨细胞类型共变的基因程序
3. **多层次建模**: 分离 MCP 效应与样本特异性变异
4. **MCP 定义**: 为每种细胞类型定义 MCP 基因集
5. **单细胞评分**: 将 MCP 映射回单细胞水平
6. **表型关联**: 检验 MCP 与表型的关联

## Perturbation 预测能力

### 核心功能

| 功能 | 描述 |
|------|------|
| **MCP 发现** | 识别协调的多细胞基因程序 |
| **扰动响应** | 分析 MCP 如何响应实验扰动 |
| **细胞互作** | 推断细胞类型间的功能联系 |
| **表型预测** | 使用 MCP 评分预测样本表型 |

### 适用场景

- **肿瘤微环境**: 解析肿瘤-免疫-基质细胞的协同程序
- **组织功能**: 理解器官水平的功能单元
- **疾病机制**: 识别疾病相关的多细胞失调
- **药物响应**: 预测药物对多细胞系统的影响

## 与其他方法的对比

| 方法 | 主要区别 | 适用场景 |
|------|----------|----------|
| **DIALOGUE** | 多细胞程序发现 | 组织水平功能单元 |
| **NicheNet** | 配体-靶点预测 | 细胞间信号推断 |
| **CellChat** | 配体-受体分析 | 通讯网络构建 |
| **MILO** | 细胞邻域分析 | 空间细胞互作 |

## 优势与局限

### 优势
- 揭示组织水平的功能组织
- 不依赖空间信息即可推断细胞互作
- 可推广到新数据集
- 提供可解释的 MCP 评分

### 局限
- 需要多个样本（建议 >10）
- 需要预先定义的细胞类型标签
- 对稀有细胞类型效果有限
- 计算成本较高

## 典型应用

1. **肿瘤免疫**: 识别肿瘤-免疫协同程序与患者预后关联
2. **自身免疫病**: 解析炎症组织中多细胞失调
3. **发育生物学**: 理解器官发育中的细胞协同
4. **感染研究**: 分析病原体感染的多细胞响应

## 相关资源

- **GitHub**: https://github.com/livnatje/DIALOGUE
- **Nature Biotechnology 文章**: https://doi.org/10.1038/s41587-022-01288-0

## 引用

```
Jerby-Arnon L, et al. (2022). DIALOGUE maps multicellular programs in 
tissue from single-cell or spatial data. Nature Biotechnology, 40(10), 
1457-1467.
```

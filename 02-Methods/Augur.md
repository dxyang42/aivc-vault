---
tags: [method, cell-type-prioritization, classification, 2020, machine-learning]
year: 2020
version: "1.0"
institution: "University of British Columbia"
authors: "Michael Skinnider, Jordan Squair, et al."
architecture: "Machine Learning Classifier"
paper: "Nature Methods 2020"
code: "https://github.com/neurorestore/Augur"
status: "已收录"
date: 2026-03-29
---

# Augur

## 基本信息

| 属性 | 内容 |
|------|------|
| **名称** | Augur |
| **作者/团队** | Michael Skinnider, Jordan Squair (University of British Columbia) |
| **发表时间** | 2020 (方法), 2021 (Protocol) |
| **发表期刊** | Nature Methods / Nature Protocols |
| **代码仓库** | https://github.com/neurorestore/Augur |
| **论文链接** | https://doi.org/10.1038/s41592-020-01003-1 |

## 核心思想

Augur 是一种**细胞类型响应优先级排序**方法，用于识别在复杂组织中哪些细胞类型对实验扰动最为敏感。其核心假设是：**对扰动响应强烈的细胞类型，其分子特征会使扰动组与对照组更容易区分**。

Augur 不直接预测扰动效应，而是通过机器学习框架量化细胞类型的"可分离性"（separability），从而推断其响应强度。

## 方法架构

```
              Input: Single-cell data with perturbation labels
                              |
        +---------------------+---------------------+
        |                                           |
   Cell Type A                              Cell Type B
   (e.g., Neurons)                           (e.g., Microglia)
        |                                           |
        v                                           v
   +--------------------+                 +--------------------+
   | Subset Data        |                 | Subset Data        |
   | (Cells of Type A)  |                 | (Cells of Type B)  |
   +---------+----------+                 +---------+----------+
             |                                      |
             v                                      v
   +--------------------+                 +--------------------+
   | Feature Selection  |                 | Feature Selection  |
   | (High variance     |                 | (High variance     |
   |  genes)            |                 |  genes)            |
   +---------+----------+                 +---------+----------+
             |                                      |
             v                                      v
   +--------------------+                 +--------------------+
   | Cross-Validation   |                 | Cross-Validation   |
   | Classifier Training|                 | Classifier Training|
   | (RF/XGBoost/SVM)   |                 | (RF/XGBoost/SVM)   |
   +---------+----------+                 +---------+----------+
             |                                      |
             v                                      v
   +--------------------+                 +--------------------+
   | AUC Score          |                 | AUC Score          |
   | (Separability)     |                 | (Separability)     |
   | e.g., 0.95         |                 | e.g., 0.62         |
   +---------+----------+                 +---------+----------+
             |                                      |
             +------------------+-------------------+
                                |
                                v
                  +-------------------------+
                  |  Cell Type Ranking      |
                  |                         |
                  |  1. Neurons (AUC=0.95)  |
                  |  2. Astrocytes (0.78)   |
                  |  3. Microglia (0.62)    |
                  +-------------------------+
```

## 算法流程

1. **细胞类型分层**: 按细胞类型标签分割数据
2. **特征选择**: 选择高变异基因作为分类特征
3. **分类器训练**: 使用随机森林/XGBoost/SVM 训练扰动 vs 对照分类器
4. **交叉验证**: 采用分层交叉验证评估分类性能
5. **AUC 计算**: 以 AUC 作为细胞类型响应强度的度量
6. **统计检验**: 与随机标签进行置换检验，确定显著性

## Perturbation 预测能力

### 核心功能

| 功能 | 描述 |
|------|------|
| **响应排序** | 对组织中所有细胞类型按响应强度排序 |
| **差异比较** | 比较不同扰动条件下细胞类型的响应差异 |
| **时间序列** | 分析扰动响应的时间动态变化 |
| **亚型分析** | 识别响应最强的细胞亚群 |

### 适用场景

- **疾病模型**: 识别疾病中最受影响的细胞类型
- **药物筛选**: 确定药物作用的靶细胞类型
- **发育研究**: 追踪发育过程中细胞类型的响应变化

## 与其他方法的对比

| 方法 | 主要区别 | 适用场景 |
|------|----------|----------|
| **Augur** | 细胞类型级别的响应排序 | 复杂组织的多细胞类型分析 |
| **MELD** | 单细胞级别的连续效应估计 | 需要精细的效应梯度 |
| **Mixscape** | Perturb-seq 专用 QC | CRISPR 筛选数据预处理 |
| **scGen** | 生成式扰动预测 | 预测未见扰动的转录组变化 |

## 优势与局限

### 优势
- 适用于任何单细胞数据类型（scRNA-seq, scATAC-seq 等）
- 不依赖先验知识，完全数据驱动
- 提供统计显著性评估
- 支持多种分类器 backend

### 局限
- 需要足够的细胞数量（建议每个细胞类型 >50 个细胞）
- 对批次效应敏感，需要预先校正
- 不能直接预测基因级别的变化

## 典型应用

1. **神经退行性疾病**: 识别脊髓损伤后最响应的神经元亚型
2. **免疫治疗**: 确定肿瘤微环境中对免疫检查点抑制剂响应的细胞类型
3. **发育生物学**: 追踪器官发育过程中细胞类型的分化响应

## 相关资源

- **Nature Protocols 文章**: https://doi.org/10.1038/s41596-021-00561-x
- **R 包文档**: https://github.com/neurorestore/Augur

## 引用

```
Skinnider MA, et al. (2021). Cell type prioritization in single-cell data. 
Nature Protocols, 16(10), 4826-4851.

Squair JW, et al. (2020). Confronting false discoveries in single-cell 
differential expression. Nature Communications, 11(1), 6242.
```

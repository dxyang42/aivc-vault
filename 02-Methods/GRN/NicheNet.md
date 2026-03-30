---
tags: [method, cell-communication, ligand-receptor, network-inference, 2019]
year: 2019
institution: "VIB-UGent Center for Inflammation Research (Saeys Lab)"
authors: "Robin Browaeys, Yvan Saeys, et al."
architecture: "Network-based Inference"
paper: "Nature Methods 2019"
code: "https://github.com/saeyslab/nichenetr"
status: "已收录"
date: 2026-03-29
---

# NicheNet

## 基本信息

| 属性 | 内容 |
|------|------|
| **名称** | NicheNet |
| **作者/团队** | Saeys Lab (VIB-UGent Center for Inflammation Research) |
| **发表时间** | 2019 (方法), 2020 (更新), 2024 (Protocol) |
| **发表期刊** | Nature Methods / Nature Protocols |
| **代码仓库** | https://github.com/saeyslab/nichenetr |
| **论文链接** | https://doi.org/10.1038/s41592-019-0667-5 |

## 核心思想

NicheNet 是一种**细胞间通讯推断**方法，通过整合先验知识（信号通路、基因调控网络）来预测哪些配体-受体相互作用可能驱动接收细胞的基因表达变化。

核心创新：
1. 构建配体→受体→信号介质→靶基因的完整调控路径
2. 结合多种数据源（蛋白质互作、基因调控、信号通路）
3. 从表达数据反推活跃的细胞间通讯

## 方法架构

```
                    Input: scRNA-seq Data
                           |
        +------------------+------------------+
        |                                     |
   Sender Cells                      Receiver Cells
   (Ligand sources)                  (Expression changes)
        |                                     |
        v                                     v
   +-------------------+            +-------------------+
   | Ligand Expression |            | DEG Analysis      |
   | (Expressed genes) |            | (vs Control)      |
   +---------+---------+            +---------+---------+
             |                                |
             +----------------+----------------+
                              |
                              v
              +-------------------------------+
              |   Prior Knowledge Network     |
              |                               |
              |   +-----------------------+   |
              |   | Ligand-Receptor Links |   |
              |   +-----------+-----------+   |
              |               |               |
              |   +-----------v-----------+   |
              |   | Signaling Mediators   |   |
              |   | (Transcription Factors)|   |
              |   +-----------+-----------+   |
              |               |               |
              |   +-----------v-----------+   |
              |   | Target Gene Links     |   |
              |   +-----------------------+   |
              +---------------+---------------+
                              |
                              v
              +-------------------------------+
              |   Regulatory Potential Model  |
              |                               |
              |   P(target|ligand) =          |
              |   weighted path strength      |
              +---------------+---------------+
                              |
              +---------------+---------------+
              |                               |
              v                               v
   +----------------------+      +----------------------+
   | Ligand Prioritization |      | Target Prediction    |
   | (Active ligands)      |      | (Affected genes)     |
   +----------------------+      +----------------------+
```

## 算法流程

1. **配体筛选**: 识别发送细胞中表达的配体
2. **差异表达分析**: 识别接收细胞中的差异表达基因
3. **调控潜力计算**: 基于先验网络计算每个配体对靶基因的调控潜力
4. **活性评分**: 结合表达数据和调控潜力计算配体活性分数
5. **靶基因预测**: 预测每个活性配体影响的靶基因集合

## Perturbation 预测能力

### 核心功能

| 功能 | 描述 |
|------|------|
| **配体预测** | 识别驱动接收细胞变化的候选配体 |
| **靶基因预测** | 预测配体调控的下游靶基因 |
| **信号通路推断** | 推断参与信号转导的中间分子 |
| **细胞通讯图谱** | 构建细胞类型间的通讯网络 |

### 适用场景

- **微环境研究**: 解析肿瘤微环境中的细胞通讯
- **发育生物学**: 理解胚胎发育中的信号传递
- **免疫学**: 分析免疫细胞间的相互作用
- **药物靶点发现**: 识别潜在的治疗靶点

## 与其他方法的对比

| 方法 | 主要区别 | 适用场景 |
|------|----------|----------|
| **NicheNet** | 整合先验知识的因果推断 | 需要解释机制的细胞通讯 |
| **CellChat** | 基于配体-受体数据库 | 快速构建通讯网络 |
| **CellPhoneDB** | 统计检验显著性 | 大规模通讯筛选 |
| **DIALOGUE** | 多细胞程序分析 | 组织水平的功能单元 |

## 优势与局限

### 优势
- 整合多层次的生物学知识
- 提供因果机制解释
- 支持 bulk 和 single-cell 数据
- 可预测未见配体的效应

### 局限
- 依赖先验知识网络的完整性
- 对稀有配体/受体可能预测不准
- 不能捕捉未知的信号通路

## 典型应用

1. **肿瘤免疫**: 识别肿瘤-免疫细胞通讯的关键配体
2. **组织修复**: 解析损伤修复过程中的细胞间信号
3. **干细胞微环境**: 理解干细胞 niche 的调控机制
4. **感染免疫**: 分析病原体-宿主细胞相互作用

## 相关资源

- **NicheNet 网站**: https://nichenet.be/
- **Vignette**: https://github.com/saeyslab/nichenetr/blob/main/vignettes/ligand_activity_geneset.md
- **Nature Protocols**: https://doi.org/10.1038/s41596-024-01121-9

## 引用

```
Browaeys R, et al. (2019). NicheNet: modeling intercellular communication 
by linking ligands to target genes. Nature Methods, 17(2), 159-162.

Tikhonova AN, et al. (2024). Unraveling cell-cell communication with 
NicheNet by inferring active ligands from transcriptomics data. 
Nature Protocols, 19(10), 2977-3017.
```

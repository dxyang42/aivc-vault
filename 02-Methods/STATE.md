---
tags: [method, transformer, arc, 2025, state-aware]
year: 2025
institution: "Arc Institute / UC Berkeley / Stanford"
authors: "Patrick Hsu, Yusuf Roohani, Paul Datlinger, Emma Lundberg, Theofanis Karaletsos"
architecture: "Transformer"
paper: "bioRxiv 2025"
code: "https://github.com/snap-stanford/stark"
status: "已收录"
date: 2026-03-28
---

# STATE 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **全称** | STate-Aware Transition Embedding |
| **发表时间** | 2025年 |
| **发表平台** | Virtual Cell Challenge 2025, bioRxiv预印本 |
| **开发团队** | Arc Institute, UC Berkeley, Stanford |
| **核心成员** | Patrick Hsu, Yusuf Roohani, Paul Datlinger, Emma Lundberg, Theofanis Karaletsos |

## 核心技术

### 架构设计
STATE采用**双模块架构**：

1. **STATE Transition (ST) 模块**
   - 基于**Transformer架构**
   - 通过**自注意力机制**学习细胞在干预下的转化过程
   - 最佳集合大小：256个细胞

2. **STATE Embedding (SE) 模块**
   - 通过预训练学习细胞嵌入
   - 提升预测准确性

### 技术特点
- 基于细胞集合的自注意力机制
- 支持单细胞分辨率转录组预测
- 支持嵌入预测两种模式
- 多尺度机器学习架构

## 训练数据

| 数据属性 | 规模 |
|---------|------|
| **观测细胞** | 1.7亿个 |
| **扰动细胞** | 1亿个 |
| **细胞系** | 70种人类细胞系 |
| **数据来源** | Tahoe-100M, Parse-PMBC, Replogle-Nadig等 |

## 性能表现

| 指标 | 结果 |
|------|------|
| **Tahoe-100M准确率提升** | 50% |
| **DES指标提升** | 100% |
| **差异表达基因识别** | 准确率是现有模型的2倍 |
| **预测能力** | 化学、遗传、细胞因子扰动 |

## 功能特点

- ✅ **零样本预测**: 支持对未见扰动的预测
- ✅ **多扰动类型**: 化学、遗传、细胞因子
- ✅ **大规模训练**: 基于1亿+细胞数据
- ✅ **开源**: 支持非商业用途

## 开源资源

- **GitHub**: https://github.com/ArcInstitute
- **虚拟细胞图谱**: https://arcinstitute.org/virtual-cell-atlas
- **数据**: Google Cloud Storage (gs://arc-ctc-tahoe100/)

## 相关论文

1. "Virtual Cell Challenge: Toward a Turing test for the virtual cell" (Cell, 2025)
2. "Predicting cellular responses to perturbation across diverse contexts with State" (bioRxiv)

## 应用案例

- 预测干细胞对药物的反应
- 预测癌细胞对化疗的响应
- 预测免疫细胞对细胞因子的反应

## 优势与局限

### 优势
- 大规模数据训练，泛化能力强
- 零样本预测能力
- 开源，社区支持

### 局限
- 计算资源需求高
- 主要基于转录组数据

---

*文档创建时间: 2026-03-28*

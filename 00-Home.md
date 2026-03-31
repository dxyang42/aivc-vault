---
tags: [MOC, index, aivc, vault, home, navigation]
date: 2026-03-31
---

# 🧬 AIVC Perturbation Vault

> **AI Virtual Cell Perturbation Prediction Knowledge Base**

最后更新: 2026-03-31 | [GitHub入口](README.md)

---

## 📊 Vault 概览

| 类别 | 数量 | 描述 |
|------|------|------|
| 🔬 **方法文档** | 102 | Perturbation 预测方法 |
| 💡 **概念文档** | 29 | 核心概念与技术术语 |
| 👥 **团队文档** | 11 | 全球研究团队 |
| 📦 **资源文档** | 14 | 数据集与代码 |
| **总计** | **156+** | 全部 Markdown 文档 |

---

## 🗺️ 核心导航

### 地图中心 (MOC)

![[01-Maps/method-map]]
![[01-Maps/concept-map]]

| 地图 | 描述 |
|------|------|
| 📋 [[01-Maps/complete-index\|complete-index]] | 所有文档完整索引 |
| 🔬 [[01-Maps/method-map\|method-map]] | 102个方法全景图 |
| 👥 [[01-Maps/team-map\|team-map]] | 11个研究团队 |
| 💡 [[01-Maps/concept-map\|concept-map]] | 29个核心概念 |
| 📅 [[01-Maps/timeline\|timeline]] | 2018-2026发展历程 |

### 快速决策

| 你的需求 | 推荐文档 |
|----------|----------|
| 不知道用哪个方法 | [[04-Concepts/method-selection-guide\|method-selection-guide]] |
| 比较多个方法 | [[04-Concepts/method-comparison-matrix\|method-comparison-matrix]] |
| 理解技术关联 | [[04-Concepts/method-relationship-graph\|method-relationship-graph]] |
| 查找术语定义 | [[04-Concepts/terminology-standard\|terminology-standard]] |

---

## 🔬 方法分类速览

### 基础模型系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/STACK\|STACK]] | 2025 | Arc Institute, 基因组基础模型 |
| [[02-Methods/STATE\|STATE]] | 2025 | 1.7亿细胞预训练 |
| [[02-Methods/CellFM\|CellFM]] | 2025 | 8亿参数RetNet架构 |
| [[02-Methods/scKGBERT\|scKGBERT]] | 2024 | 知识增强BERT |
| [[02-Methods/GeneCompass\|GeneCompass]] | 2024 | 跨物种基因表达 |

### 扰动预测专门方法
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/scGen\|scGen]] | 2019 | VAE扰动预测奠基 |
| [[02-Methods/CPA\|CPA]] | 2023 | 组合扰动+剂量响应 |
| [[02-Methods/GEARS\|GEARS]] | 2023 | GNN+知识图谱 |
| [[02-Methods/scREPA\|scREPA]] | 2024 | 表示学习框架 |
| [[02-Methods/CellCap\|CellCap]] | 2024 | 细胞响应预测 |

### 因果推断方法
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/CausCell\|CausCell]] | 2024 | 同济大学, 因果VAE |
| [[02-Methods/scCausalVI\|scCausalVI]] | 2025 | 因果推断+do-算子 |
| [[02-Methods/CINEMA-OT\|CINEMA-OT]] | 2023 | 最优传输因果分析 |

### 组合扰动方法
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/GEARS\|GEARS]] | 2023 | 基因组合效应预测 |
| [[02-Methods/PDGrapher\|PDGrapher]] | 2024 | 扰动图神经网络 |
| [[02-Methods/GPerturb\|GPerturb]] | 2024 | 高斯过程扰动建模 |

### 零样本预测
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/PertAdapt\|PertAdapt]] | 2024 | 自适应扰动预测 |
| [[02-Methods/scDCA\|scDCA]] | 2024 | 深度聚类对齐 |
| [[02-Methods/PertEval-scFM\|PertEval-scFM]] | 2024 | 基础模型评估框架 |

---

## 💡 核心概念

![[01-Maps/concept-map]]

### 基础概念
- [[04-Concepts/Perturbation-Prediction\|Perturbation-Prediction]] - 扰动预测任务定义
- [[04-Concepts/Gene-Expression\|Gene-Expression]] - 基因表达基础
- [[04-Concepts/scRNA-seq\|scRNA-seq]] - 单细胞测序技术
- [[04-Concepts/Single-Cell-Analysis\|Single-Cell-Analysis]] - 单细胞分析方法

### 技术概念
- [[04-Concepts/Variational-Autoencoder\|Variational-Autoencoder]] - VAE原理
- [[04-Concepts/Attention-Mechanism\|Attention-Mechanism]] - 注意力机制
- [[04-Concepts/Causal-Inference\|Causal-Inference]] - 因果推断
- [[04-Concepts/Optimal-Transport\|Optimal-Transport]] - 最优传输

### 评估与基准
- [[04-Concepts/Benchmark\|Benchmark]] - 评估基准
- [[04-Concepts/Perturbation-Metrics\|Perturbation-Metrics]] - 评估指标
- [[04-Concepts/method-selection-guide\|method-selection-guide]] - 方法选择指南

---

## 📦 资源与数据

### 基准数据集
| 资源 | 描述 |
|------|------|
| [[05-Resources/scPerturb\|scPerturb]] | 标准化扰动数据库 (400万+细胞) |
| [[05-Resources/Norman-2021\|Norman-2021]] | 组合扰动基准 |
| [[05-Resources/Replogle-2022\|Replogle-2022]] | K562 + RPE1大规模筛选 |
| [[05-Resources/sci-Plex\|sci-Plex]] | 化合物扰动筛选 |
| [[05-Resources/Tahoe-100M\|Tahoe-100M]] | 1亿细胞参考图谱 |

### 工具与代码
- [[05-Resources/code-implementations\|code-implementations]] - 各方法代码链接
- [[05-Resources/Perturbation-Benchmark\|Perturbation-Benchmark]] - 评估框架
- [[05-Resources/Perturbation-Analysis-Tools\|Perturbation-Analysis-Tools]] - 分析工具

---

## 🔍 查询示例

### 按技术类型查找
```
tag:#VAE OR tag:#GNN
```

### 按年份查找
```
[[02-Methods]] AND 2024
```

### 按应用场景查找
```
tag:#combinatorial-perturbation
```

### 查找相关概念
```
[[04-Concepts/Perturbation-Prediction]]
```

---

## 📅 最近更新

### 2026-03-31 (Vault重构完成)
- **方法文档**: 102个完整方法
- **概念文档**: 29个核心概念
- **资源文档**: 14个数据集与工具
- **质量提升**: 全部公式清理，ASCII结构图

### 2026-03-30 (Benchmark扩充)
- 新增17个方法文档
- 新增5个资源文档
- 新增4个概念文档

### 2026-03-29 (方法扩充)
- 新增基础模型: UCE, X-Cell, X-Pert
- 新增因果方法: CausalBERT, scCausalVI
- 新增可视化: Perturb-Viz

---

## 🎯 使用场景

### 场景1: 快速了解领域
1. 阅读 [[01-Maps/timeline\|timeline]] 了解发展历程
2. 浏览 [[01-Maps/method-map\|method-map]] 查看方法全景
3. 查看 [[04-Concepts/method-selection-guide\|method-selection-guide]] 选择方法

### 场景2: 深入研究特定方法
1. 在 [[01-Maps/complete-index\|complete-index]] 中找到方法
2. 阅读方法文档了解细节
3. 查看 [[04-Concepts/method-relationship-graph\|method-relationship-graph]] 理解关联

### 场景3: 比较多个方法
1. 使用 [[04-Concepts/method-comparison-matrix\|method-comparison-matrix]] 横向对比
2. 查看各方法的优势与局限章节
3. 参考应用场景推荐

---

## 🤝 贡献

本知识库由社区贡献维护。
- 添加新方法: 参考 [[05-Resources/templates/方法模板\|方法模板]]
- 添加新概念: 参考 [[05-Resources/templates/概念模板\|概念模板]]
- 提交Issue或PR到 [GitHub](README.md)

---

*Vault版本: 2026-03-31 | 方法: 102 | 概念: 29*

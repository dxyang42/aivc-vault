---
tags: [MOC, index, aivc, vault, home]
date: 2026-03-29
version: V7
---

# 🧬 AIVC Perturbation 预测知识库

> **AI Virtual Cell Perturbation Prediction Knowledge Base**
> 
> 版本: **V7 (最终版)** | 最后更新: 2026-03-29 | 文档数: 84

---

## 📊 Vault 概览

| 类别 | 数量 | 描述 |
|------|------|------|
| 🔬 **方法文档** | 28 | Perturbation 预测方法 |
| 👥 **团队文档** | 11 | 全球研究团队 |
| 💡 **概念文档** | 9 | 核心概念与技术术语 |
| 📦 **资源文档** | 8 | 数据集与代码 |
| 📝 **日常笔记** | 5 | 迭代记录与待办 |
| **总计** | **68** | 全部 Markdown 文档 |

---

## 🗺️ 核心导航

### 地图 (MOC)

| 地图 | 描述 | 链接 |
|------|------|------|
| 📋 **完整索引** | 所有文档列表 | [[01-Maps/完整索引\|完整索引]] |
| 🔬 **方法地图** | 28个方法全景图 | [[01-Maps/方法地图\|方法地图]] |
| 👥 **团队地图** | 11个研究团队 | [[01-Maps/团队地图\|团队地图]] |
| 💡 **概念地图** | 核心概念词典 | [[01-Maps/概念地图\|概念地图]] |
| 📅 **时间线** | 2018-2026发展历程 | [[01-Maps/时间线\|时间线]] |

### 快速决策

| 你的需求 | 推荐文档 |
|----------|----------|
| 不知道用哪个方法 | [[04-Concepts/方法选择指南\|方法选择指南]] |
| 比较多个方法 | [[04-Concepts/方法对比矩阵\|方法对比矩阵]] |
| 理解技术关联 | [[04-Concepts/方法关联图谱\|方法关联图谱]] |
| 查找术语定义 | [[04-Concepts/术语统一规范\|术语统一规范]] |

---

## 🔬 方法分类

### 按技术架构

#### VAE 系列 (scVI生态)
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/scVI\|scVI]] | 2018 | VAE奠基 |
| [[02-Methods/scGen\|scGen]] | 2019 | 首个扰动预测 |
| [[02-Methods/scANVI\|scANVI]] | 2019 | 半监督注释 |
| [[02-Methods/totalVI\|totalVI]] | 2020 | RNA+Protein |
| [[02-Methods/MultiVI\|MultiVI]] | 2021 | 多组学整合 |
| [[02-Methods/CPA\|CPA]] | 2023 | 组合扰动 |

#### Transformer 系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/STATE\|STATE]] | 2025 | 1.7亿细胞 |
| [[02-Methods/scLAMBDA\|scLAMBDA]] | 2024 | LLM入场 |
| [[02-Methods/scBERT\|scBERT]] | 2023 | 基因BERT |
| [[02-Methods/scGPT-spatial\|scGPT-spatial]] | 2024 | 空间转录组 |

#### GNN 系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/GEARS\|GEARS]] | 2023 | GNN+知识图谱 |
| [[02-Methods/Cell_Oracle\|Cell Oracle]] | 2023 | GRN建模 |

#### 流匹配系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/CellFlow\|CellFlow]] | 2025 | 流匹配 |
| [[02-Methods/AlphaCell\|AlphaCell]] | 2026 | 世界模型 |
| [[02-Methods/CFM-GP\|CFM-GP]] | 2026 | 条件流匹配 |
| [[02-Methods/CellFlux\|CellFlux]] | 2025 | 基于流 |

#### 语言模型系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/CellHermes\|CellHermes]] | 2026 | 细胞语言模型 |
| [[02-Methods/scLAMBDA\|scLAMBDA]] | 2024 | LLM |
| [[02-Methods/GenePT\|GenePT]] | 2023 | 基因嵌入 |

#### 扩散模型系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/scDFM\|scDFM]] | 2025 | 单细胞扩散 |
| [[02-Methods/scPPDM\|scPPDM]] | 2025 | 扰动扩散 |
| [[02-Methods/SCALE\|SCALE]] | 2025 | 大规模扩散 |

#### 其他
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/scArches\|scArches]] | 2021 | 迁移学习 |
| [[02-Methods/UCE\|UCE]] | 2023 | 通用嵌入 |
| [[02-Methods/scFoundation\|scFoundation]] | 2024 | 基础模型 |
| [[02-Methods/Tahoe-x1\|Tahoe-x1]] | 2025 | Arc基础模型 |
| [[02-Methods/LPM\|LPM]] | 2025 | 语言扰动 |
| [[02-Methods/Scouter\|Scouter]] | 2022 | 统计框架 |

### 按年份

- **2018**: [[02-Methods/scVI\|scVI]]
- **2019**: [[02-Methods/scGen\|scGen]], [[02-Methods/scANVI\|scANVI]]
- **2020**: [[02-Methods/totalVI\|totalVI]]
- **2021**: [[02-Methods/MultiVI\|MultiVI]], [[02-Methods/scArches\|scArches]]
- **2022**: [[02-Methods/Scouter\|Scouter]]
- **2023**: [[02-Methods/GEARS\|GEARS]], [[02-Methods/CPA\|CPA]], [[02-Methods/Cell_Oracle\|Cell Oracle]], [[02-Methods/scBERT\|scBERT]], [[02-Methods/UCE\|UCE]], [[02-Methods/GenePT\|GenePT]]
- **2024**: [[02-Methods/scLAMBDA\|scLAMBDA]], [[02-Methods/scGPT-spatial\|scGPT-spatial]], [[02-Methods/scFoundation\|scFoundation]]
- **2025**: [[02-Methods/STATE\|STATE]], [[02-Methods/CellFlow\|CellFlow]], [[02-Methods/scDFM\|scDFM]], [[02-Methods/scPPDM\|scPPDM]], [[02-Methods/SCALE\|SCALE]], [[02-Methods/CellFlux\|CellFlux]], [[02-Methods/Tahoe-x1\|Tahoe-x1]], [[02-Methods/LPM\|LPM]]
- **2026**: [[02-Methods/AlphaCell\|AlphaCell]], [[02-Methods/CellHermes\|CellHermes]], [[02-Methods/CFM-GP\|CFM-GP]]

---

## 👥 顶级团队

| 团队 | 机构 | 国家 | 代表方法 |
|------|------|------|----------|
| [[03-Teams/Arc_Institute\|Arc Institute]] | UC Berkeley/Stanford | 🇺🇸 | STATE, Tahoe-x1 |
| [[03-Teams/Fabian_Theis_Lab\|Fabian Theis Lab]] | Helmholtz/TUM | 🇩🇪 | scGen, CPA, CellFlow |
| [[03-Teams/MIT-Weissman-Lab\|MIT Weissman Lab]] | MIT | 🇺🇸 | 组合扰动 |
| [[03-Teams/Samantha_Morris_Lab\|Samantha Morris Lab]] | WashU | 🇺🇸 | Cell Oracle |
| [[03-Teams/Tongji_DELTA_Lab\|同济 DELTA Lab]] | 同济大学 | 🇨🇳 | AlphaCell, CellHermes |
| [[03-Teams/Tencent-AI-Lab\|腾讯 AI Lab]] | 腾讯 | 🇨🇳 | scBERT, scGPT |
| [[03-Teams/Toronto-BoWang-Lab\|Toronto Bo Wang Lab]] | 多伦多大学 | 🇨🇦 | CFM-GP |
| [[03-Teams/Broad-Institute\|Broad Institute]] | Broad | 🇺🇸 | Perturb-seq |
| [[03-Teams/Recursion-Pharma\|Recursion Pharma]] | Recursion | 🇺🇸 | 工业界应用 |
| [[03-Teams/Jure_Leskovec_Lab\|Jure Leskovec Lab]] | Stanford | 🇺🇸 | GEARS |
| [[03-Teams/Westlake_Guo_Tiannan_Lab\|西湖大学 郭天南 Lab]] | 西湖大学 | 🇨🇳 | 蛋白质组+AI |

---

## 💡 核心概念

### 基础概念
- [[04-Concepts/术语统一规范\|术语统一规范]] - 完整术语对照表
- [[04-Concepts/方法选择指南\|方法选择指南]] - 场景化推荐
- [[04-Concepts/方法对比矩阵\|方法对比矩阵]] - 多维度对比
- [[04-Concepts/方法关联图谱\|方法关联图谱]] - 技术演进可视化

### 技术专题
- [[04-Concepts/VAE-in-Single-Cell\|VAE in Single-Cell]] - 变分自编码器
- [[04-Concepts/Transformer-in-Single-Cell\|Transformer in Single-Cell]] - 注意力机制
- [[04-Concepts/GNN-in-Gene-Networks\|GNN in Gene Networks]] - 图神经网络
- [[04-Concepts/Diffusion-Models\|Diffusion Models]] - 扩散模型
- [[04-Concepts/Flow-Matching\|Flow Matching]] - 流匹配

---

## 📦 资源

### 数据集
- [[05-Resources/数据集资源\|数据集资源]] - 数据集汇总
- [[05-Resources/Norman-2021\|Norman 2021]] - 组合扰动基准
- [[05-Resources/Replogle-2022\|Replogle 2022]] - K562 + RPE1
- [[05-Resources/sci-Plex\|sci-Plex]] - 化合物扰动
- [[05-Resources/Tahoe-100M\|Tahoe-100M]] - 1亿细胞图谱

### 代码
- [[05-Resources/代码实现\|代码实现]] - 代码资源汇总

### 模板
- [[05-Resources/templates/方法模板\|方法模板]] - 新建方法文档
- [[05-Resources/templates/团队模板\|团队模板]] - 新建团队文档

---

## 📝 迭代记录

| 迭代 | 日期 | 主题 | 关键产出 |
|------|------|------|----------|
| V7 | 2026-03-29 | 最终完善 | 84文档, 完整索引, 最终报告 |
| V6 | 2026-03-29 | 批判审查 | 数学公式补充, 术语规范, 关联图谱 |
| V5 | 2026-03-28 | 结构优化 | scVI生态, 方法选择指南 |
| V4 | 2026-03-28 | Obsidian迁移 | PARA+Zettelkasten结构 |
| V3 | 2026-03-27 | 大规模扩充 | 30+方法 |
| V2 | 2026-03-26 | Perturbation基础 | 8个方法 |
| V1 | 2026-03-25 | 基础模型 | 5个方法 |

详细记录:
- [[06-Daily/V7-完成报告\|V7 完成报告]]
- [[06-Daily/iter6-总结\|第6轮迭代总结]]
- [[06-Daily/iter6-批判报告\|第6轮批判报告]]

---

## 🔍 搜索提示

### 按技术搜索
- VAE: `tag:#vae`
- Transformer: `tag:#transformer`
- GNN: `tag:#gnn`
- 流匹配: `tag:#flow-matching`
- 扩散模型: `tag:#diffusion`

### 按年份搜索
- 2026: `year:2026`
- 2025: `year:2025`

### 按机构搜索
- 同济: `tag:#tongji`
- Arc: `tag:#arc`
- Stanford: `tag:#stanford`

---

## 🎯 使用场景

### 场景1: 快速了解领域
1. 阅读 [[01-Maps/时间线]]
2. 查看 [[04-Concepts/方法关联图谱]]
3. 浏览 [[01-Maps/方法地图]]

### 场景2: 选择合适方法
1. 查看 [[04-Concepts/方法选择指南]]
2. 对比 [[04-Concepts/方法对比矩阵]]
3. 深入阅读具体方法文档

### 场景3: 学术研究
1. 查找相关 [[03-Teams/]] 团队
2. 了解 [[05-Resources/]] 数据集
3. 参考 [[04-Concepts/术语统一规范]]

### 场景4: 添加新方法
1. 复制 [[05-Resources/templates/方法模板]]
2. 填写方法信息
3. 更新 [[01-Maps/方法地图]]

---

## 📞 反馈与贡献

如发现错误或需要补充，请:
1. 在相应文档添加注释
2. 更新相关链接
3. 记录变更到 [[06-Daily/]]

---

*Vault 结构: PARA + Zettelkasten*
*创建: 2026-03-28*
*最后更新: 2026-03-29*
*版本: V7 (最终版)*

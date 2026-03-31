---
tags: [MOC, index, aivc, vault, home]
date: 2026-03-29
---

# 🧬 AIVC Perturbation 预测知识库

> **AI Virtual Cell Perturbation Prediction Knowledge Base**
> 
> 最后更新: 2026-03-31 (最终整合 - 117方法, 29概念)

---

## 📊 Vault 概览

| 类别 | 数量 | 描述 |
|------|------|------|
| 🔬 **方法文档** | 117 | Perturbation 预测方法 |
| 👥 **团队文档** | 11 | 全球研究团队 |
| 💡 **概念文档** | 29 | 核心概念与技术术语 |
| 📦 **资源文档** | ~12 | 数据集与代码 |
| **总计** | **~170** | 全部 Markdown 文档 |

---

## 🗺️ 核心导航

### 地图 (MOC)

| 地图 | 描述 | 链接 |
|------|------|------|
| 📋 **完整索引** | 所有文档列表 | [[01-Maps/complete-index\|complete-index]] |
| 🔬 **方法地图** | 117个方法全景图 | [[01-Maps/method-map\|method-map]] |
| 👥 **团队地图** | 11个研究团队 | [[01-Maps/team-map\|team-map]] |
| 💡 **概念地图** | 29个核心概念 | [[01-Maps/concept-map\|concept-map]] |
| 📅 **时间线** | 2018-2026发展历程 | [[01-Maps/timeline\|timeline]] |

### 快速决策

| 你的需求 | 推荐文档 |
|----------|----------|
| 不知道用哪个方法 | [[04-Concepts/method-selection-guide\|method-selection-guide]] |
| 比较多个方法 | [[04-Concepts/method-comparison-matrix\|method-comparison-matrix]] |
| 理解技术关联 | [[04-Concepts/method-relationship-graph\|method-relationship-graph]] |
| 查找术语定义 | [[04-Concepts/terminology-standard\|terminology-standard]] |

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
| [[02-Methods/X-Cell\|X-Cell]] | 2026 | 49亿参数扩散模型 |
| [[02-Methods/X-Pert\|X-Pert]] | 2025 | 统一扰动建模 |

#### GNN 系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/GEARS\|GEARS]] | 2023 | GNN+知识图谱 |
| [[02-Methods/Cell_Oracle\|Cell Oracle]] | 2023 | GRN建模 |
| [[02-Methods/Celcomen\|Celcomen]] | 2025 | 空间因果解耦 |

#### 基础模型系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/UCE\|UCE]] | 2024 | 跨物种通用细胞嵌入 |
| [[02-Methods/CellFM\|CellFM]] | 2025 | 8亿参数RetNet架构 |
| [[02-Methods/scMulan\|scMulan]] | 2024 | 多任务生成式语言模型 |

#### 流匹配系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/CellFlow\|CellFlow]] | 2024 | 流匹配最优传输 |
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
| [[02-Methods/DC-DSB\|DC-DSB]] | 2025 | 方向约束薛定谔桥 |

#### 因果推断系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/scCausalVI\|scCausalVI]] | 2025 | 因果VAE + do-算子 |
| [[02-Methods/CASCADE\|CASCADE]] | 2024 | 因果发现 |
| [[02-Methods/CausalBERT\|CausalBERT]] | 2025 | 预训练因果语言模型 |

#### 网络干扰系列
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/NetPerturb\|NetPerturb]] | 2025 | 深度学习网络流 |
| [[02-Methods/Cell_Oracle\|Cell Oracle]] | 2023 | GRN建模 |

#### 疾病建模与临床应用
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/Perturb-Cancer\|Perturb-Cancer]] | 2024 | 患者来源癌症模型 |
| [[02-Methods/DrugCell\|DrugCell]] | 2020 | 药物响应深度学习 |

#### 可视化工具
| 方法 | 年份 | 特点 |
|------|------|------|
| [[02-Methods/Perturb-Viz\|Perturb-Viz]] | 2024 | 交互式可视化平台 |

---

## 📚 核心资源

### 数据集
- [[05-Resources/Norman-2021\|Norman 2021]] - 组合扰动基准
- [[05-Resources/Replogle-2022\|Replogle 2022]] - K562 + RPE1
- [[05-Resources/sci-Plex\|sci-Plex]] - 化合物扰动
- [[05-Resources/Tahoe-100M\|Tahoe-100M]] - 1亿细胞图谱

### 代码实现
- [[05-Resources/code-implementations\|code-implementations]] - 各方法代码链接

### 文档模板
- [[05-Resources/templates/方法模板\|方法模板]] - 添加新方法
- [[05-Resources/templates/团队模板\|团队模板]] - 添加新团队

---

## 🎯 使用场景

### 场景 1: 快速了解领域
1. 阅读 [[01-Maps/timeline\|timeline]] 了解发展历程
2. 浏览 [[01-Maps/method-map\|method-map]] 查看方法全景
3. 查看 [[04-Concepts/method-selection-guide\|method-selection-guide]] 选择合适方法

### 场景 2: 深入研究特定方法
1. 在 [[01-Maps/complete-index\|complete-index]] 中找到方法
2. 阅读方法文档了解细节
3. 查看 [[04-Concepts/method-relationship-graph\|method-relationship-graph]] 理解技术关联

### 场景 3: 比较多个方法
1. 使用 [[04-Concepts/method-comparison-matrix\|method-comparison-matrix]] 横向对比
2. 查看各方法的优势与局限章节
3. 参考应用场景推荐

---

## 📦 数据与资源

### Benchmark 与数据集
| 资源 | 描述 | 链接 |
|------|------|------|
| **scPerturb** | 标准化扰动数据库 (400万+细胞) | [[05-Resources/scPerturb\|scPerturb]] |
| **Perturbation-Benchmark** | 评估框架与指标 | [[05-Resources/Perturbation-Benchmark\|评估框架]] |
| **GEO-Perturbation** | NCBI GEO数据集汇总 | [[05-Resources/GEO-Perturbation\|GEO数据集]] |
| **Drug-Screening** | 药物筛选Benchmark | [[05-Resources/Drug-Screening-Benchmarks\|药物筛选]] |
| **Synthetic-Data** | 合成数据生成器 | [[05-Resources/Synthetic-Perturbation-Data\|合成数据]] |

### 经典数据集
- [[05-Resources/Norman-2021\|Norman 2021]] - 组合扰动基准
- [[05-Resources/Replogle-2022\|Replogle 2022]] - 大规模Perturb-seq
- [[05-Resources/sci-Plex\|sci-Plex]] - 化合物筛选
- [[05-Resources/Tahoe-100M\|Tahoe-100M]] - 亿级扰动图谱

---

## 📖 最近更新

### 2026-03-30 (第11轮更新 - Benchmark & Review扩充)
- **新增 17 个方法文档** (02-Methods/)
  - GPerturb, CellCap, scPRAM, PerturbNet, PSD, Tahoe-x1
  - sc_Perturbation_Transformer, CellFlow-Plus, scCausalGP
  - MultiPerturb, PerturbVAE-Pro, scDiffusion-Perturb
  - PerturbGNN, TemporalPerturb, ContrastivePerturb
  - EnsemblePerturb, scPerturb-Benchmark
- **新增 5 个资源文档** (05-Resources/)
  - scPerturb, Perturbation-Benchmark, GEO-Perturbation
  - Drug-Screening-Benchmarks, Synthetic-Perturbation-Data
- **新增 4 个概念文档** (04-Concepts/)
  - Attention-Mechanism, Causal-Inference, Generative-Models, Benchmark
- **Vault规模**: ~89方法, ~27概念, ~12资源

### 2026-03-29 (第10轮更新)
- 新增 1 个方法文档
  - [[02-Methods/PRESCIENT|PRESCIENT]] - 生成式细胞命运预测 (Cell Systems 2020)

### 2026-03-29 (第9轮更新)
- 新增 3 个方法文档
  - [[02-Methods/UCE|UCE]] - 跨物种通用细胞嵌入 (Nature 2024)
  - [[02-Methods/Perturb-Cancer|Perturb-Cancer]] - 患者来源癌症模型 (Nature Cancer 2024)
  - [[02-Methods/CellFlow|CellFlow]] - 流匹配最优传输 (Nature Methods 2024)

### 2026-03-29 (第7轮更新)
- 新增 3 个方法文档
  - [[02-Methods/CausalBERT\|CausalBERT]] - 预训练因果语言模型
  - [[02-Methods/NetPerturb\|NetPerturb]] - 深度学习网络流预测
  - [[02-Methods/Perturb-Viz\|Perturb-Viz]] - 扰动效应可视化平台

### 2026-03-29
- 新增 5 个方法文档
  - [[02-Methods/X-Cell\|X-Cell]] - 49亿参数扩散语言模型
  - [[02-Methods/X-Pert\|X-Pert]] - 统一扰动建模框架
  - [[02-Methods/DC-DSB\|DC-DSB]] - 方向约束扩散薛定谔桥
  - [[02-Methods/Celcomen\|Celcomen]] - 空间因果解耦
  - [[02-Methods/scGPT-spatial\|scGPT-spatial]] - 空间感知基础模型

---

## 🙏 贡献

本知识库由社区贡献维护。欢迎提交 Issue 或 PR 补充新方法。

---

*最后更新: 2026-03-31*

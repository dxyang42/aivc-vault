# Perturbation 预测领域主要研究团队及最新工作 (2025-2026)

## 1. Arc Institute (美国)

### 团队信息
- **机构**: Arc Institute (非营利研究机构)
- **地点**: 美国加州
- **核心成员**: 
  - Patrick Hsu (联合创始人)
  - Yusuf Roohani (虚拟细胞模型负责人)
  - Paul Datlinger (基因组工程副主任)
  - Emma Lundberg
  - Theofanis Karaletsos

### 2025-2026年新方法与关键论文

#### STATE 模型 (2025年6月发布)
- **论文**: "Virtual Cell Challenge: Toward a Turing test for the virtual cell" (Cell, 2025)
- **预印本**: "Predicting cellular responses to perturbation across diverse contexts with State"
- **核心创新**:
  - 基于Transformer架构的虚拟细胞模型
  - 训练数据: 1.7亿个观测细胞 + 1亿个扰动细胞
  - 覆盖70种人类细胞系
  - 可预测化学、遗传和细胞因子扰动的转录组响应
  - 在Tahoe-100M基准测试中，PDS指标提升50%，DES指标提升100%

#### Tahoe-100M 数据集 (2025年2月发布)
- **论文**: "Tahoe-100M: A giga-scale single-cell perturbation atlas for context-dependent gene function and cellular modeling" (bioRxiv, 2025)
- **规模**: 1亿个单细胞转录组，覆盖50种癌细胞系，1,100种小分子化合物，60,000种药物-细胞相互作用
- **合作方**: Vevo Therapeutics (现Tahoe Therapeutics), Parse Biosciences, Ultima Genomics

#### Virtual Cell Challenge (虚拟细胞挑战赛)
- **启动时间**: 2025年6月
- **目标**: 建立标准化的虚拟细胞模型评测基准
- **奖金**: $175,000
- **评测框架**: 提供专门构建的数据集和评估指标

### 开源项目
- **STATE模型**: https://github.com/ArcInstitute (开源，非商业用途)
- **Arc Virtual Cell Atlas**: https://arcinstitute.org/virtual-cell-atlas
- **Tahoe-100M数据**: Google Cloud Storage (gs://arc-ctc-tahoe100/)

---

## 2. Stanford University - Jure Leskovec 团队 (美国)

### 团队信息
- **机构**: 斯坦福大学计算机科学系
- **核心成员**:
  - Jure Leskovec (教授，图神经网络先驱)
  - Yusuf Roohani (博士生)
  - Kexin Huang
  - Stephen Quake (合作者)

### 2025-2026年新方法与关键论文

#### GEARS (Graph-Enhanced Gene Activation and Repression Simulator)
- **论文**: "Predicting transcriptional outcomes of novel multigene perturbations with GEARS" (Nature Biotechnology, 2023; 2024年正式发表)
- **预印本**: 2022年7月发布于bioRxiv
- **核心创新**:
  - 结合图神经网络与基因-基因关系知识图谱
  - 可预测单基因和多基因扰动的转录结果
  - 能够泛化到未见过的基因组合
  - 利用几何深度学习捕捉基因相互作用
- **GitHub**: https://github.com/snap-stanford/GEARS

### 研究方向
- 图神经网络在生物数据中的应用
- 多基因扰动效应预测
- 基因相互作用网络推断

---

## 3. Helmholtz Munich - Fabian Theis 团队 (德国)

### 团队信息
- **机构**: Helmholtz Munich (亥姆霍兹慕尼黑中心)
- **职位**: 计算生物学研究所所长
- **学术兼职**: 慕尼黑工业大学(TUM)数学建模生物系统讲席教授
- **核心成员**:
  - Fabian J. Theis (主任)
  - Mohammad Lotfollahi
  - F. Alexander Wolf
  - Lukas Heumos
  - Yuge Ji

### 2025-2026年新方法与关键论文

#### scGen (2019年发表，持续更新)
- **论文**: "scGen predicts single-cell perturbation responses" (Nature Methods, 2019)
- **核心创新**:
  - 基于变分自编码器(VAE)的扰动响应预测
  - 可预测未见细胞类型的扰动效应
  - 支持跨物种预测
- **GitHub**: https://github.com/theislab/scgen

#### CPA (Compositional Perturbation Autoencoder)
- **论文**: "Predicting cellular responses to complex perturbations in high-throughput screens" (Molecular Systems Biology, 2023)
- **核心创新**: 解耦扰动、细胞和剂量效应

#### Pertpy (2025年)
- **论文**: "Pertpy: an end-to-end framework for perturbation analysis" (Nature Methods, 2025)
- **核心创新**: 一站式单细胞扰动数据分析框架

#### 合作研究
- 参与Arc Institute Virtual Cell Challenge科学顾问委员会
- 与同济大学刘琦教授团队合作CellHermes项目

### 开源项目
- **scGen**: https://github.com/theislab/scgen
- **scVI-tools**: https://github.com/scverse/scvi-tools
- **Pertpy**: https://github.com/theislab/pertpy

---

## 4. Washington University - Samantha Morris 团队 (美国)

### 团队信息
- **机构**: 华盛顿大学圣路易斯分校
- **实验室**: Morris Lab
- **核心成员**:
  - Samantha A. Morris (PI)
  - Kenji Kamimoto (博士后，CellOracle主要开发者)

### 2025-2026年新方法与关键论文

#### CellOracle (2023年发表于Nature)
- **论文**: "Dissecting cell identity via network inference and in silico gene perturbation" (Nature, 2023)
- **核心创新**:
  - 整合单细胞多组学数据推断基因调控网络(GRN)
  - 支持计算机模拟转录因子(TF)扰动
  - 可预测细胞身份转变
  - 应用于小鼠/人造血和斑马鱼胚胎发育
- **GitHub**: https://github.com/morris-lab/CellOracle

### 开源项目
- **CellOracle**: https://github.com/morris-lab/CellOracle
- **文档**: https://morris-lab.github.io/CellOracle.documentation/

---

## 5. Broad Institute (美国)

### 团队信息
- **机构**: Broad Institute of MIT and Harvard
- **相关平台**: Genetic Perturbation Platform (GPP)
- **核心成员**:
  - John Doench (GPP副主任)
  - Ruth Hanna

### 2025-2026年新方法与关键论文

#### CMap (Connectivity Map)
- 全球最大的扰动驱动基因表达数据集
- 包含小分子化合物、基因敲低/敲除/过表达数据

#### PerturBase (中国合作)
- **论文**: "PerturBase: a comprehensive database for single-cell perturbation data analysis and visualization" (Nucleic Acids Research, 2025)
- **内容**: 整合122个数据集，覆盖24,254个遗传扰动和230个化学扰动

### 开源项目
- **CMap**: https://clue.io/

---

## 6. MIT - Weissman 团队相关 (美国)

### 团队信息
- **机构**: 麻省理工学院
- **相关研究**: 单细胞扰动测序技术

### 2025-2026年相关工作
- 参与单细胞扰动响应预测基准测试研究
- 相关论文: "Decoding heterogeneous single-cell perturbation responses" (Nature Cell Biology, 2025)
- 与Yale、Altos Labs等机构合作

---

## 7. UC Berkeley (美国)

### 团队信息
- **机构**: 加州大学伯克利分校
- **相关研究**: 与Arc Institute合作

### 2025-2026年新方法与关键论文

#### MORPH (2025年)
- **论文**: "MORPH Predicts the Single-Cell Outcome of Genetic Perturbations Across Conditions and Data Modalities" (bioRxiv, 2025)
- **核心创新**: 跨条件和数据模态预测遗传扰动的单细胞结果

#### Spatial Causal Disentanglement
- **论文**: "Estimation of single-cell and tissue perturbation effect in spatial transcriptomics via Spatial Causal Disentanglement" (ICLR 2025)
- **核心创新**:
  - 首个空间转录组学因果推断可识别模型
  - 整合解离单细胞数据和空间单细胞数据

---

## 8. Tsinghua University (清华大学) (中国)

### 团队信息
- **机构**: 清华大学
- **相关团队**: 自动化系、生命科学学院
- **核心成员**:
  - 刘琦 (教授，与Fabian Theis合作)
  - 其他计算生物学研究者

### 2025-2026年新方法与关键论文

#### scPerturBench (2025年)
- **论文**: "Benchmarking algorithms for generalizable single-cell perturbation response prediction" (submitted, 2025)
- **GitHub**: https://github.com/bm2-lab/scPerturBench
- **核心创新**:
  - 系统性基准测试27种单细胞扰动响应预测方法
  - 评估模型在未见细胞环境和未见扰动上的泛化能力
  - 使用29个数据集和多种评估指标

### 评测的方法列表
- scGen, CPA, GEARS, GenePert, linearModel
- scGPT, scFoundation, chemCPA
- biolord, CellOT, inVAE, scDisInFact
- scouter, scELMo, GeneCompass, cycleCDR, PRnet

---

## 9. Tongji University - DELTA Lab (同济大学) (中国)

### 团队信息
- **机构**: 同济大学
- **实验室**: 数字生命智能体实验室 (DELTA Lab)
- **核心成员**:
  - 刘琦 (教授，通讯作者)
  - 何斌 (教授)
  - 啜国晖 (助理教授)
  - 陈晓涵 (博士)
  - 杨兴博 (博士)
  - 高溢骋 (博士)
  - 汪伟旭 (博士，亥姆霍兹慕尼黑中心)
  - 董科竟 (博士)
  - Fabian J. Theis (合作者)

### 2025-2026年新方法与关键论文

#### AlphaCell (2026年3月发布)
- **核心创新**:
  - 构建"虚拟细胞世界模型"
  - 高保真细胞扰动预测
  - 世界模型(World Model)架构

#### CellHermes (2026年3月发布)
- **核心创新**:
  - 细胞语言模型
  - 跨模态异构数据融合
  - 基于LoRA的参数高效微调
  - 将单细胞转录组数据转化为"基因表达句子"
  - 将蛋白质互作网络转化为自然语言陈述

### 开源项目
- 即将开源 (请关注相关发布)

---

## 10. Westlake University - 郭天南团队 (西湖大学) (中国)

### 团队信息
- **机构**: 西湖大学医学院
- **实验室**: Guomics (蛋白质组复杂科学实验室)
- **核心成员**:
  - 郭天南 (教授，PI)
  - 孙瑞 (博士后)
  - 钱刘佳

### 2025-2026年新方法与关键论文

#### AI虚拟细胞"3+1"方案 (2025年3月)
- **论文**: "Grow AI Virtual Cells: Three Data Pillars and Closed-Loop Learning" (Cell Research, 2025)
- **核心创新**:
  - 提出三大数据支柱:
    1. 先验知识 (Prior Knowledge)
    2. 静态结构 (Static Structure)
    3. 动态状态 (Dynamic States)
  - 闭环主动学习系统 (Closed-Loop Active Learning)
  - 从酵母(S. cerevisiae)入手，逐步扩展到人类癌细胞系

#### 扰动蛋白质组学 (Perturbation Proteomics)
- **论文**: "AI-empowered perturbation proteomics for complex biological systems" (Cell Genomics, 2024)
- **核心创新**:
  - 整合16,000个扰动条件下的三阴性乳腺癌蛋白质组数据
  - 蛋白质组层面的扰动响应预测

#### 空间蛋白质组学
- **论文**: "Spatial proteomics of single cells and organelles on tissue slides using filter-aided expansion proteomics" (Nature Communications)

### 研究方向
- 蛋白质组学驱动的虚拟细胞建模
- 高通量质谱技术
- AI与机器人实验结合的闭环学习

### 网站
- https://guomics.com

---

## 附录: 重要数据集和基准测试资源

### 数据集
| 名称 | 机构 | 规模 | 发布年份 |
|------|------|------|----------|
| Tahoe-100M | Arc/Tahoe | 1亿细胞 | 2025 |
| scBaseCount | Arc | 2亿细胞 | 2025 |
| scPerturb | Sander Lab | 44个数据集 | 2024 |
| PerturBase | 中国 | 122个数据集 | 2025 |

### 基准测试框架
| 名称 | 机构 | 描述 |
|------|------|------|
| scPerturBench | Tsinghua | 27种方法系统性基准测试 |
| PerturBench | Altos Labs | 机器学习模型基准测试 |
| CausalBench | 多机构 | 基因调控网络推断基准 |
| Virtual Cell Challenge | Arc Institute | 虚拟细胞模型竞赛 |

### 主要开源工具
| 工具 | 团队 | 功能 |
|------|------|------|
| GEARS | Stanford | 多基因扰动预测 |
| scGen | Theis Lab | 单细胞扰动响应预测 |
| CellOracle | Morris Lab | GRN推断与模拟 |
| CPA | Theis Lab | 复合扰动预测 |
| Pertpy | Theis Lab | 扰动分析框架 |
| STATE | Arc Institute | 虚拟细胞模型 |

---

*报告生成时间: 2026年3月*
*数据来源: MetaSo MCP 深度搜索*

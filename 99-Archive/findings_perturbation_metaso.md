# Perturbation 效果预测相关论文和资料调研

> 使用 MetaSo MCP 搜索整理
> 调研日期: 2026-03-28
> 调研主题: AI Virtual Cell (AIVC) 中 Perturbation 预测

---

## 目录

1. [STATE (MIT Broad)](#1-state-mit-broad)
2. [STACK (Arc Institute)](#2-stack-arc-institute)
3. [VCWorld (Virtual Cell)](#3-vcworld-virtual-cell)
4. [GEARS (Stanford)](#4-gears-stanford)
5. [CPA (Chemical Perturbation Autoencoder)](#5-cpa-chemical-perturbation-autoencoder)
6. [scGen](#6-scgen)
7. [Cell Oracle](#7-cell-oracle)
8. [Perturb-seq](#8-perturb-seq)
9. [Gene Perturbation Prediction (Deep Learning)](#9-gene-perturbation-prediction-deep-learning)
10. [Combinatorial Genetic Perturbation](#10-combinatorial-genetic-perturbation)
11. [Single Cell Perturbation Benchmarking](#11-single-cell-perturbation-benchmarking)

---

## 1. STATE (MIT Broad)

### 概述
STATE是一种基于Transformer的虚拟细胞模型，用于预测细胞集合中对药物、细胞因子或基因扰动的反应。

### 关键信息
- **训练数据**: 基于1.67亿个细胞的观测数据和超过1亿个细胞的扰动数据训练，涵盖70种细胞系
- **架构**: 由State Embedding（SE）和State Transition（ST）两个模块组成
  - **SE模型**: 将转录组数据转化为平滑向量空间
  - **ST模型**: 预测细胞在干扰下的状态转换
- **性能**: 在预测新干扰反应方面表现优异，准确率是现有模型的两倍
- **技术特点**: 利用双向Transformer架构，结合自注意力机制，能捕捉生物和技术异质性
- **评估框架**: 推出Cell_Eval评估框架和虚拟细胞挑战赛

### 相关资源
- 论文发表于bioRxiv
- 研究团队通过实验生成扰动数据，弥补单细胞数据的不足，提升模型的因果推断能力

---

## 2. STACK (Arc Institute)

### 概述
Stack是Arc研究所推出的开源基础模型，通过表格注意力机制生成细胞表征，实现零样本场景下的性能提升。

### 关键信息
- **训练规模**: 在1.49亿个标准化人类单细胞数据上训练
- **核心技术**: 表格注意力机制（tabular attention）
- **架构创新**: 将单细胞数据转化为二维表格，捕捉基因间与细胞间关联
- **训练策略**: 
  - 基于scBaseCount数据库预训练
  - 通过CellxGene和Parse PBMC后训练
  - 学会将细胞作为提示指令预测其他细胞反应
- **性能**: 无需微调即可学习新任务，在扰动预测指标上全面超越现有方案

### Perturb Sapiens 图谱
- 基于Stack预测20000个"细胞类型-组织-扰动"组合
- 涵盖28个组织、40个细胞类别、201种扰动
- 揭示药物在不同细胞类型中的反应差异

### 开源资源
- Stack: https://github.com/ArcInstitute/stack
- Perturb Sapiens: https://huggingface.co/datasets/arcinstitute/Perturb-Sapiens

---

## 3. VCWorld (Virtual Cell)

### 概述
VCWorld是一种用于虚拟细胞模拟的生物世界模型，结合结构化生物知识和大语言模型的迭代推理能力。

### 关键信息
- **类型**: 细胞级白盒模拟器
- **核心特点**:
  - 结合结构化生物知识和大语言模型的迭代推理能力
  - 数据高效，能重现扰动引起的信号级联反应
  - 生成可解释的分步预测和明确的机制假设
- **优势**: 
  - 现有模型依赖大规模单细胞数据，泛化能力受限于数据质量和批次效应
  - VCWorld作为白盒模型，提供可解释性和与生物原理的一致性
- **性能**: 在药物扰动基准测试中表现出最先进的预测性能
- **验证**: 推断的机制路径与公开的生物证据一致

### 相关论文
- "VCWorld: A Biological World Model for Virtual Cell Simulation" (arXiv 2025)
- "VCWorld: A Cell-Level White-Box Simulator Integrating Biological Knowledge with Large Language Model Reasoning"

---

## 4. GEARS (Stanford)

### 概述
GEARS是一种结合深度学习与基因-基因关系知识图谱的计算方法，用于预测基因扰动的转录反应。

### 关键信息
- **开发团队**: 斯坦福大学 (Yusuf Roohani, Jure Leskovec, Kexin Huang等)
- **核心能力**:
  - 预测单基因或多基因扰动的结果
  - 即使这些基因或组合此前没有实验数据也能预测
  - 预测转录反应，帮助科学家在基因编辑后预测细胞的基因表达变化
- **技术方法**:
  - 利用图神经网络融合基因关系图和扰动关系图
  - 整合基因共表达网络和基因本体论知识
  - 捕捉基因间的相互作用，预测多基因扰动的非加性效应
  - 使用嵌入表示、多维组件、图神经网络等技术
- **性能**:
  - 在预测不同遗传交互类型时精度比现有方法高40%
  - 在识别强交互方面表现更优
  - 能预测从未实验扰动过的基因组合结果

### 应用场景
- 药物发现
- 癌症生物学
- 再生医学
- 个性化治疗方案

### 资源
- GitHub: https://github.com/snap-stanford/GEARS
- 论文发表于Nature Biotechnology 2023

---

## 5. CPA (Chemical Perturbation Autoencoder)

### 概述
CPA（Compositional Perturbation Autoencoder）是一种用于学习单细胞水平扰动效应的框架，能够编码并学习不同细胞类型、剂量和药物组合下的表型药物反应。

### 关键信息
- **开发团队**: Facebook AI与慕尼黑亥姆霍兹研究中心合作
- **核心功能**:
  - 对未见的药物和基因组合进行离分布预测
  - 学习可解释的药物和细胞类型潜在空间
  - 估计每个扰动及其组合的剂量-反应曲线
  - 将扰动效应从一种细胞类型转移到未见的细胞类型
  - 在潜在空间和基因表达空间中去除批次效应
- **技术方法**:
  - 利用RNA测序数据训练
  - 通过压缩和解压缩数据，提取细胞的关键属性
  - 重新组合这些属性以预测基因表达变化
  - 自监督学习技术
- **开源**: 提供API和Python包

### 应用
- 预测药物组合效果
- 预测剂量、时间等干预措施
- 加速药物组合的发现

---

## 6. scGen

### 概述
scGen是一种用于预测不同细胞类型、研究和物种中单细胞扰动响应的生成模型。

### 关键信息
- **发表**: Nature Methods 2019
- **框架**: 基于scvi-tools框架实现
- **技术基础**: 变分自编码器（VAE）
- **核心能力**:
  - 跨细胞类型预测扰动效应
  - 跨研究预测
  - 跨物种预测
  - 去除批次效应

### 工作原理
- 使用编码器网络将基因表达测量值投影到潜在空间
- 计算向量δ，表示受扰动细胞与未受扰动细胞在潜在空间中的差异
- 对未受扰动细胞在潜在空间中进行线性外推
- 通过解码器网络将潜在空间中的预测映射到基因表达空间

### 应用场景
- 训练于多细胞类型和条件的数据集，预测仅在一种条件下存在的细胞类型的扰动效应
- 多物种场景：用一种或所有物种预测特定物种的效应
- 在两个条件（如对照和扰动）的数据集上训练，在具有相似基因的第二个数据集上预测

### 资源
- GitHub: https://github.com/theislab/scgen
- 文档: https://scgen.readthedocs.io/

---

## 7. Cell Oracle

### 概述
CellOracle是一个Python库，用于使用单细胞组学数据和基因调控网络模型进行计算机基因扰动分析。

### 关键信息
- **开发团队**: 华盛顿大学Samantha Morris团队
- **核心功能**:
  - 从单细胞转录组和表观基因组数据推断基因调控网络（GRN）
  - 模拟阻碍或增强转录因子对细胞身份的影响
  - 分析转录因子对细胞身份的调节
- **应用案例**:
  - 小鼠和人类造血
  - 斑马鱼胚胎发生
  - 正确模拟由于转录因子扰动而发生的表型变化
  - 识别新的发育调节因子（如noto、lhx1a）
- **谱系重编程**: 揭示成功和失败命运转换的不同网络配置

### 支持的基因组版本
- Human: hg38, hg19
- Mouse: mm39, mm10, mm9

### 资源
- GitHub: https://github.com/morris-lab/CellOracle
- 论文: "Dissecting cell identity via network inference and in silico gene perturbation"

---

## 8. Perturb-seq

### 概述
Perturb-seq是一种实验方法，通过结合CRISPR基因筛选与信息丰富的单细胞RNA测序表型，绘制遗传扰动的转录效应。

### 关键信息
- **技术原理**: 结合CRISPR-based遗传筛选与单细胞RNA测序
- **应用规模**: 可在基因组规模上应用
- **功能**:
  - 系统预测基因功能
  - 深入研究复杂细胞表型（如非整倍体、分化、RNA加工、线粒体基因组应激特异性调控）
- **技术演进**:
  - **第一代**: CROP-seq等，依赖polyA
  - **第二代**: direct guide RNA capture，使用特定barcode
  - **第三代**: ClonMapper实现动态研究

### 重要研究
- **Genome-scale Perturb-seq**: 靶向所有表达基因，使用CRISPR干扰（CRISPRi），跨越超过250万个人类细胞
- **发现**: 发现新的核糖体生物合成调节因子（CCDC86、ZNF236、SPATA5L1）、转录调节因子（C7orf26）、线粒体呼吸调节因子（TMEM242）

### 相关工具
- **Mixscale**: 分析CRISPRi-based Perturb-seq数据的R包
- **PerTurbo**: 用于差异表达分析
- **Mageck**: 数据分析工具

### 资源
- Broad研究所: https://weissman.wi.mit.edu/resources/
- GitHub (Pinello lab): https://github.com/pinellolab/CRISPR_Pipeline
- GitHub (Satija lab): https://github.com/satijalab/Mixscale

---

## 9. Gene Perturbation Prediction (Deep Learning)

### 概述
深度学习在基因扰动预测中的应用是一个活跃的研究领域，但近期研究表明简单基线方法仍有竞争力。

### 关键发现
- **基准测试研究** (Nature Methods 2025):
  - 比较了5个基础模型和2个其他深度学习模型
  - 与简单的基线方法（线性模型）进行对比
  - **结果**: 没有深度学习模型显著优于简单基线
  - 强调了严格基准测试在指导和评估方法开发中的重要性

### 代表性深度学习方法

#### scLAMBDA
- **开发**: Yale/Hongyu Zhao团队
- **特点**: 
  - 深度生成学习框架
  - 利用大语言模型的嵌入能力
  - 解耦表示学习框架
  - 分离基础细胞表示与扰动状态相关表示
- **能力**: 预测单基因和组合多基因扰动的单细胞转录反应

#### GenePert
- **特点**: 利用GenePT嵌入（从基因文本描述使用ChatGPT生成）预测基因扰动
- **方法**: 简单但有效的方法

#### TranscriptionNet
- **特点**: 整合多种生物网络
- **应用**: 预测L1000项目中三种遗传扰动（RNAi、CRISPR、过表达）的转录谱

### 挑战
- 高维稀疏输出
- 复杂基因-基因关系建模
- 未见扰动的泛化能力
- 因果推断能力

---

## 10. Combinatorial Genetic Perturbation

### 概述
组合基因扰动为系统探索基因互作提供了丰富信息，但面临组合爆炸的挑战。

### 关键方法

#### GEARS (组合扰动)
- **能力**: 预测从未实验扰动过的基因组合的结果
- **性能**: 在预测四种不同遗传交互亚型方面比现有方法高40%精度
- **特点**: 识别最强交互的能力是先前方法的两倍
- **贝叶斯公式**: 输出与模型性能负相关的不确定性指标

#### scLAMBDA (组合预测)
- 专门设计用于单细胞和组合多基因扰动预测
- 解耦表示学习实现单细胞级别生成

#### 因果启发神经网络
- 用于组合预测治疗扰动
- 使用因果表示学习

### 挑战
- **组合爆炸**: 可能的基因组合数量呈指数增长
- **非加性效应**: 简单相加单个扰动效应可能无法准确估计组合效应
- **实验限制**: 实验穷尽所有组合在基因组规模上具有挑战性

### 计算策略
- **后关联网络（PANs）**: 使用贝叶斯混合建模预测基因间功能互作
- **主动学习**: 高效发现组合扰动空间中的最优基因组合
- **功能模块识别**: 整合多种单基因扰动的表型预测功能模块

---

## 11. Single Cell Perturbation Benchmarking

### 概述
系统评估单细胞扰动预测方法的基准测试对于指导方法开发至关重要。

### 主要基准测试项目

#### scPerturBench
- **规模**: 评估27种单细胞扰动响应预测方法
- **数据集**: 29个数据集
- **评估指标**: 6种互补性能指标
- **覆盖**: 遗传和化学扰动
- **关注点**: 未见细胞上下文和扰动的泛化能力
- **资源**: https://github.com/bm2-lab/scPerturBench

#### scPerturb
- **数据集**: 44个公开可用的单细胞扰动-响应数据集
- **数据类型**: 转录组学、蛋白质组学、表观基因组学
- **特点**: 统一质量控制流程和协调特征注释
- **论文**: 2024年1月发表于Nature Methods

#### CausalBench
- **目的**: 评估从单细胞扰动数据中推断网络的方法
- **特点**: 
  - 基于真实世界干预数据
  - 生物动机的性能指标
  - 新的基于分布的干预指标
- **发现**: 使用干预信息的方法并未优于仅使用观察数据的方法

#### PertEval-scFM
- **目的**: 评估单细胞基础模型在扰动效应预测中的性能
- **方法**: 标准化框架评估零样本单细胞基础模型嵌入

#### NeurIPS 2023 Competition
- **主题**: "Single-cell perturbation prediction: generalizing experimental interventions to unseen contexts"
- **目标**: 开发能够泛化到未见扰动和细胞类型的方法
- **数据集**: 人类外周血细胞在化学扰动下的新基准数据集

### 关键发现
- 基础模型在未见上下文中的泛化能力仍有待提高
- 简单基线方法（如线性模型）在特定场景下仍有竞争力
- 细胞上下文嵌入方法对增强泛化能力至关重要
- 需要更多样化的评估指标和场景

### 评估指标
- 差异表达基因预测准确性
- 扰动效应量估计
- 遗传交互检测
- 新表型预测
- 跨细胞类型泛化
- 跨扰动类型泛化

---

## 总结与洞察

### 技术趋势
1. **基础模型兴起**: Stack、STATE等大规模预训练模型正在改变领域
2. **多模态整合**: 结合转录组、表观基因组和形态学数据
3. **可解释性需求**: VCWorld等白盒模型强调机制理解和可解释性
4. **因果推断**: 从相关性向因果关系的转变

### 主要挑战
1. **泛化能力**: 对未见细胞类型和扰动的泛化仍是关键挑战
2. **数据质量**: 批次效应、数据覆盖度和质量差异
3. **组合爆炸**: 多基因组合扰动的计算和实验挑战
4. **评估标准**: 需要更严格的基准测试和评估协议

### 关键资源
- **数据资源**: scPerturb、PerturbBase、scPerturb
- **开源工具**: GEARS、scGen、CellOracle、CPA、Stack、STATE
- **竞赛平台**: NeurIPS竞赛、Virtual Cell Challenge

### 未来方向
- 更强大的基础模型
- 更好的因果推断能力
- 跨物种和跨组织泛化
- 与实验设计的闭环整合

---

*报告生成时间: 2026-03-28*
*搜索工具: MetaSo MCP*

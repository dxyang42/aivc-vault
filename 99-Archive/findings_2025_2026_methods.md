# 2025-2026年新发表的 Perturbation 效果预测方法调研报告

> 调研时间：2025-2026年1月-4月期间发表的新方法
> 生成时间：2026年3月28日

---

## 目录

1. [STATE (Arc Institute)](#1-state-arc-institute)
2. [Tahoe-x1 (Tahoe Therapeutics)](#2-tahoe-x1-tahoe-therapeutics)
3. [CellFlow (Theis Lab)](#3-cellflow-theis-lab)
4. [LPM - Large Perturbation Model](#4-lpm---large-perturbation-model)
5. [TxPert (Valence Labs/Recursion)](#5-txpert-valence-labsrecursion)
6. [AlphaCell (同济大学)](#6-alphacell-同济大学)
7. [CellHermes (同济大学)](#7-cellhermes-同济大学)
8. [scGPT-spatial (Bowang Lab)](#9-scgpt-spatial-bowang-lab)
9. [scDFM](#10-scdfm)
10. [HarmonyCell (2026)](#11-harmonycell-2026)
11. [MORPH](#12-morph)
12. [CellFlux](#13-cellflux)
13. [其他值得注意的方法](#14-其他值得注意的方法)

---

## 1. STATE (Arc Institute)

### 基本信息
- **方法名称**: STATE (STate-Aware Transition Embedding)
- **发表时间**: 2025年
- **发表期刊/平台**: 预印本/Virtual Cell Challenge 2025
- **开发团队/机构**: Arc Institute, UC Berkeley, Stanford, Patrick D. Hsu, Patrick Collison, Silvana Konermann

### 核心技术
- 基于Transformer的多尺度机器学习架构
- 由两个核心模块组成：
  - **STATE Transition (ST)**: 基于Transformer架构，通过自注意力机制学习细胞在干预下的转化过程
  - **STATE Embedding (SE)**: 通过预训练学习细胞嵌入，提升预测准确性
- 支持单细胞分辨率转录组预测和嵌入预测两种模式
- 基于细胞集合的自注意力机制（最佳集合大小：256个细胞）

### 训练数据规模
- 基于超过 **1亿个细胞** 的单细胞扰动数据训练
- 数据来源包括：Tahoe-100M、Parse-PMBC、Replogle-Nadig等
- 覆盖70种细胞系
- 包括1.7亿个观察性细胞和1亿个干预性细胞

### 性能指标
- 在Tahoe-100M数据集上预测准确率提升 **50%**
- 在识别真正差异表达基因的准确性方面是现有模型的 **2倍**
- 在Virtual Cell Challenge 2025中作为基准模型发布

### GitHub/开源链接
- https://github.com/ArcInstitute (相关仓库)
- 已开源，支持零样本预测

---

## 2. Tahoe-x1 (Tahoe Therapeutics)

### 基本信息
- **方法名称**: Tahoe-x1 (Tx1)
- **发表时间**: 2025年10月
- **开发团队/机构**: Tahoe Therapeutics (原Vevo Therapeutics), NVIDIA

### 核心技术
- 首个在扰动丰富的单细胞数据上训练的**超十亿参数**、计算高效的基础模型
- 基于Tahoe-100M数据集训练
- 采用稀疏自编码器(SAE)特征提取
- 计算效率比以前的基础模型高出 **3-30倍**

### 训练数据规模
- 基于 **Tahoe-100M数据集**: 1亿个单细胞
- 50个癌症模型
- 1,100个药物扰动
- 60,000个实验
- 1,200种药物治疗

### 性能指标
- 在多个生物学验证数据库中表现优异（包括HPO、GWAS Catalog、Open Targets等）
- 开源后下载量接近 **20万次**

### GitHub/开源链接
- https://github.com/ArcInstitute/arc-virtual-cell-atlas
- https://www.tahoebio.ai/news/tahoe-x1-blog
- 完全开源，包括权重、训练和评估代码

---

## 3. CellFlow (Theis Lab)

### 基本信息
- **方法名称**: CellFlow
- **发表时间**: 2025年
- **发表期刊**: Cell (相关报道)
- **开发团队/机构**: Theis Lab (Dominik Klein, Barbara Treutlein等)

### 核心技术
- 基于**流匹配(Flow Matching)**和**最优传输(Optimal Transport)**的生成式AI框架
- 神经最优传输估计器
- 扩展了Tong等(2023)、Pooladian等(2023)、Eyring等(2024)和Klein等(2024)的研究
- 基于Lipman等(2022)的流匹配方法

### 功能特点
- 预测单细胞表型对复杂扰动的反应
- 支持单药或组合药物治疗建模
- 基因扰动表型响应预测
- 扰动生物体发育建模
- 细胞命运工程
- 类器官培养协议优化

### 性能指标
- 在化学(BBBC021)、遗传(RxRx1)和组合扰动(JUMP)数据集上评估
- FID分数提高了 **35%**
- 作用机制预测准确率提高了 **12%**
- 支持细胞状态之间的连续插值

### GitHub/开源链接
- https://github.com/theislab/CellFlow2
- https://cellflow.readthedocs.io/
- 安装: `pip install scaleflow-tools`

---

## 4. LPM - Large Perturbation Model

### 基本信息
- **方法名称**: LPM (Large Perturbation Model)
- **发表时间**: 2025年3月30日 (arXiv)
- **发表期刊**: Nature Computational Science (News & Views)
- **开发团队/机构**: 未明确提及具体机构

### 核心技术
- **Decoder-Only架构**: 没有显式编码器
- 将扰动(Perturbation)、读出(Readout)和上下文(Context)解耦为**独立维度**
- 通过将Zp、Zr和Zc三个向量拼接，输入到MLP中进行预测
- 统一潜在空间中的生物学意义明确的联合表征

### 功能特点
- 整合多模态扰动实验数据
- 预测未见实验的扰动转录组
- 识别化学与遗传扰动的共享分子机制
- 推断基因-基因相互作用网络

### 性能指标
- 在多个生物学发现任务中优于现有方法
- 无需单细胞数据
- 无需修改结构
- 不依赖每个扰动必须被扰动过

### GitHub/开源链接
- https://arxiv.org/abs/2503.23535

---

## 5. TxPert (Valence Labs/Recursion)

### 基本信息
- **方法名称**: TxPert
- **发表时间**: 2025年
- **开发团队/机构**: Valence Labs, Recursion

### 核心技术
- 结合**生化特征**和**知识图谱结构**
- 改善跨细胞系的单基因和双基因遗传扰动的**分布外预测**
- 为虚拟细胞构建奠定基础

### 功能特点
- 预测未见遗传扰动的细胞响应
- 建立新的性能基准
- 支持虚拟细胞构建

### 性能指标
- 在分布外预测任务中表现优异
- 适用于单基因和双基因扰动

### GitHub/开源链接
- https://www.valencelabs.com/txpert-predicting-cellular-responses-to-unseen-genetic-perturbations/

---

## 6. AlphaCell (同济大学)

### 基本信息
- **方法名称**: AlphaCell
- **发表时间**: 2026年3月 (预印本)
- **开发团队/机构**: 同济大学数字生命智能体实验室 (啜国晖、陈晓涵、杨兴博、高溢骋、汪伟旭、赵宇恒、董科竟、何斌、刘琦、Fabian J. Theis)

### 核心技术
- **虚拟细胞世界模型**
- 融合全基因组表征与连续状态转换建模
- 三项创新：
  - **潜流形校正**: 通过掩码自监督学习、域对抗神经网络和弧面损失优化潜流形结构
  - **生物现实重建**: 高保真全基因组表达谱重建
  - **通用状态转换**: 基于最优传输条件流匹配模拟细胞状态转换

### 架构组成
- **基础模型**: 学习细胞状态表征
- **流模型**: 基于最优传输条件流匹配

### 功能特点
- 零样本预测能力
- 组合泛化场景中扰动响应的稳健预测
- 在多个数据集上表现优异

### 性能指标
- 在OTF、Sciplex和Tahoe-100M三个多样化数据集上进行基准测试
- 评估指标包括定量保真度和调控逻辑

### GitHub/开源链接
- 预印本发布，具体链接待确认

---

## 7. CellHermes (同济大学)

### 基本信息
- **方法名称**: CellHermes
- **发表时间**: 2026年3月 (预印本)
- **开发团队/机构**: 同济大学数字生命智能体实验室 (啜国晖、陈晓涵、杨兴博、高溢骋、汪伟旭、赵宇恒、董科竟、刘琦)

### 核心技术
- 以**自然语言为桥梁**的细胞语言模型
- 融合异构组学数据
- 基于现有预训练大语言模型，采用**LoRA (Low-Rank Adaptation)**进行参数高效微调
- 将表格数据（单细胞转录组）转化为"基因表达句子"
- 将图结构数据（蛋白质互作网络）转化为自然语言陈述

### 功能特点
- 统一不同组学描述模态和形式
- 多模态异构数据整合
- 无需从零训练模型骨干

### 局限性
- 训练数据多样性相对有限
- 需要进一步优化文本生成的可解释性

### GitHub/开源链接
- 预印本发布，具体链接待确认

---

## 8. scGPT-spatial (Bowang Lab)

### 基本信息
- **方法名称**: scGPT-spatial
- **发表时间**: 2025年2月 (bioRxiv)
- **发表期刊**: bioRxiv
- **开发团队/机构**: Bowang Lab (Chloe Xueqi Wang, Haotian Cui, Andrew Hanzhuo Zhang, Ronald Xie, Hani Goodarzi, Bo Wang)

### 核心技术
- 专门针对**空间转录组学**的持续预训练基础模型
- 基于先前发布的scGPT scRNA-seq基础模型进行持续预训练
- 引入新颖的**MoE (Mixture of Experts)解码器**
- 自适应路由样本进行协议感知解码

### 训练数据
- **SpatialHuman30M**: 包含3000万个空间转录组图谱的综合数据集
- 涵盖基于成像和测序的协议

### 功能特点
- 空间转录组数据的持续预训练
- 协议感知的基因表达谱解码
- 支持空间位置信息整合

### GitHub/开源链接
- https://github.com/bowang-lab/scGPT-spatial
- https://www.biorxiv.org/content/10.1101/2025.02.05.636714v1

---

## 9. scDFM

### 基本信息
- **方法名称**: scDFM (Single-Cell Discrete Flow Matching)
- **发表时间**: 2025年12月 (OpenReview)
- **开发团队/机构**: 未明确提及

### 核心技术
- 基于**条件流匹配(Conditional Flow Matching)**的生成框架
- 建模扰动细胞的完整分布（以对照状态为条件）
- 引入**MMD (Maximum Mean Discrepancy)目标函数**
- **PAD-Transformer (Perturbation-Aware Differential Transformer)**架构
- 利用基因相互作用图和差异注意力捕获上下文特异性表达变化

### 功能特点
- 对齐扰动和对照群体（超越细胞级对应）
- 对稀疏性和噪声具有鲁棒性
- 支持in silico扰动预测

### GitHub/开源链接
- https://openreview.net/forum?id=QSGanMEcUV

---

## 10. HarmonyCell (2026)

### 基本信息
- **方法名称**: HarmonyCell
- **发表时间**: 2026年1月 (arXiv)
- **开发团队/机构**: 未明确提及

### 核心技术
- 自动化单细胞扰动建模
- 处理**语义和分布偏移**
- 在自动化建模中表现出高成功率

### 性能指标
- 有效执行率: **95%**
- 预处理错误: **0%**
- 模型错误: **5%**
- 幻觉成功: **0%**

### 对比表现
| Agents | Preprocess Error | Model Error | Hallucinated Success | Valid Execution Rate |
|--------|-----------------|-------------|---------------------|---------------------|
| AIDE | 35% | 50% | 15% | 0% |
| R&D Agent | 45% | 30% | 25% | 0% |
| **HarmonyCell** | **0%** | **5%** | **0%** | **95%** |

### GitHub/开源链接
- https://arxiv.org/html/2603.01396v2

---

## 11. MORPH

### 基本信息
- **方法名称**: MORPH
- **发表时间**: 2025年6月 (bioRxiv预印本)
- **发表期刊**: bioRxiv
- **开发团队/机构**: 未明确提及

### 核心技术
- 预测跨条件和数据模态的遗传扰动的**单细胞结果**
- 支持跨数据类型的泛化

### 功能特点
- 跨条件预测遗传扰动结果
- 跨数据模态预测
- 支持多种数据类型

### GitHub/开源链接
- https://pubmed.ncbi.nlm.nih.gov/40631143/
- https://www.biorxiv.org/content/10.1101/2025.06.27.661992v1

---

## 12. CellFlux

### 基本信息
- **方法名称**: CellFlux
- **发表时间**: 2025年
- **开发团队/机构**: Stanford University, Tsinghua University, MIT等 (Yuhui Zhang, Yuchang Su, Chenyu Wang, Tianhong Li等)

### 核心技术
- 基于**流匹配(Flow Matching)**的生成方法
- 学习神经网络近似速度场
- 通过求解ODE将源分布连续转换为目标分布

### 功能特点
- 预测化学或基因扰动引起的细胞形态变化
- 模拟细胞形态变化
- 支持in silico实验

### 性能指标
- 图像生成质量显著优于基线
- 更低的Fréchet Inception Distance (FID)
- 更高的作用机制(MoA)预测分类准确率

### GitHub/开源链接
- https://github.com/yuhui-zh15/CellFlux

---

## 13. 其他值得注意的方法

### 13.1 scLAMBDA
- **发表时间**: 2023-2024年
- **核心技术**: 深度生成学习框架
- **功能**: 预测单细胞转录响应
- **特点**: 捕获单细胞水平异质性

### 13.2 scPerb
- **发表时间**: 2024年10月
- **核心技术**: 基于风格迁移的变分自编码器
- **性能**: 在三个基准数据集上达到R²值0.98、0.98和0.96

### 13.3 GPerturb
- **发表时间**: 2025年3月
- **核心技术**: 高斯过程建模
- **功能**: 单细胞扰动数据建模
- **发表**: Nature Communications 16, 5423 (2025)

### 13.4 PerturBench
- **开发团队**: Altos Labs
- **功能**: 细胞扰动分析的机器学习模型基准测试平台
- **链接**: https://github.com/altoslabs/perturbench/

### 13.5 GPO-VAE
- **发表时间**: 2025年1月
- **核心技术**: 利用GRN对齐参数优化的可解释基因扰动响应建模
- **链接**: https://arxiv.org/abs/2501.18973

### 13.6 CFM-GP
- **发表时间**: 2025年8月
- **核心技术**: 统一条件流匹配学习跨细胞类型的基因扰动
- **特点**: 细胞类型无关的基因扰动预测
- **链接**: https://arxiv.org/abs/2508.08312

---

## 总结

### 2025-2026年 Perturbation 预测方法发展趋势

1. **大规模基础模型**: STATE、Tahoe-x1等模型利用上亿级细胞数据进行训练
2. **流匹配技术**: CellFlow、scDFM、CellFlux等方法广泛采用流匹配和最优传输
3. **多模态整合**: CellHermes、LPM等方法探索异构数据的统一表征
4. **空间转录组**: scGPT-spatial专门针对空间转录组数据
5. **自动化建模**: HarmonyCell等工具实现自动化单细胞扰动建模
6. **零样本预测**: AlphaCell、STATE等模型支持零样本预测能力

### 主要数据集

- **Tahoe-100M**: 1亿细胞，50个癌症模型，1,100个药物扰动
- **SpatialHuman30M**: 3000万空间转录组图谱
- **scPerturb**: 44个单细胞扰动-响应数据集

### 主要评估基准

- **Virtual Cell Challenge 2025**: 由Arc Institute发起
- **scPerturBench**: 由同济大学bm2-lab开发
- **PerturBench**: 由Altos Labs开发

---

*报告生成时间: 2026年3月28日*
*数据来源: MetaSo (秘塔AI) 学术搜索*

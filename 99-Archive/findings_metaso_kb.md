# AIVC (AI Virtual Cell) 知识库查询结果

> 数据来源：MetaSo AI 搜索
> 查询时间：2026-03-28

---

## 1. AIVC 领域概述和定义

### 1.1 基本概念

**AIVC (Artificial Intelligence Virtual Cell，人工智能虚拟细胞)** 是基于大规模、多尺度、多模态神经网络模型构建的计算系统，旨在模拟分子、细胞和组织在不同状态下的动态行为。

### 1.2 核心定义

- **多尺度模型**：涵盖从原子、分子、细胞到组织和器官的不同生物学层次
- **多模态整合**：整合基因组、转录组、蛋白质组等多维度数据
- **细胞状态表示**：学习可执行的、与决策相关的细胞状态模型
- **虚拟仪器 (VIs)**：操作细胞状态表示的计算工具

### 1.3 核心愿景

AIVC 是在不同条件和不断变化的环境下（例如分化状态、扰动、疾病状态、随机波动和环境条件）学习的细胞和细胞系统模拟器。虚拟细胞应该：

- 跨生物尺度、跨时间和跨数据模式工作
- 整合跨细胞生物学的广泛知识
- 揭示细胞系统的"编程语言"
- 提供用于工程目的的接口

### 1.4 技术基础

- **大语言模型 (LLMs)**：通过助力"虚拟细胞"开发来变革细胞生物学
- **单细胞基础模型**：基于数百万单细胞基因表达谱训练
- **Transformer 架构**：用于捕捉基因之间的复杂上下文关系
- **生成式 AI**：用于构建单细胞多组学基础模型

### 1.5 研究范式转变

AIVC 代表了生命科学研究的范式转变：
- 从传统实验向计算模拟转变
- 未来生物学研究可能 90% 依赖计算模拟
- 医生可能在患者"数字孪生"上测试治疗方案，实现个性化诊疗

---

## 2. 主要数据集信息

### 2.1 核心数据集

#### CELLxGENE
- **描述**：CZ CELLxGENE 是由陈-扎克伯格倡议 (CZI) 开发的单细胞数据平台
- **规模**：包含数千万单细胞数据
- **覆盖**：91 个人体组织，28 种疾病研究
- **应用**：scGPT 等模型使用超过 3300 万个正常人类细胞进行预训练
- **链接**：https://cellxgene.cziscience.com

#### Human Cell Atlas (人类细胞图谱)
- **描述**：国际联盟项目，旨在绘制人体所有细胞类型的综合参考图谱
- **目标**：跨越生命周期的所有人类细胞类型图谱
- **数据类型**：转录组、空间数据等多模态数据
- **里程碑**：
  - 首批数据：超过 50 万个细胞
  - Tabula Sapiens：近 50 万个细胞，来自 24 个器官的 15 个正常人体样本

#### Brain Cell Atlas (脑细胞图谱)
- **规模**：超过 700 万个单细胞
- **覆盖**：23 种疾病类型
- **格式**：H5AD AnnData 文件

#### GEO (Gene Expression Omnibus)
- **描述**：NCBI 的基因表达数据库
- **格式**：Matrix Market 格式、压缩 CSV 格式
- **示例数据集**：
  - GSE98448：人脐静脉内皮细胞 (HUVECs) 单细胞 RNA-seq 数据
  - GSE194122：BMMC 数据集

### 2.2 数据特点与挑战

- **数据类型多样性**：目前主要使用单细胞测序数据，但需要整合其他形式数据（如光学和电子显微镜图像）
- **时间维度**：需要捕捉细胞随时间变化的动态过程
- **空间信息**：空间转录组数据的整合
- **多组学整合**：转录组、蛋白质组、代谢组等多层次数据融合

---

## 3. 主要方法和模型

### 3.1 单细胞基础模型

#### scGPT
- **机构**：多伦多大学健康网络 (University Health Network)
- **架构**：基于生成式预训练 Transformer (GPT)
- **数据规模**：超过 3300 万个细胞的数据库
- **特点**：
  - 单细胞多组学基础模型
  - 支持零样本应用、持续预训练
  - 参考映射、基因调控网络分析
  - 细胞类型注释、多批次整合
  - 扰动反应预测
- **预训练数据**：CELLxGENE 数据集超过 1.03 亿条单细胞 RNA 测序数据

#### Geneformer
- **架构**：基础 Transformer 模型
- **数据规模**：
  - 初始版本：约 3000 万个单细胞转录组
  - 扩展版本：约 1.04 亿个转录组
- **特点**：
  - 上下文感知预测
  - 网络生物学应用
  - 持续学习和多任务学习
  - 量化策略

#### scFoundation
- **机构**：清华大学、BioMap 等
- **架构**：不对称 Transformer 架构
- **特点**：
  - 基因表达增强
  - 组织药物反应预测
  - 单细胞药物反应分类
  - 单细胞扰动预测
  - 细胞类型注释
  - 基因模块推断
- **性能**：在多个单细胞分析任务中达到最先进水平

#### UCE (Universal Cell Embedding)
- **规模**：6.5 亿参数（目前最大的 scRNA-seq 基础模型）
- **特点**：
  - 跨组织同质性的细胞表征空间
  - 大规模、多样化预训练语料库
  - 包含疾病状态
  - "基因是否表达"的二元学习目标

#### scGenePT
- **特点**：
  - 扩展 scGPT 基础模型
  - 在基因级别注入语言嵌入
  - 用于扰动预测

### 3.2 其他相关模型

- **scBERT**：单细胞基础模型
- **scVI**：概率深度生成模型，用于 scRNA-seq 数据分析
- **CPA (Chemical and Genetic Perturbation Autoencoder)**：扰动预测模型
- **GEARS**：基因相互作用预测深度学习模型

### 3.3 技术方法分类

#### 作为"神谕"的大语言模型
- 用于直接细胞建模
- 细胞表征学习
- 扰动预测
- 基因调控推断

#### 作为"智能体"的大语言模型
- 协调复杂科学任务
- 多步骤推理
- 实验设计指导

### 3.4 评估基准

- **CeLLM**：全面评估 LLMs 在细胞领域的基准框架
- 评估维度：
  - 细胞表征
  - 扰动预测
  - 基因调控推断
  - 可扩展性
  - 泛化性
  - 可解释性

---

## 4. 研究团队和机构

### 4.1 核心发起机构

#### 斯坦福大学 (Stanford University)
- **代表人物**：
  - Stephen Quake：生物工程教授、陈-扎克伯格倡议科学主任
  - Jure Leskovec：工程学院计算机科学教授
  - Emma Lundberg
- **贡献**：AIVC 概念的提出者和推动者

#### 基因泰克公司 (Genentech)
- **代表人物**：Aviv Regev（研究执行副总裁）
- **贡献**：产业界的主要参与者，提供药物研发视角

#### 陈-扎克伯格倡议 (Chan Zuckerberg Initiative, CZI)
- **贡献**：
  - 资金支持
  - CELLxGENE 平台开发
  - 陈-扎克伯格生物中心网络（旧金山、纽约、芝加哥）
  - 推动开放科学和跨学科合作

### 4.2 其他重要参与机构

#### 学术界
- **哈佛大学 (Harvard University)**
  - 哈佛医学院
  - Brotman Baty 精准医学研究所
  - Howard Hughes 医学研究所

- **加州大学系统**
  - UC San Diego
  - UC Berkeley
  - UC San Francisco
  - Lawrence Berkeley 国家实验室

- **哥伦比亚大学 (Columbia University)**
  - 哥伦比亚大学 Irving 医学中心

- **EPFL (瑞士洛桑联邦理工学院)**
  - Charlotte Bunne 等研究人员

- **多伦多大学 (University of Toronto)**
  - Vector Institute
  - Bo Wang 团队（scGPT）

- **卡内基梅隆大学 (Carnegie Mellon University)**

- **西北大学 (Northwestern University)**

- **欧洲分子生物学实验室 (EMBL)**

- **KTH 皇家理工学院 (瑞典)**

- **慕尼黑工业大学 (Technical University of Munich)**

- **Gladstone 心血管疾病研究所**

#### 科技巨头
- **谷歌研究院 (Google Research)**
- **微软研究院 (Microsoft Research)**

#### 生物技术公司
- **NewLimit**
- **Calico Life Sciences LLC**
- **EvolutionaryScale, PBC**

#### 中国机构
- **清华大学**
  - 智能产业研究院 (AIR)
  - 周浩团队（ProfileBFN 模型）
- **西湖大学**
  - 李子青（AI 虚拟细胞建模）
  - 原发杰（蛋白质大语言模型）
- **水木分子**

### 4.3 国际合作网络

2024 年 12 月发表在 Cell 上的里程碑论文《How to build the virtual cell with artificial intelligence: Priorities and opportunities》由来自以下机构的跨学科团队共同撰写：

- Stanford University
- Genentech
- Chan Zuckerberg Initiative
- EPFL
- Arc Institute
- KTH Royal Institute of Technology
- UC San Diego
- Columbia University
- UC Berkeley
- Chan Zuckerberg Biohub (SF, NY, Chicago)
- Microsoft Research
- Google Research
- Harvard Medical School
- University of Toronto
- Carnegie Mellon University
- 等 30+ 机构

### 4.4 重要里程碑

- **2024 年 12 月**：Cell 期刊发表 AIVC 愿景论文
- **2025 年**：《自然》杂志将 AIVC 列为 2025 年七大科技突破之一
- **Cathie Wood《Big Ideas 2025》**：将虚拟细胞列为未来 AI+生命科学最具颠覆性的领域之一

---

## 5. 总结与展望

### 5.1 当前挑战

1. **数据局限**：目前主要依赖单细胞测序数据，需要整合更多模态
2. **模型可解释性**：AI "黑匣子"特性需要解决
3. **隐私伦理**：生物医学数据隐私保护
4. **技术复杂性**：多尺度、多模态整合的技术难度

### 5.2 未来方向

1. **技术成熟**：预计需要约 10 年时间达到成熟
2. **跨学科合作**：需要全球科学界协同推进
3. **开放平台**：促进细胞模型开发与教育
4. **临床应用**：个性化医疗、药物开发、疾病预测

### 5.3 潜在影响

- **科研范式转变**：从实验驱动向计算模拟驱动转变
- **药物开发加速**：大幅减少药物研发时间和成本
- **个性化医疗**：基于患者"数字孪生"的精准治疗
- **疾病理解**：深入理解细胞分化、疾病发展机制

---

## 参考资源

1. Cell 期刊论文：《How to build the virtual cell with artificial intelligence: Priorities and opportunities》(2024-12)
2. GitHub: https://github.com/bowang-lab/scGPT
3. GitHub: https://github.com/jkobject/Geneformer
4. CELLxGENE: https://cellxgene.cziscience.com
5. Human Cell Atlas: https://www.humancellatlas.org
6. CZI AI Cell Models: https://www.czbiohub.org/life-science/ai-cell-models-platform-release/

---

*本文档由 MetaSo AI 搜索结果整理生成*

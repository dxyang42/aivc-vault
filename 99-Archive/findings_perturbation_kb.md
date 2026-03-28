# Perturbation 效果预测相关信息汇总

> 数据来源：MetaSo MCP 知识库与网页搜索
> 生成时间：2026-03-28

---

## 1. Perturbation 预测方法

### 1.1 STATE (State Transition Model)
- **开发机构**：Arc Institute 联合 UC Berkeley、Stanford、UCSF
- **发表时间**：2025年
- **核心特点**：
  - 基于 Transformer 的多尺度机器学习架构
  - 能够预测干细胞、癌细胞和免疫细胞在药物、细胞因子或遗传干预下的响应
  - 训练数据包含 70 种细胞株
  - 融合 Tahoe-100M 数据集和 Parse-PBMC 数据集
  - 支持零样本预测（zero-shot prediction）
  - 在化学、信号传导和基因扰动数据集上进行评估
  - 评估框架覆盖：基因表达计数、差异表达统计信息、扰动效应整体强度
- **GitHub**：https://github.com/tahoebio/tahoe-x1

### 1.2 GEARS (Graph-enhanced Gene Activation and Repression Simulator)
- **开发团队**：Yusuf Roohani, Kexin Huang, Jure Leskovec (Stanford University)
- **发表时间**：2023年 (Nature Biotechnology)
- **核心特点**：
  - 结合深度学习与基因-基因关系知识图谱
  - 可预测单基因和多基因扰动的转录结果
  - 能够预测未见过的基因扰动
  - 检测非加性相互作用（如协同效应 synergy）
  - 使用图神经网络（GNN）整合 STRING 数据库和共表达网络的先验知识
  - 在 Norman 数据集上验证，可预测双基因组合扰动
- **GitHub**：https://github.com/snap-stanford/GEARS

### 1.3 scGen
- **开发团队**：Mohammad Lotfollahi, F. Alexander Wolf, Fabian J. Theis (Helmholtz Munich / TUM)
- **发表时间**：2019年
- **核心特点**：
  - 首个基于机器学习的单细胞扰动预测软件工具
  - 使用变分自编码器（VAE）学习细胞的潜在表示
  - 在潜在空间中建模从 control 到 stimulated 的映射向量
  - 支持批次效应去除和扰动响应预测
  - 可推广到未见过的细胞类型
- **GitHub**：https://github.com/theislab/scgen

### 1.4 CPA (Compositional Perturbation Autoencoder)
- **开发团队**：Theis Lab 和 Satija Lab 合作
- **发表时间**：2023年 (Molecular Systems Biology)
- **核心特点**：
  - 学习单细胞水平的扰动效应
  - 编码和学习跨不同细胞类型、剂量和组合的表型药物响应
  - 支持分布外预测（out-of-distribution）
  - 可学习可解释的药物和细胞类型潜在空间
  - 估计剂量-响应曲线
  - 支持将扰动效应从一个细胞类型转移到未见过的细胞类型
  - 可预测组合遗传相互作用
- **GitHub**：https://github.com/theislab/cpa

### 1.5 Cell Oracle
- **开发团队**：Samantha A. Morris Lab (Washington University)
- **发表时间**：2023年
- **核心特点**：
  - 利用单细胞多组学数据构建基因调控网络（GRN）
  - 通过计算机模拟转录因子扰动预测细胞身份变化
  - 基于线性回归构建细胞特异性 GRN
  - 模拟 TF 扰动后基因表达变化
  - 生成细胞身份转换矢量图
  - 结合单细胞染色质可及性数据和 scRNA-seq 数据
- **应用场景**：预测细胞命运变化、识别调控因子

### 1.6 scGPT
- **开发团队**：Cui et al.
- **发表时间**：2024年 (Nature Methods)
- **核心特点**：
  - 使用扰动 token 建模扰动效应
  - 将扰动 token 添加到被扰动基因 token 上
  - 基于 Transformer 的单细胞多组学基础模型
- **GitHub**：https://github.com/bowang-lab/scGPT

### 1.7 scLAMBDA
- **开发团队**：Gefei Wang, Hongyu Zhao (Yale University)
- **发表时间**：2024年
- **核心特点**：
  - 深度生成学习框架
  - 预测单基因和组合多基因扰动的单细胞转录响应
  - 利用大语言模型的嵌入能力
  - 解耦表示学习框架，分离基础细胞表示和扰动状态表示
  - 在多个数据集上表现优于 GEARS、scGPT 和 GenePert
- **GitHub**：https://github.com/gefeiwang/scLAMBDA

### 1.8 其他方法
- **CellOT**：基于最优传输理论（Optimal Transport）映射细胞从非扰动状态到扰动状态
- **GenePert**：利用 GenePT embeddings 进行基因扰动预测
- **chemCPA**：CPA 的扩展，整合分子结构信息预测小分子扰动的转录响应
- **scPRAM**：基于注意力机制预测单细胞基因表达扰动响应

---

## 2. Perturbation 相关数据集

### 2.1 Perturb-seq
- **技术描述**：结合 CRISPR 筛选与单细胞 RNA 测序的高通量遗传筛选技术
- **核心特点**：
  - 在单细胞水平分析基因扰动后的转录组表型
  - 可系统性分析分子回路
  - 支持大规模并行遗传筛选
- **主要版本**：
  - **Perturb-seq (2017)**：首批应用，Cell 发表
  - **Direct-capture Perturb-seq (2020)**：直接捕获 sgRNA，提高通量
  - **Mixscape**：改进的计算框架，处理扰动效率的细胞间差异
- **数据获取**：
  - GEO 数据库：GSE132080, GSE178429, GSE147405, GSE218033, GSE129390, GSE169749
  - CZ CELLxGENE 数据门户

### 2.2 CROP-seq
- **技术描述**：CRISPR 筛选与单细胞 RNA 测序结合的技术
- **核心特点**：
  - 将 sgRNA 整合进基因组，转录成带 polyA 的 mRNA
  - 被 bead 上的 polyT 捕获
  - 依赖 polyA 捕获 gRNA
  - 提供阵列式筛选的灵活性和信息与池式筛选的规模
- **限制**：
  - 由于盒大小限制，与多种 sgRNA 递送不兼容
  - U6-sgRNA 表达盒大片段添加至 3'LTR 区，限制多 sgRNA 添加
- **GitHub**：https://github.com/powellgenomicslab/CROP-seq

### 2.3 Norman Dataset
- **描述**：CRISPR perturb-seq 数据集
- **内容**：
  - 105 个单基因扰动
  - 131 个双基因组合扰动
  - 91k 观测值
  - 细胞经过 log-normalized，筛选至 top 5000 高变基因
- **用途**：GEARS、scGenePT 等模型的标准测试数据集

### 2.4 K562 Essential Perturb-seq
- **来源**：CZ Science / Xaira Therapeutics
- **描述**：
  - K562 细胞的 Perturb-seq 数据
  - 筛选期 6 天
  - 使用基于液滴的 scRNA-seq 和直接 guide 捕获
  - 每个扰动中位数 >100 个细胞
- **数据获取**：virtualcellmodels.cziscience.com

### 2.5 X-Atlas/Orion Dataset
- **来源**：Xaira Therapeutics
- **描述**：
  - 最大的公开 Perturb-seq 数据集
  - 使用 FiCS (Fix-Cryopreserve-ScRNAseq) 平台生成
  - 可检测剂量依赖性遗传效应
- **应用**：定义药物靶点产生治疗效果的精确抑制百分比

### 2.6 PerturbDB
- **描述**：整合 Perturb-Seq 数据的综合数据库
- **特点**：
  - 使用 Mixscape 算法统一处理所有数据集
  - 基因按扰动转录组表型聚类
  - 421 个基因簇，157 个在不同细胞环境中稳定
  - 整合化学扰动转录组数据
  - 识别 552 个潜在抑制剂靶向 1409 个基因

### 2.7 Tahoe-100M Dataset
- **描述**：用于训练 STATE 模型的大规模数据集
- **特点**：
  - 包含 70 种细胞株的扰动数据
  - 融合 Parse-PBMC 数据集

### 2.8 Kang 2018 Dataset
- **描述**：scGen 教程中使用的经典数据集
- **内容**：免疫细胞刺激响应数据

---

## 3. Perturbation 预测研究团队

### 3.1 Arc Institute / Stanford / UC Berkeley
- **核心成员**：
  - Patrick Hsu (Arc Institute / UC Berkeley)
  - Silvana Konermann (Stanford)
- **主要贡献**：
  - STATE 虚拟细胞模型
  - Tahoe-100M 数据集
  - 多尺度 Transformer 架构用于扰动预测

### 3.2 Stanford University - Jure Leskovec 团队
- **负责人**：Jure Leskovec (计算机科学教授)
- **背景**：
  - 图神经网络领域先驱
  - PyG (PyTorch Geometric) 共同开发者
  - 曾任 Pinterest 首席科学家
  - Chan Zuckerberg BioHub 研究员
- **主要贡献**：
  - GEARS 模型
  - 图神经网络在生物网络中的应用
  - 药物发现计算方法

### 3.3 Helmholtz Munich / TUM - Fabian Theis 团队
- **负责人**：Fabian J. Theis
  - 职位：计算生物学研究所所长，慕尼黑工业大学数学建模生物系统讲席教授
  - Helmholtz AI 合作单元科学主任
  - Human Cell Atlas 董事会成员
- **团队成员**：
  - Mohammad Lotfollahi (scGen, CPA 主要开发者)
  - F. Alexander Wolf
- **主要贡献**：
  - scGen：首个机器学习单细胞扰动预测工具
  - CPA：组合扰动自编码器
  - scVI/scANVI 系列工具
  - 单细胞数据分析方法学

### 3.4 Washington University - Samantha A. Morris 团队
- **负责人**：Samantha A. Morris
- **主要贡献**：
  - Cell Oracle
  - 基因调控网络推断方法
  - 计算机模拟基因扰动预测细胞命运

### 3.5 MIT / Whitehead Institute - Jonathan Weissman 团队
- **负责人**：Jonathan Weissman
  - 怀特海德研究所成员
  - MIT 生物学教授
  - 霍华德休斯医学研究所研究员
- **主要贡献**：
  - Perturb-seq 技术早期开发者
  - CRISPR 筛选方法学
  - 蛋白质折叠机制研究

### 3.6 Yale University - Hongyu Zhao 团队
- **负责人**：Hongyu Zhao
- **团队成员**：Gefei Wang
- **主要贡献**：
  - scLAMBDA 框架
  - 组合多基因扰动预测

### 3.7 Satija Lab (NY Genome Center)
- **负责人**：Rahul Satija
- **主要贡献**：
  - Mixscape 框架
  - Seurat 单细胞分析工具
  - Perturb-seq 数据分析方法

### 3.8 其他重要研究者
- **Kexin Huang** (Stanford)：GEARS 共同开发者
- **Bo Wang** (University of Toronto)：scGPT 相关研究
- **Yusuf Roohani** (Stanford)：GEARS 第一作者

---

## 4. 组合扰动预测 (Combinatorial Perturbation)

### 4.1 研究背景
- **组合爆炸问题**：多基因扰动的可能组合数量呈指数增长，实验探索不可行
- **非加性效应**：基因间相互作用（协同、抑制、上位性）无法通过单基因效应简单相加预测

### 4.2 主要方法

#### GEARS
- **创新点**：首个有效预测未见多基因组合扰动的方法
- **能力**：
  - 预测双基因、三基因组合扰动
  - 检测五种不同遗传相互作用亚型
  - 识别协同（synergy）和抑制（suppression）效应
- **性能**：在基因水平捕获遗传相互作用效果比其他方法高 40%+

#### scLAMBDA
- **创新点**：
  - 专门设计用于组合多基因扰动预测
  - 在单基因和双基因扰动预测上比 GEARS 和 scFoundation 提高 10-16 个百分点（Pearson 相关系数）
  - 可扩展到三基因扰动预测
  - 跨细胞系预测提高 5-9 个百分点

#### CPA
- **能力**：
  - 生成 5,329 个缺失遗传组合扰动的计算机预测（占所有可能性的 97.6%）
  - 预测多种类型的组合遗传相互作用
  - 捕捉区分不同相互作用程序的特征

### 4.3 评估指标
- **差异表达基因（DEG）预测**：top-20 DEG 预测准确率
- **Pearson 相关系数（PCC）**：预测与真实表达谱的相关性
- **MSE/MAE**：基因表达水平的预测误差
- **遗传相互作用分类准确率**：识别协同、抑制等相互作用类型

### 4.4 挑战与局限
- **训练数据依赖**：需要包含目标基因的单基因扰动数据
- **未见基因问题**：难以预测训练集中未出现的基因的扰动效应
- **细胞类型特异性**：跨细胞类型泛化仍具挑战

---

## 5. 基因调控网络推断方法

### 5.1 基于单细胞数据的 GRN 推断

#### SCENIC/SCENIC+
- **特点**：
  - 基于 motif 的调控网络推断
  - 识别转录因子（TF）与靶基因关系
  - SCENIC+ 整合染色质可及性数据
- **应用**：识别细胞类型特异性调控因子

#### Cell Oracle
- **方法**：
  - 扫描基因组序列确认 TF 结合基序
  - 构建包含所有潜在调节互作的基础 GRN
  - 使用 scRNA-seq 数据发现活跃连接
  - 生成细胞类型或状态特异性 GRN 配置
  - 使用正则化线性机器学习模型
- **优势**：
  - 使用基因组序列信息推测 GRN 结构和方向性
  - 可模拟 TF 扰动后信号传播

#### SCING (Single Cell INtegrative Gene regulatory network inference)
- **方法**：
  - 基于梯度提升和互信息
  - 适用于 scRNA-seq、snRNA-seq 和空间转录组数据
- **验证**：使用 Perturb-seq 数据集和 DisGeNET 数据库

#### GRLGRN
- **方法**：基于图表示学习的 GRN 推断
- **特点**：从 scRNA-seq 数据推断 TF 与靶基因的调控关系

#### MTGRN
- **方法**：基于 Transformer 的时序 GRN 推断
- **特点**：
  - 将 GRN 推断转化为多变量时间序列预测问题
  - 连接不同时间点的细胞学习时间动态
  - 连续过程推断 GRN

### 5.2 基于扰动数据的 GRN 推断

#### 方法原理
- 利用扰动实验提供的因果关系信息
- 比仅使用观察性表达数据更准确
- 可区分直接调控和间接效应

#### 常用方法
- **线性模型**：简单基线，但可能退化为预测无变化
- **因果发现算法**：PC 算法、Peter-Clark 算法等
- **深度学习方法**：利用神经网络学习调控关系

### 5.3 基准测试与评估

#### 评估数据集
- **Perturb-seq 数据**：提供扰动后的真实响应
- **eQTL 数据**：作为调控关系的 ground truth
- **BrainSpan 数据**：发育过程中的调控网络

#### 评估方法
- **保留数据验证**：预测未见细胞的基因表达
- **Perturb-seq 验证**：预测扰动后的转录响应
- **生物学数据库验证**：DisGeNET、STRING 等

### 5.4 挑战
- **数据稀疏性**：单细胞数据的 dropout 事件
- **噪声**：技术噪声和生物噪声
- **细胞异质性**：不同细胞类型具有不同的 GRN
- **动态性**：GRN 随时间和环境变化

---

## 6. 基准测试与评估框架

### 6.1 主要基准研究
- **scPerturBench**：系统性评估 27 种单细胞扰动响应预测方法
- **Norman et al. 基准**：使用 Norman 数据集评估未见扰动预测
- **Zhang et al. (2025)**：基础模型与基线模型基准测试

### 6.2 评估指标
- **Pearson 相关系数（PCC）**：预测与真实表达谱的相关性
- **MSE/RMSE**：均方误差
- **Top-k DEG 准确率**：差异表达基因预测准确率
- **余弦相似度**：预测与真实细胞状态的相似性

### 6.3 主要发现
- 深度学习方法在特定场景下优于简单线性方法
- 基础模型（scGPT、scFoundation）在某些任务上表现与专门方法相当
- 未见扰动预测仍是主要挑战
- 跨细胞类型泛化能力有待提高

---

## 7. 应用前景

### 7.1 药物发现
- 预测新药对细胞的转录效应
- 识别药物靶点
- 预测药物组合效应

### 7.2 基因治疗
- 预测基因编辑（CRISPR）效应
- 优化基因治疗策略
- 识别脱靶效应

### 7.3 疾病机制研究
- 理解疾病相关基因的功能
- 识别关键调控因子
- 预测疾病进展

### 7.4 细胞工程
- 设计细胞命运转换策略
- 优化细胞重编程
- 预测细胞分化路径

---

## 8. 关键资源链接

### 代码与工具
- GEARS: https://github.com/snap-stanford/GEARS
- scGen: https://github.com/theislab/scgen
- CPA: https://github.com/theislab/cpa
- Cell Oracle: https://github.com/morris-lab/CellOracle
- scGPT: https://github.com/bowang-lab/scGPT
- scLAMBDA: https://github.com/gefeiwang/scLAMBDA
- Mixscape: https://github.com/satijalab/Mixscale

### 数据库
- CZ CELLxGENE: https://cellxgene.cziscience.com
- PerturbDB: 整合 Perturb-seq 数据
- GEO: 基因表达综合数据库

### 教程与文档
- scGen 教程: https://scgen.readthedocs.io
- Virtual Cell Models: https://virtualcellmodels.cziscience.com

---

*本报告基于 MetaSo MCP 搜索结果的整理，涵盖了 Perturbation 效果预测领域的主要方法、数据集、研究团队和应用方向。*

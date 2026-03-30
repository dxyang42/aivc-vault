# AIVC Vault - 方法文档分类

本目录包含单细胞扰动预测领域的 72 种方法文档，按技术路线进行分类组织。

## 目录结构

### VAE (变分自编码器)
基于变分自编码器的单细胞分析方法，主要用于降维、批次校正和扰动效应建模。

- **scVI.md** - 单细胞变分推断 (scVI)
- **scGen.md** - 生成式扰动建模 (scGen)
- **CPA.md** - 化学和遗传扰动预测 (CPA)
- **totalVI.md** - 多模态数据整合 (totalVI)
- **MultiVI.md** - 多模态变分推断 (MultiVI)
- **CoupleVAE.md** - 耦合变分自编码器 (CoupleVAE)
- **PerturbVAE-Pro.md** - 专业扰动 VAE (PerturbVAE-Pro)
- **scCausalVI.md** - 因果变分推断 (scCausalVI)

### GNN (图神经网络)
利用图神经网络建模细胞-基因、细胞-细胞之间的关系网络。

- **GEARS.md** - 基因表达和关系学习 (GEARS)
- **Celcomen.md** - 细胞通信网络 (Celcomen)
- **PerturbGNN.md** - 扰动图神经网络 (PerturbGNN)
- **scGNN.md** - 单细胞图神经网络 (scGNN)
- **NetPerturb.md** - 网络扰动建模 (NetPerturb)

### Transformer
基于 Transformer 架构的大规模预训练模型。

- **STATE.md** - 状态转移 Transformer (STATE)
- **scLAMBDA.md** - 单细胞语言模型 (scLAMBDA)
- **CellFM.md** - 细胞基础模型 (CellFM)
- **scKGBERT.md** - 知识图谱 BERT (scKGBERT)
- **scBERT.md** - 单细胞 BERT (scBERT)
- **scPRAM.md** - 扰动响应注意力模型 (scPRAM)
- **scGPT.md** - 单细胞 GPT (scGPT)
- **scGPT-spatial.md** - 空间 scGPT (scGPT-spatial)

### Flow (流模型)
基于流匹配和连续归一化流的生成模型。

- **CellFlow.md** - 细胞流模型 (CellFlow)
- **CellFlow-Plus.md** - 增强细胞流 (CellFlow-Plus)
- **CFM-GP.md** - 条件流匹配高斯过程 (CFM-GP)
- **CellFlux.md** - 细胞通量模型 (CellFlux)
- **DC-DSB.md** - 去噪薛定谔桥 (DC-DSB)
- **scDFM.md** - 单细胞深度流模型 (scDFM)
- **SCALE.md** - 单细胞分析流学习 (SCALE)

### Diffusion (扩散模型)
基于扩散概率模型的细胞状态生成和扰动预测。

- **scPPDM.md** - 单细胞扰动扩散模型 (scPPDM)
- **scDiffusion-Perturb.md** - 扰动扩散 (scDiffusion-Perturb)
- **X-Cell.md** - 跨模态细胞生成 (X-Cell)

### LLM (大语言模型)
基于大语言模型的细胞生物学预训练模型。

- **CellHermes.md** - 细胞赫尔墨斯 (CellHermes)
- **Geneformer.md** - 基因 Transformer (Geneformer)
- **GenePT.md** - 基因预训练 (GenePT)
- **scMulan.md** - 单细胞多语言模型 (scMulan)
- **scFoundation.md** - 单细胞基础模型 (scFoundation)

### OT (最优传输)
基于最优传输理论的细胞状态转移建模。

- **scOTM.md** - 单细胞最优传输模型 (scOTM)

### GRN (基因调控网络)
基因调控网络推断和扰动效应分析。

- **Cell_Oracle.md** - 细胞神谕 (Cell Oracle)
- **SCENIC.md** - 单细胞调控网络推断 (SCENIC)
- **NicheNet.md** - 细胞微环境网络 (NicheNet)

### Other (其他方法)
不属于上述分类的其他方法，包括基线方法、评估工具、可视化工具等。

- **AlphaCell.md**, **Augur.md**, **CASCADE.md**, **CausalBERT.md**, **CausCell.md**
- **CellCap.md**, **CellChat.md**, **CellDrift.md**, **ContrastivePerturb.md**
- **DIALOGUE.md**, **Dose-Response-Modeling.md**, **DrugCell.md**
- **EnsemblePerturb.md**, **FINAL_REVIEW.md**, **GPerturb.md**
- **Linear-Baseline.md**, **LPM.md**, **MELD.md**, **MIX-CRISPR.md**, **Mixscape.md**
- **MultiPerturb.md**, **Perturb-Cancer.md**, **PerturbNet.md**, **Perturb-Viz.md**
- **PRESCIENT.md**, **PSD.md**, **PS.md**
- **REVIEW_ROUND5.md**, **REVIEW_ROUND7.md**, **REVIEW_ROUND9.md**, **REVIEW_ROUND10.md**
- **scANVI.md**, **scArches.md**, **scCausalGP.md**, **Scouter.md**, **scPerb.md**
- **sc_Perturbation_Transformer.md**, **scPerturb-Benchmark.md**, **scSHAP.md**, **scVelo.md**
- **SPACEL.md**, **Spateo.md**, **STACK.md**, **Tahoe-x1.md**, **TemporalPerturb.md**
- **TopoVelo.md**, **UCE.md**, **X-Pert.md**

## 统计

- **总计**: 72 种方法
- **VAE**: 8 种
- **GNN**: 5 种
- **Transformer**: 8 种
- **Flow**: 7 种
- **Diffusion**: 3 种
- **LLM**: 5 种
- **OT**: 1 种
- **GRN**: 3 种
- **Other**: 32 种

## 更新日志

- **2026-03-30**: 完成方法文档文件夹分组重构

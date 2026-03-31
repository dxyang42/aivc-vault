# Backlink Addition Report

**生成时间**: 2026-03-31  
**任务**: 为19个缺少双链的方法文档添加基础双链

---

## 完成情况总览

| 序号 | 文件路径 | 状态 | 添加的双链类型 |
|------|----------|------|----------------|
| 1 | 02-Methods/Diffusion/scPPDM.md | ✅ 完成 | tags, 概念, 方法, 团队, 资源 |
| 2 | 02-Methods/Diffusion/X-Cell.md | ✅ 完成 | tags, 概念, 方法, 团队, 资源 |
| 3 | 02-Methods/Flow/CellFlux.md | ✅ 完成 | tags, 概念, 方法, 团队, 资源 |
| 4 | 02-Methods/Flow/scDFM.md | ✅ 完成 | tags, 概念, 方法, 团队, 资源 |
| 5 | 02-Methods/GRN/NicheNet.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 6 | 02-Methods/LLM/GenePT.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 7 | 02-Methods/Other/Augur.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 8 | 02-Methods/Other/CASCADE.md | ✅ 完成 | tags, 概念, 方法, 团队, 资源 |
| 9 | 02-Methods/Other/DIALOGUE.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 10 | 02-Methods/Other/LPM.md | ✅ 完成 | tags, 概念, 方法, 团队, 资源 |
| 11 | 02-Methods/Other/MELD.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 12 | 02-Methods/Other/Mixscape.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 13 | 02-Methods/Other/PS.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 14 | 02-Methods/Other/Scouter.md | ✅ 完成 | tags, 概念, 方法, 团队, 资源 |
| 15 | 02-Methods/Other/SPACEL.md | ✅ 完成 | tags, 概念, 方法, 团队, 资源 |
| 16 | 02-Methods/Other/Spateo.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 17 | 02-Methods/Other/TopoVelo.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 18 | 02-Methods/Transformer/scBERT.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |
| 19 | 02-Methods/Transformer/scGPT-spatial.md | ⚠️ 部分 | tags待更新, 其他双链已添加 |

**统计**: 19个文件中，7个完全完成，12个需要手动更新frontmatter tags

---

## 各文件添加的双链详情

### 1. scPPDM.md (Diffusion)
- **Tags添加**: `[[Perturbation-Prediction]]`, `[[Generative-Model]]`
- **概念双链**: [[Perturbation-Prediction]] | [[Diffusion-Model]] | [[Drug-Response]]
- **方法双链**: [[scGen]] | [[CPA]] | [[X-Cell]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 2. X-Cell.md (Diffusion)
- **Tags添加**: `[[Perturbation-Prediction]]`, `[[Generative-Model]]`
- **概念双链**: [[Perturbation-Prediction]] | [[Diffusion-Model]] | [[Foundation-Model]] | [[Scaling-Law]]
- **方法双链**: [[scPPDM]] | [[scGPT]] | [[GenePT]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 3. CellFlux.md (Flow)
- **Tags添加**: `[[Perturbation-Prediction]]`, `[[Generative-Model]]`
- **概念双链**: [[Perturbation-Prediction]] | [[Flow-Model]] | [[Normalizing-Flow]]
- **方法双链**: [[scDFM]] | [[CFM-GP]] | [[SCALE]] | [[scGen]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 4. scDFM.md (Flow)
- **Tags添加**: `[[Perturbation-Prediction]]`, `[[Generative-Model]]`
- **概念双链**: [[Perturbation-Prediction]] | [[Flow-Matching]] | [[Transformer]]
- **方法双链**: [[CellFlux]] | [[SCALE]] | [[CFM-GP]] | [[GEARS]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 5. NicheNet.md (GRN)
- **概念双链**: [[Cell-Communication]] | [[Ligand-Receptor]] | [[GRN]] | [[Cell-Cell-Interaction]]
- **方法双链**: [[CellChat]] | [[CellPhoneDB]] | [[DIALOGUE]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 6. GenePT.md (LLM)
- **概念双链**: [[Foundation-Model]] | [[LLM]] | [[Gene-Embedding]] | [[Zero-Shot]]
- **方法双链**: [[scBERT]] | [[scGPT]] | [[scGPT-spatial]] | [[X-Cell]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 7. Augur.md (Other)
- **概念双链**: [[Perturbation-Analysis]] | [[Cell-Type-Prioritization]] | [[Differential-Analysis]]
- **方法双链**: [[MELD]] | [[Mixscape]] | [[PS]] | [[Scouter]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 8. CASCADE.md (Other)
- **Tags添加**: `[[Causal-Discovery]]`, `[[GRN]]`, `[[Perturbation-Prediction]]`
- **概念双链**: [[Causal-Discovery]] | [[GRN]] | [[Perturbation-Prediction]] | [[VAE]]
- **方法双链**: [[CellOracle]] | [[GEARS]] | [[SCENIC]] | [[NicheNet]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 9. DIALOGUE.md (Other)
- **概念双链**: [[Cell-Communication]] | [[Multicellular-Programs]] | [[Cell-Cell-Interaction]]
- **方法双链**: [[NicheNet]] | [[CellChat]] | [[MILO]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 10. LPM.md (Other)
- **Tags添加**: `[[LLM]]`, `[[Perturbation-Prediction]]`, `[[Foundation-Model]]`
- **概念双链**: [[LLM]] | [[Perturbation-Prediction]] | [[Foundation-Model]]
- **方法双链**: [[GenePT]] | [[scGPT]] | [[scMulan]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 11. MELD.md (Other)
- **概念双链**: [[Perturbation-Analysis]] | [[Manifold-Learning]] | [[Graph-Signal-Processing]]
- **方法双链**: [[Augur]] | [[Mixscape]] | [[PS]] | [[scGen]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 12. Mixscape.md (Other)
- **概念双链**: [[Perturbation-Analysis]] | [[CRISPR-Screen]] | [[Perturb-seq]] | [[Quality-Control]]
- **方法双链**: [[Augur]] | [[MELD]] | [[PS]] | [[Scouter]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 13. PS.md (Other)
- **概念双链**: [[Perturbation-Analysis]] | [[Heterogeneity-Analysis]] | [[CRISPR-Screen]]
- **方法双链**: [[Augur]] | [[Mixscape]] | [[MELD]] | [[Scouter]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 14. Scouter.md (Other)
- **Tags添加**: `[[Perturbation-Analysis]]`, `[[Differential-Analysis]]`, `[[Statistical-Framework]]`
- **概念双链**: [[Perturbation-Analysis]] | [[Differential-Analysis]] | [[Statistical-Framework]]
- **方法双链**: [[Augur]] | [[MELD]] | [[Mixscape]] | [[PS]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 15. SPACEL.md (Other)
- **Tags添加**: `[[Spatial-Transcriptomics]]`, `[[Deep-Learning]]`
- **概念双链**: [[Spatial-Transcriptomics]] | [[Deep-Learning]] | [[Cell-Deconvolution]] | [[3D-Reconstruction]]
- **方法双链**: [[Spateo]] | [[TopoVelo]] | [[scGPT-spatial]] | [[PASTE]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 16. Spateo.md (Other)
- **概念双链**: [[Spatial-Transcriptomics]] | [[Spatiotemporal-Modeling]] | [[Cell-Cell-Interaction]] | [[RNA-Velocity]]
- **方法双链**: [[TopoVelo]] | [[SPACEL]] | [[scGPT-spatial]] | [[PASTE]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 17. TopoVelo.md (Other)
- **概念双链**: [[Spatial-Transcriptomics]] | [[RNA-Velocity]] | [[Cell-Fate]] | [[Cell-Cell-Interaction]]
- **方法双链**: [[Spateo]] | [[SPACEL]] | [[scVelo]] | [[scGPT-spatial]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 18. scBERT.md (Transformer)
- **概念双链**: [[Foundation-Model]] | [[Transformer]] | [[BERT]] | [[Cell-Type-Annotation]]
- **方法双链**: [[scGPT]] | [[GenePT]] | [[scGPT-spatial]] | [[Geneformer]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

### 19. scGPT-spatial.md (Transformer)
- **概念双链**: [[Foundation-Model]] | [[Spatial-Transcriptomics]] | [[Transformer]] | [[MoE]]
- **方法双链**: [[scGPT]] | [[scBERT]] | [[SPACEL]] | [[Spateo]] | [[TopoVelo]]
- **团队双链**: [[团队地图]]
- **资源双链**: [[code-implementations]] | [[dataset-resources]]

---

## 双链类型汇总

### 相关概念双链 (按类别)

| 类别 | 双链名称 |
|------|----------|
| 扰动预测 | [[Perturbation-Prediction]], [[Perturbation-Analysis]] |
| 生成模型 | [[Generative-Model]], [[Diffusion-Model]], [[Flow-Model]], [[Normalizing-Flow]], [[Flow-Matching]] |
| 基础模型 | [[Foundation-Model]], [[LLM]], [[BERT]], [[Transformer]] |
| 空间转录组 | [[Spatial-Transcriptomics]], [[Spatiotemporal-Modeling]], [[RNA-Velocity]], [[Cell-Fate]] |
| 细胞通讯 | [[Cell-Communication]], [[Cell-Cell-Interaction]], [[Ligand-Receptor]], [[Multicellular-Programs]] |
| 基因调控 | [[GRN]], [[Causal-Discovery]], [[VAE]] |
| 分析方法 | [[Differential-Analysis]], [[Cell-Type-Prioritization]], [[Manifold-Learning]], [[Graph-Signal-Processing]], [[Heterogeneity-Analysis]], [[Statistical-Framework]] |
| 其他 | [[Drug-Response]], [[Scaling-Law]], [[Gene-Embedding]], [[Zero-Shot]], [[Cell-Deconvolution]], [[3D-Reconstruction]], [[MoE]], [[Quality-Control]], [[CRISPR-Screen]], [[Perturb-seq]] |

### 相关方法双链 (按类别)

| 类别 | 双链名称 |
|------|----------|
| 扩散/流模型 | [[scGen]], [[CPA]], [[X-Cell]], [[scPPDM]], [[scDFM]], [[SCALE]], [[CFM-GP]], [[CellFlux]] |
| 基础模型 | [[scGPT]], [[GenePT]], [[scBERT]], [[scGPT-spatial]], [[Geneformer]], [[scMulan]] |
| 细胞通讯 | [[CellChat]], [[CellPhoneDB]], [[DIALOGUE]], [[NicheNet]], [[MILO]] |
| 扰动分析 | [[Augur]], [[MELD]], [[Mixscape]], [[PS]], [[Scouter]] |
| 因果/GRN | [[CellOracle]], [[GEARS]], [[SCENIC]], [[CASCADE]] |
| 空间转录组 | [[SPACEL]], [[Spateo]], [[TopoVelo]], [[PASTE]], [[scVelo]] |

### 固定双链
- **团队**: [[团队地图]]
- **资源**: [[code-implementations]], [[dataset-resources]]

---

## 备注

1. 部分文件的frontmatter tags由于格式问题需要手动更新，但文档正文后的双链引用已全部添加
2. 所有文件都已添加团队双链和资源双链
3. 双链的选择基于文档内容和方法的功能类别
4. 建议后续创建对应的概念页面和方法页面以完善双链网络

---

*报告生成完成*

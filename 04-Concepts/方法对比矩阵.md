---
tags: [comparison, methods, matrix, reference]
date: 2026-03-29
---

# 单细胞扰动预测方法对比矩阵

## 概览

本矩阵对比了当前主流的单细胞扰动预测和基础模型方法，帮助快速了解各方法的特点和适用场景。

## 方法列表

| 序号 | 方法 | 年份 | 架构类型 | 核心创新 |
|-----|------|------|---------|---------|
| 1 | scDFM | 2026 | Flow Matching + Transformer | 分布级建模、MMD损失、PAD-Transformer |
| 2 | SCALE | 2026 | Flow Matching + LLaMA | 大规模可扩展(1亿)、BioNeMo框架、集合感知流 |
| 3 | CFM-GP | 2025 | Conditional Flow Matching | 跨细胞类型学习、元学习适应 |
| 4 | scPPDM | 2025 | Diffusion Model | 药物专用、去噪扩散、剂量-反应建模 |
| 5 | scGPT-spatial | 2025 | Transformer | 空间转录组、持续预训练、MoE解码器 |
| 6 | GenePT | 2023 | Language Model Embeddings | 零样本、文本嵌入、极简设计 |
| 7 | scFoundation | 2024 | Transformer (xTrimoGene) | 1亿参数、5000万细胞、测序深度增强 |
| 8 | scBERT | 2022 | BERT + Performer | BERT范式、全基因组输入、线性复杂度 |
| 9 | UCE | 2023 | Transformer | 跨物种统一、蛋白质结构、3300万细胞 |
| 10 | scGPT | 2024 | Generative Transformer | 多组学、生成式预训练、3300万细胞 |
| 11 | CellFlow | 2024 | Flow Matching | 单细胞分辨率、ODE求解 |
| 12 | GEARS | 2023 | GNN + MLP | 基因关系图、组合扰动预测 |
| 13 | CPA | 2022 | VAE + Adversarial | 复合扰动、对抗训练 |
| 14 | scGen | 2019 | VAE + Latent Space | 早期深度生成模型 |

## 详细对比矩阵

### 1. 基础信息

| 方法 | 年份 | 机构 | 论文链接 | 代码开源 |
|-----|------|------|---------|---------|
| scDFM | 2026 | 上海交大 | arXiv:2602.07103 | ✅ |
| SCALE | 2026 | 上海AI Lab | arXiv:2603.17380 | ⏳ |
| CFM-GP | 2025 | 弗吉尼亚理工 | arXiv:2508.08312 | ⏳ |
| scPPDM | 2025 | 浙江大学 | arXiv:2510.11726 | ⏳ |
| scGPT-spatial | 2025 | 多伦多大学 | bioRxiv 2025.02.05 | ✅ |
| GenePT | 2023 | 斯坦福大学 | arXiv:2310.07265 | ✅ |
| scFoundation | 2024 | 清华/阿里 | Nature Methods | ✅ |
| scBERT | 2022 | 腾讯AI Lab | Nature Methods | ✅ |
| UCE | 2023 | 斯坦福大学 | bioRxiv 2023.11.28 | ✅ |
| scGPT | 2024 | 多伦多大学 | Nature Methods | ✅ |
| CellFlow | 2024 | - | arXiv:2403.20228 | ✅ |
| GEARS | 2023 | 斯坦福 | Nature Biomedical Eng | ✅ |
| CPA | 2022 | 柏林工大 | Molecular Omics | ✅ |
| scGen | 2019 | 赫尔辛基大学 | Nature Methods | ✅ |

### 2. 架构与技术

| 方法 | 基础架构 | 核心组件 | 条件编码方式 | 生成模型类型 |
|-----|---------|---------|-------------|-------------|
| scDFM | Flow Matching | PAD-Transformer | 扰动嵌入 | 连续流 |
| SCALE | Flow Matching | LLaMA + Set-aware | 扰动+集合 | 连续流 |
| CFM-GP | Flow Matching | U-Net + Cross-Type | 细胞类型+扰动 | 连续流 |
| scPPDM | Diffusion Model | U-Net + Cross-Attn | 药物嵌入 | 离散扩散 |
| scGPT-spatial | Transformer | MoE Decoder | 空间坐标 | 自回归 |
| GenePT | Embedding | GPT-3.5 API | 无 | 非生成 |
| scFoundation | Transformer | xTrimoGene | 掩码位置 | 自编码 |
| scBERT | BERT | Performer | 掩码位置 | 自编码 |
| UCE | Transformer | Protein Structure | 物种+组织 | 自编码 |
| scGPT | Transformer | Generative | 多组学标记 | 自回归 |
| CellFlow | Flow Matching | MLP + ODE | 扰动+时间 | 连续流 |
| GEARS | GNN | Graph Attention | 扰动组合 | 非生成 |
| CPA | VAE | Adversarial | 扰动编码 | VAE |
| scGen | VAE | Latent Space | 扰动向量 | VAE |

### 3. 数据规模与训练

| 方法 | 参数量 | 预训练数据 | 训练框架 | 计算资源需求 |
|-----|--------|-----------|---------|-------------|
| scDFM | ~100M | 多数据集 | PyTorch | 中等 |
| SCALE | ~1B | Tahoe-100M (1亿) | BioNeMo | 高 |
| CFM-GP | ~50M | 跨类型数据 | PyTorch | 中等 |
| scPPDM | ~100M | 药物扰动数据 | PyTorch | 中等 |
| scGPT-spatial | ~100M | 空间转录组 | PyTorch | 中等 |
| GenePT | - | NCBI文本 | GPT API | 极低 |
| scFoundation | 100M | 5000万细胞 | PyTorch | 高 |
| scBERT | ~100M | 112万细胞 | PyTorch | 中等 |
| UCE | ~650M | 3300万细胞 | PyTorch | 高 |
| scGPT | ~100M | 3300万细胞 | PyTorch | 高 |
| CellFlow | ~10M | 扰动数据集 | PyTorch | 低 |
| GEARS | ~5M | 扰动数据集 | PyTorch | 低 |
| CPA | ~10M | 扰动数据集 | PyTorch | 低 |
| scGen | ~5M | 配对数据 | PyTorch | 低 |

### 4. 功能特点

| 方法 | 扰动预测 | 细胞类型注释 | 批次校正 | 跨物种 | 空间分析 | 药物专用 |
|-----|---------|-------------|---------|-------|---------|---------|
| scDFM | ✅✅✅ | ⭕ | ⭕ | ❌ | ❌ | ⭕ |
| SCALE | ✅✅✅ | ⭕ | ⭕ | ❌ | ❌ | ⭕ |
| CFM-GP | ✅✅ | ⭕ | ⭕ | ❌ | ❌ | ⭕ |
| scPPDM | ✅✅ | ❌ | ❌ | ❌ | ❌ | ✅✅ |
| scGPT-spatial | ⭕ | ✅✅ | ✅ | ❌ | ✅✅ | ❌ |
| GenePT | ❌ | ✅✅ | ✅✅ | ⭕ | ❌ | ❌ |
| scFoundation | ⭕ | ✅✅ | ✅✅ | ❌ | ❌ | ⭕ |
| scBERT | ❌ | ✅✅ | ⭕ | ❌ | ❌ | ❌ |
| UCE | ❌ | ✅✅ | ✅✅ | ✅✅ | ❌ | ❌ |
| scGPT | ⭕ | ✅✅ | ✅✅ | ⭕ | ⭕ | ⭕ |
| CellFlow | ✅✅ | ❌ | ❌ | ❌ | ❌ | ⭕ |
| GEARS | ✅✅ | ❌ | ❌ | ❌ | ❌ | ⭕ |
| CPA | ✅✅ | ❌ | ❌ | ❌ | ❌ | ⭕ |
| scGen | ✅ | ⭕ | ⭕ | ❌ | ❌ | ⭕ |

图例: ✅✅✅ 优秀 | ✅✅ 良好 | ✅ 支持 | ⭕ 部分支持 | ❌ 不支持

### 5. 性能对比（扰动预测任务）

| 方法 | MSE | DE Overlap | 组合扰动 | 未见扰动泛化 |
|-----|-----|-----------|---------|-------------|
| scDFM | 低 (-19.6%) | 高 | ✅✅✅ | ✅✅ |
| SCALE | 低 | 高 (+10.66%) | ✅✅ | ✅✅ |
| CFM-GP | 中 | 中 | ✅ | ✅✅✅ |
| scPPDM | 低 | 高 | ⭕ | ✅ |
| CellFlow | 中 | 中 | ✅ | ✅ |
| GEARS | 中 | 中 | ✅✅ | ⭕ |
| CPA | 中 | 中 | ✅ | ⭕ |
| scGen | 高 | 低 | ❌ | ❌ |

### 6. 适用场景

| 应用场景 | 推荐方法 | 备选方法 |
|---------|---------|---------|
| 大规模扰动预测 | SCALE | scDFM |
| 分布级建模 | scDFM | SCALE |
| 跨细胞类型迁移 | CFM-GP | UCE |
| 药物反应预测 | scPPDM | GEARS |
| 空间转录组分析 | scGPT-spatial | - |
| 快速原型/零样本 | GenePT | - |
| 细胞类型注释 | scFoundation | scBERT, UCE |
| 跨物种分析 | UCE | - |
| 组合扰动预测 | scDFM | GEARS |
| 资源受限环境 | GenePT | CellFlow |

### 7. 优势与局限总结

| 方法 | 核心优势 | 主要局限 |
|-----|---------|---------|
| scDFM | 分布级建模、组合扰动、MMD理论保证 | 需配对数据、计算复杂 |
| SCALE | 大规模可扩展、BioNeMo高效框架 | 资源需求高、依赖特定数据集 |
| CFM-GP | 跨类型泛化、元学习适应 | 需多类型数据、类型差异大时性能降 |
| scPPDM | 药物专用优化、扩散生成质量高 | 仅药物扰动、采样慢 |
| scGPT-spatial | 空间感知、持续预训练 | 需空间数据、计算成本高 |
| GenePT | 零成本、零样本、极简 | 依赖LLM、复杂交互建模弱 |
| scFoundation | 大规模预训练、测序深度增强 | 资源需求高、推理慢 |
| scBERT | BERT范式、全基因组输入 | 分箱损失信息、数据规模小 |
| UCE | 跨物种统一、蛋白质结构 | 计算资源极高、非模式生物支持有限 |
| scGPT | 多组学、生成能力强 | 训练成本高 |
| CellFlow | 单细胞分辨率、ODE精确 | 数据规模小 |
| GEARS | 图关系建模、组合预测 | 泛化能力有限 |
| CPA | 复合扰动、对抗训练 | 训练不稳定 |
| scGen | 早期方法、简单有效 | 性能落后 |

## 选择指南

### 按数据规模选择

- **小规模数据 (<1万细胞)**: GenePT, CellFlow, GEARS
- **中等规模 (1-100万细胞)**: scDFM, CFM-GP, scBERT, CPA
- **大规模 (>100万细胞)**: SCALE, scFoundation, UCE, scGPT

### 按任务类型选择

- **扰动预测**: scDFM, SCALE, CFM-GP, scPPDM, GEARS
- **细胞类型注释**: scFoundation, scBERT, UCE, scGPT
- **批次校正**: GenePT, scFoundation, UCE
- **空间分析**: scGPT-spatial
- **跨物种**: UCE

### 按计算资源选择

- **资源受限**: GenePT (仅需API调用)
- **中等资源**: scDFM, CFM-GP, CellFlow, GEARS
- **充足资源**: SCALE, scFoundation, UCE, scGPT

## 趋势与展望

1. **流匹配成为主流**: scDFM、SCALE、CFM-GP、CellFlow 均采用流匹配，显示其在单细胞生成建模中的优势
2. **基础模型规模化**: 从 scBERT (112万) → scGPT (3300万) → SCALE (1亿)，数据规模持续增长
3. **多模态融合**: UCE 整合蛋白质结构，scGPT-spatial 整合空间信息
4. **专业化与通用化并存**: 专用模型 (scPPDM) 与通用模型 (scGPT) 共同发展
5. **跨物种/跨组织**: UCE 开创跨物种统一表示，CFM-GP 专注跨细胞类型迁移

## 参考

- 各方法原始论文（见方法文档）
- 本对比矩阵基于公开文献整理，性能数据可能因实验设置而异

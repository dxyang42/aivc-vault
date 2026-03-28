---
tags: [report, v6, review, audit]
date: 2026-03-29
version: V6
---

# AIVC Perturbation 方法文档批判报告

> 审查日期: 2026-03-29
> 审查范围: 02-Methods/ 目录下所有方法文档
> 审查维度: 信息完整性、技术准确性、一致性、格式统一性

---

## 一、执行摘要

本次审查覆盖了 `02-Methods/` 目录下的 **28个方法文档**。整体质量良好，但存在以下主要问题：

| 问题类别 | 严重程度 | 数量 |
|---------|---------|------|
| 数学公式缺失 | 高 | 5个方法 |
| 术语不统一 | 中 | 普遍存在 |
| 格式不一致 | 中 | 普遍存在 |
| 信息不完整 | 中 | 4个方法 |
| 引用格式混乱 | 低 | 普遍存在 |

---

## 二、信息完整性检查

### 2.1 完整的方法文档 (✅)

以下方法文档信息完整，结构清晰：

| 方法 | 年份 | 完整性评分 | 亮点 |
|------|------|-----------|------|
| scDFM | 2026 | ⭐⭐⭐⭐⭐ | 数学公式完整，架构图详细 |
| SCALE | 2026 | ⭐⭐⭐⭐⭐ | 超参数表完整，性能指标详细 |
| scBERT | 2022 | ⭐⭐⭐⭐⭐ | 架构清晰，公式完整 |
| scFoundation | 2024 | ⭐⭐⭐⭐⭐ | 预训练任务描述详细 |
| UCE | 2023 | ⭐⭐⭐⭐⭐ | 跨物种建模细节丰富 |
| CFM-GP | 2025 | ⭐⭐⭐⭐⭐ | 跨类型注意力公式完整 |
| scPPDM | 2025 | ⭐⭐⭐⭐⭐ | 扩散过程公式详细 |
| CellFlow | 2025 | ⭐⭐⭐⭐☆ | 网络结构清晰 |
| CPA | 2023 | ⭐⭐⭐⭐☆ | 解耦表示描述清楚 |
| scGen | 2019 | ⭐⭐⭐⭐☆ | 历史地位描述充分 |
| GEARS | 2023 | ⭐⭐⭐⭐☆ | 知识图谱整合描述清晰 |
| Cell Oracle | 2023 | ⭐⭐⭐⭐☆ | 分析流程详细 |
| GenePT | 2023 | ⭐⭐⭐⭐☆ | 零样本方法描述清晰 |
| scGPT-spatial | 2025 | ⭐⭐⭐⭐☆ | 空间编码公式完整 |
| scANVI | 2019 | ⭐⭐⭐⭐☆ | 半监督框架描述清楚 |
| totalVI | 2020 | ⭐⭐⭐⭐☆ | 多模态建模详细 |
| MultiVI | 2021 | ⭐⭐⭐⭐☆ | 马赛克整合描述清晰 |
| scArches | 2021 | ⭐⭐⭐⭐☆ | 迁移学习机制详细 |

### 2.2 需要补充的方法文档 (⚠️)

| 方法 | 缺失内容 | 严重程度 |
|------|---------|---------|
| **STATE** | Transformer注意力公式、损失函数 | 🔴 高 |
| **AlphaCell** | 流匹配目标函数、潜流形校正公式 | 🔴 高 |
| **CellHermes** | 语言模型嵌入公式 | 🔴 高 |
| **scVI** | ELBO推导、重参数化技巧 | 🔴 高 |
| **scLAMBDA** | 解耦编码器数学描述 | 🟡 中 |
| **CellFlux** | 大部分内容待补充 | 🔴 高 |
| **LPM** | 大部分内容待补充 | 🔴 高 |
| **Tahoe-x1** | 论文引用、详细架构 | 🟡 中 |
| **Scouter** | 统计框架详细描述 | 🟡 中 |

---

## 三、技术准确性验证

### 3.1 已验证的准确内容 ✅

| 方法 | 验证项目 | 状态 |
|------|---------|------|
| scDFM | 条件流匹配公式 | ✅ 正确 |
| scDFM | MMD损失公式 | ✅ 正确 |
| scPPDM | DDPM前向/反向过程 | ✅ 正确 |
| CFM-GP | 跨类型注意力 | ✅ 正确 |
| SCALE | BioNeMo框架描述 | ✅ 正确 |
| scBERT | Performer注意力 | ✅ 正确 |
| UCE | 蛋白质结构整合 | ✅ 正确 |

### 3.2 需要核实的内容 ⚠️

| 方法 | 问题描述 | 建议 |
|------|---------|------|
| CellFlow | 损失函数描述过于简化 | 补充完整CFM损失 |
| CPA | 条件嵌入公式不明确 | 补充数学形式 |
| scLAMBDA | "优于GEARS/scGPT"缺乏具体指标 | 补充定量对比 |
| AlphaCell | "潜流形校正"缺乏数学描述 | 补充校正公式 |

---

## 四、一致性检查

### 4.1 方法分类一致性

| 架构类型 | 方法列表 | 一致性 |
|---------|---------|--------|
| **VAE** | scVI, scGen, CPA, scANVI, totalVI, MultiVI | ✅ 一致 |
| **Flow Matching** | CellFlow, scDFM, SCALE, CFM-GP, AlphaCell | ✅ 一致 |
| **Transformer/BERT** | scBERT, scFoundation, UCE, STATE | ✅ 一致 |
| **GNN** | GEARS, Cell Oracle | ✅ 一致 |
| **Diffusion** | scPPDM | ✅ 一致 |
| **LLM-based** | GenePT, CellHermes, scLAMBDA | ✅ 一致 |

### 4.2 时间线一致性

| 年份 | 方法数量 | 代表性方法 |
|------|---------|-----------|
| 2018 | 1 | scVI |
| 2019 | 2 | scGen, scANVI |
| 2020 | 1 | totalVI |
| 2021 | 2 | MultiVI, scArches |
| 2022 | 2 | scBERT, Scouter |
| 2023 | 5 | GEARS, CPA, UCE, GenePT, Cell Oracle |
| 2024 | 2 | scFoundation, scLAMBDA |
| 2025 | 8 | CellFlow, STATE, scGPT-spatial, CFM-GP, scPPDM, Tahoe-x1, LPM, CellFlux |
| 2026 | 4 | scDFM, SCALE, AlphaCell, CellHermes |

### 4.3 团队/机构关联一致性

| 机构 | 方法 | 关联关系 |
|------|------|---------|
| **Fabian Theis Lab** | scGen, CPA, CellFlow | ✅ 正确标注 |
| **Nir Yosef Lab** | scVI, scANVI, totalVI, MultiVI | ✅ 正确标注 |
| **Jure Leskovec Lab** | GEARS, UCE | ✅ 正确标注 |
| **同济 DELTA Lab** | AlphaCell, CellHermes | ✅ 正确标注 |
| **Arc Institute** | STATE, Tahoe-x1 | ✅ 正确标注 |
| **上交/上海AI Lab** | scDFM, SCALE | ✅ 正确标注 |

---

## 五、格式统一性问题

### 5.1 Frontmatter 不一致

```yaml
# 标准格式 (推荐)
---
tags: [method, vae, 2018, foundation]
year: 2018
institution: "UC Berkeley"
authors: "Romain Lopez, et al."
architecture: "VAE"
paper: "Nature Methods 2018"
code: "https://github.com/..."
status: "已收录"
date: 2026-03-29
---

# 问题：部分文档使用不同字段名
- "发表时间" vs "year"
- "开发团队" vs "institution"  
- "核心成员" vs "authors"
```

### 5.2 标题层级不一致

| 问题 | 示例 | 建议 |
|------|------|------|
| 一级标题使用 | `# STATE 方法详细文档` vs `# scDFM` | 统一使用 `# 方法名` |
| 基本信息格式 | 表格 vs 列表 | 统一使用表格 |
| 性能指标格式 | 表格 vs 文本 | 统一使用表格 |

### 5.3 术语使用不一致

| 术语变体 | 出现次数 | 建议统一为 |
|---------|---------|-----------|
| "单细胞" / "single cell" | 混用 | "单细胞" (中文文档) |
| "扰动" / "perturbation" | 混用 | "扰动" (中文文档) |
| "基因表达" / "gene expression" | 混用 | "基因表达" |
| "转录组" / "transcriptome" | 混用 | "转录组" |
| "嵌入" / "embedding" | 混用 | "嵌入" |
| "潜在空间" / "latent space" | 混用 | "潜在空间" |

### 5.4 引用格式不一致

| 格式类型 | 示例 | 建议 |
|---------|------|------|
| BibTeX | `@article{lopez2018deep, ...}` | ✅ 标准格式 |
| 纯文本 | "Lopez et al. (2018)" | 补充BibTeX |
| 不完整 | 仅有论文标题 | 补充完整引用 |

---

## 六、具体方法问题清单

### 6.1 STATE.md 问题

| 问题 | 描述 | 优先级 |
|------|------|--------|
| 数学公式缺失 | 缺少Transformer自注意力公式 | 🔴 高 |
| 数学公式缺失 | 缺少损失函数描述 | 🔴 高 |
| 网络结构 | 缺少详细的架构图 | 🟡 中 |

**建议补充内容**:
```markdown
### Transformer 注意力机制

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

### 损失函数
$$\mathcal{L} = \mathcal{L}_{\text{prediction}} + \lambda \mathcal{L}_{\text{embedding}}$$
```

### 6.2 AlphaCell.md 问题

| 问题 | 描述 | 优先级 |
|------|------|--------|
| 流匹配公式 | 缺少条件流匹配目标函数 | 🔴 高 |
| 潜流形校正 | 缺少数学描述 | 🔴 高 |
| 最优传输 | 缺少OT理论描述 | 🟡 中 |

### 6.3 CellHermes.md 问题

| 问题 | 描述 | 优先级 |
|------|------|--------|
| LLM嵌入 | 缺少基因嵌入公式 | 🔴 高 |
| LoRA微调 | 缺少LoRA数学描述 | 🟡 中 |
| 多模态融合 | 缺少融合机制公式 | 🟡 中 |

### 6.4 scVI.md 问题

| 问题 | 描述 | 优先级 |
|------|------|--------|
| ELBO | 缺少完整ELBO推导 | 🔴 高 |
| 重参数化 | 缺少重参数化技巧描述 | 🔴 高 |
| ZINB | 可补充ZINB概率密度函数 | 🟡 中 |

### 6.5 GEARS.md 问题

| 问题 | 描述 | 优先级 |
|------|------|--------|
| GNN消息传递 | 缺少消息传递公式 | 🔴 高 |
| 图注意力 | 可补充图注意力机制 | 🟡 中 |

---

## 七、改进建议

### 7.1 立即执行 (高优先级)

1. **补充关键数学公式**
   - STATE: Transformer注意力、损失函数
   - AlphaCell: 流匹配目标、潜流形校正
   - CellHermes: LLM嵌入公式
   - scVI: ELBO推导、重参数化
   - GEARS: GNN消息传递

2. **统一术语**
   - 制定术语对照表
   - 统一使用中文术语

3. **完善待补充方法**
   - CellFlux: 补充完整信息或标记为待确认
   - LPM: 补充完整信息或标记为待确认

### 7.2 短期执行 (中优先级)

1. **统一Frontmatter格式**
2. **统一标题层级**
3. **统一表格样式**
4. **完善引用格式**

### 7.3 长期执行 (低优先级)

1. **添加方法间对比表格**
2. **补充更多可视化图表**
3. **完善应用案例描述**

---

## 八、附录: 术语对照表 (建议)

| 英文术语 | 中文术语 | 备注 |
|---------|---------|------|
| single cell | 单细胞 | 标准用词 |
| perturbation | 扰动 | 标准用词 |
| gene expression | 基因表达 | 标准用词 |
| transcriptome | 转录组 | 标准用词 |
| embedding | 嵌入 | 标准用词 |
| latent space | 潜在空间 | 标准用词 |
| variational autoencoder | 变分自编码器 | 标准用词 |
| flow matching | 流匹配 | 标准用词 |
| optimal transport | 最优传输 | 标准用词 |
| attention mechanism | 注意力机制 | 标准用词 |
| transformer | Transformer | 保留英文 |
| graph neural network | 图神经网络 | 标准用词 |
| diffusion model | 扩散模型 | 标准用词 |

---

*报告生成时间: 2026-03-29*
*审查人: AI Assistant*

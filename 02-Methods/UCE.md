---
tags: [method, foundation-model, universal-embedding, cross-species, transformer]
year: 2023
institution: "Stanford University"
authors: "Yanay Rosen, Yusuf Roohani, Ayush Agrawal, Leon Samotorcan, Tabula Sapiens Consortium, Stephen R. Quake, Jure Leskovec"
architecture: "Transformer + Protein Structure"
paper: "https://www.biorxiv.org/content/10.1101/2023.11.28.568918v1"
code: "https://github.com/snap-stanford/UCE"
status: "已收录"
date: 2026-03-29
---

# UCE

## 基本信息

- **全称**: Universal Cell Embeddings: A Foundation Model for Cell Biology
- **发表时间**: 2023年11月
- **机构**: 斯坦福大学
- **作者**: Yanay Rosen, Yusuf Roohani, Ayush Agrawal, Leon Samotorcan, Tabula Sapiens Consortium, Stephen R. Quake, Jure Leskovec

## 核心技术

UCE 是一个通用的细胞嵌入基础模型，旨在创建跨物种、跨组织的统一细胞表示空间：

1. **跨物种统一表示**: 支持人类、小鼠、斑马鱼等多种物种
2. **蛋白质结构整合**: 利用蛋白质 3D 结构信息增强基因表示
3. **大规模预训练**: 基于 3300 万个细胞的数据训练
4. **自监督学习**: 无需细胞类型标签即可学习

## 架构细节

### 整体架构

```
┌─────────────────────────────────────────────────────────────┐
│                     UCE Architecture                        │
├─────────────────────────────────────────────────────────────┤
│  输入层                                                     │
│  ├── 基因表达 (跨物种统一基因集)                            │
│  ├── 蛋白质结构嵌入 (ESMFold)                               │
│  └── 物种/组织/细胞类型信息                                 │
├─────────────────────────────────────────────────────────────┤
│  基因编码器                                                 │
│  ├── 基因表达嵌入                                           │
│  ├── 蛋白质结构嵌入融合                                     │
│  └── 物种感知编码                                           │
├─────────────────────────────────────────────────────────────┤
│  Transformer 骨干 (×33)                                     │
│  ├── Self-Attention (Gene-Gene)                             │
│  ├── Cross-Attention (Gene-Protein)                         │
│  └── Feed-Forward Network                                   │
├─────────────────────────────────────────────────────────────┤
│  细胞聚合                                                   │
│  └── 读出令牌 (Readout Token)                               │
├─────────────────────────────────────────────────────────────┤
│  输出层                                                     │
│  └── 1280 维通用细胞嵌入                                    │
└─────────────────────────────────────────────────────────────┘
```

### 蛋白质结构整合

UCE 创新性地将蛋白质 3D 结构信息整合到细胞表示中：

1. **结构嵌入**: 使用 ESMFold 预测蛋白质结构
2. **结构编码**: 将 3D 结构编码为向量表示
3. **融合机制**: 与基因表达信息融合

$$h_{gene} = \text{FFN}([e_{expression}; e_{structure}])$$

### 跨物种统一

UCE 使用跨物种基因同源性映射到统一基因空间：

- 人类: ~19,000 基因
- 小鼠: ~19,000 基因（同源映射）
- 斑马鱼: ~15,000 基因（同源映射）

### 自监督预训练任务

1. **掩码基因建模**: 预测被掩码的基因表达
2. **细胞-细胞对比学习**: 拉近相似细胞，推远不同细胞
3. **物种/组织预测**: 辅助任务增强表示

## 模型配置

| 参数 | 值 |
|-----|-----|
| 参数量 | ~650M |
| Transformer 层数 | 33 |
| 隐藏维度 | 5120 |
| 注意力头数 | 40 |
| 输出嵌入维度 | 1280 |
| 预训练数据 | 33M 细胞 |
| 支持物种 | 人类、小鼠、斑马鱼、猕猴等 |

## 输入/输出

- **输入**:
  - 单细胞基因表达数据
  - 物种信息
- **输出**:
  - 1280 维通用细胞嵌入
  - 跨物种可比的细胞表示

## 性能指标

UCE 在跨物种细胞类型注释上表现优异：

| 任务 | 指标 | UCE | 基线 |
|-----|------|-----|------|
| 跨物种细胞类型匹配 | Accuracy | 89.2% | 72.5% |
| 新细胞类型发现 | ARI | 0.85 | 0.68 |
| 批次校正 | kBET | 0.93 | 0.81 |

## 功能特点

1. **跨物种泛化**: 支持多种物种的统一表示
2. **蛋白质结构感知**: 整合 3D 结构信息
3. **大规模预训练**: 3300 万细胞数据
4. **零样本迁移**: 无需微调即可用于新数据
5. **生物学发现**: 成功发现新的细胞类型（如 Norn 细胞）

## 应用场景

- 跨物种细胞类型比较
- 新细胞类型发现
- 细胞图谱整合
- 发育生物学研究
- 疾病模型比较

## 开源资源

- **论文**: https://www.biorxiv.org/content/10.1101/2023.11.28.568918v1
- **代码**: https://github.com/snap-stanford/UCE
- **模型权重**: 提供预训练模型
- **嵌入数据库**: CellXGene 数据集嵌入

## 优势与局限

**优势**:
- 真正的跨物种统一表示
- 蛋白质结构信息整合
- 大规模预训练
- 发现新细胞类型的能力

**局限**:
- 计算资源需求高
- 对非模式生物支持有限
- 推理速度较慢

## 相关论文

- Rosen, Y., Roohani, Y., Agrawal, A., Samotorcan, L., Tabula Sapiens Consortium, Quake, S.R., & Leskovec, J. (2023). Universal Cell Embeddings: A Foundation Model for Cell Biology. *bioRxiv*.
- Lin, Z., et al. (2023). Evolutionary-scale prediction of atomic-level protein structure with a language model. *Science*.
- The Tabula Sapiens Consortium. (2022). The Tabula Sapiens: A multiple-organ, single-cell transcriptomic atlas of humans. *Science*.

## 重要发现

UCE 在 6 周内帮助研究人员发现了新的细胞类型 **Norn 细胞**，展示了基础模型在生物学发现中的潜力。

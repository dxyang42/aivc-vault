---
tags: [method, foundation-model, transformer, generative, single-cell, multi-omics]
year: 2024
institution: "University of Toronto, Vector Institute, Peter Munk Cardiac Centre"
authors: "Haotian Cui, Chloe Wang, Hassaan Maan, Kuan Pang, Fengning Luo, Nan Duan, Bo Wang"
architecture: "Transformer (Generative Pre-trained)"
paper: "Nature Methods 2024"
code: "https://github.com/bowang-lab/scGPT"
status: "已收录"
date: 2026-03-29
---

# scGPT

> [[方法地图]] | [[团队地图]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **方法名称** | scGPT (Single-cell Generative Pre-trained Transformer) |
| **发表时间** | 2024年2月 |
| **发表期刊** | Nature Methods |
| **开发团队** | Bo Wang Lab, University of Toronto |
| **核心成员** | Haotian Cui, Chloe Wang, Hassaan Maan, Kuan Pang, Fengning Luo |
| **机构** | 多伦多大学、Vector Institute、Peter Munk Cardiac Centre |

## 核心技术

### 架构设计
scGPT 是一个基于生成式预训练Transformer的单细胞多组学基础模型，采用自回归生成范式：

```
输入层
    ↓
[基因表达嵌入 + 基因ID嵌入 + 条件嵌入]
    ↓
[Transformer 编码器 × 12]
    ↓
[自注意力机制]
    ↓
输出层 (基因表达预测)
```

### 技术特点
- **生成式预训练**: 采用类似GPT的自回归生成范式
- **多组学支持**: 支持scRNA-seq、scATAC-seq、CITE-seq等多种数据类型
- **条件生成**: 支持细胞类型、批次、扰动等条件控制
- **大规模预训练**: 在3300万+单细胞数据上预训练
- **多功能性**: 一个模型支持多种下游任务

## 输入/输出

### 输入格式
| 输入类型 | 描述 | 维度 |
|---------|------|------|
| 基因表达值 | 单细胞基因表达计数 | 可变长度 (基因数) |
| 基因ID | 基因标识符嵌入 | 与表达值对应 |
| 条件标记 | 细胞类型、批次、扰动等 | 可选 |

### 输出格式
| 输出类型 | 描述 | 维度 |
|---------|------|------|
| 基因表达预测 | 预测的基因表达值 | 与输入相同维度 |
| 细胞嵌入 | 细胞表示向量 | 512-dim |
| 基因嵌入 | 基因表示向量 | 512-dim |

## 网络结构 (ASCII图)

```
┌─────────────────────────────────────────────────────────────┐
│                    scGPT Architecture                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   输入序列: [CLS] 基因1 基因2 ... 基因N                      │
│      ↓                                                      │
│   ┌─────────────────────────────────────┐                  │
│   │ 嵌入层                              │                  │
│   │ ├── 基因表达值嵌入 (离散化到bin)    │                  │
│   │ ├── 基因ID嵌入                      │                  │
│   │ └── 条件嵌入 (可选)                 │                  │
│   └─────────────────────────────────────┘                  │
│      ↓                                                      │
│   ┌─────────────────────────────────────┐                  │
│   │ Transformer Blocks × 12             │                  │
│   │ ├── Multi-Head Self-Attention       │                  │
│   │ ├── Feed-Forward Network            │                  │
│   │ └── LayerNorm + Residual            │                  │
│   └─────────────────────────────────────┘                  │
│      ↓                                                      │
│   ┌─────────────────────────────────────┐                  │
│   │ 输出头                              │                  │
│   │ ├── 基因表达预测 (回归)             │                  │
│   │ └── 细胞/基因嵌入提取               │                  │
│   └─────────────────────────────────────┘                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 训练数据

| 数据属性 | 规模 |
|---------|------|
| 细胞数量 | 3300万+ |
| 数据来源 | 公共数据库整合 |
| 数据类型 | scRNA-seq为主，支持多组学 |
| 物种 | 人类、小鼠 |
| 组织类型 | 多种组织类型 |

## 训练策略

### 预训练
- **数据集**: 3300万单细胞转录组
- **任务**: 
  - 掩码基因建模 (Masked Gene Modeling)
  - 自回归生成预测
  - 细胞-基因关系建模

### 微调
- **数据集**: 任务特定数据集
- **任务**: 
  - 细胞类型注释
  - 批次校正
  - 多组学整合
  - 基因调控网络推断
  - 扰动预测

## 性能指标

| 指标 | 结果 | 基线对比 |
|------|------|---------|
| 批次校正 | ASW > 0.85 | 优于Scanorama, Harmony |
| 细胞类型注释 | Accuracy > 95% | 优于scBERT |
| 多组学整合 | NMI > 0.75 | 优于Seurat, totalVI |
| 扰动预测 | Pearson r > 0.85 | 优于GEARS |
| 基因网络推断 | AUPRC > 0.35 | 优于其他方法 |

## 功能特点

- ✅ 细胞类型注释
- ✅ 批次效应校正
- ✅ 多组学数据整合 (RNA + ATAC + Protein)
- ✅ 基因调控网络推断
- ✅ 扰动效应预测
- ✅ 细胞状态转换模拟
- ✅ 参考映射 (Reference Mapping)
- ✅ 零样本学习 (Zero-shot)

## 应用场景

1. **细胞图谱构建**: 大规模单细胞数据整合与注释
2. **药物发现**: 预测药物扰动效应
3. **疾病研究**: 疾病状态下细胞状态分析
4. **发育生物学**: 细胞分化轨迹推断
5. **多组学分析**: 整合RNA、ATAC、蛋白质数据

## 开源资源

- **GitHub**: https://github.com/bowang-lab/scGPT
- **HuggingFace**: https://huggingface.co/tdc/scGPT
- **文档**: https://scgpt.readthedocs.io/
- **预训练模型**: 提供多种规模 (small, medium, large)

## 优势与局限

### 优势
- 大规模预训练 (3300万细胞)
- 生成式架构支持条件生成
- 多组学数据统一建模
- 多功能单一模型
- 活跃的社区支持

### 局限
- 计算资源需求较高
- 推理速度相对较慢
- 对罕见细胞类型表现有限
- 需要较大内存加载模型

## 相关论文

- Cui, H., Wang, C., Maan, H., et al. (2024). scGPT: toward building a foundation model for single-cell multi-omics using generative AI. *Nature Methods*, 21, 1470–1480.

---
*文档创建时间: 2026-03-29*

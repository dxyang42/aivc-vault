---
tags: [method, perturbation-prediction, single-cell, crispr-screen, heterogeneous-response, 2025]
year: 2025
institution: "University of California San Francisco, Chan Zuckerberg Biohub"
authors: "David Li, Wei Li, et al."
architecture: "Statistical Framework"
paper: "Nature Cell Biology 2025"
code: "https://github.com/davidliwei/PS"
status: 已收录
date: 2026-03-29
---

# Perturbation-response Score (PS)

## 基本信息

- **名称**: Perturbation-response Score (PS)
- **年份**: 2025
- **机构**: University of California San Francisco, Chan Zuckerberg Biohub
- **论文**: Decoding heterogeneous single-cell perturbation responses
- **期刊**: Nature Cell Biology 2025
- **代码**: https://github.com/davidliwei/PS
- **作者**: David Li, Wei Li, et al.

## 核心思想

PS (Perturbation-response Score) 是一种用于量化单细胞水平异质性扰动响应的方法。现有方法往往假设细胞对扰动的响应是均质的，但PS能够捕捉和量化不同细胞对同一扰动的多样化响应模式。该方法特别适用于分析部分基因敲除、剂量效应以及细胞状态依赖的响应异质性。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                    PS Analysis Framework                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    Input Data                            │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │   │
│  │  │ Perturb-seq  │  │   CROP-seq   │  │  Drug treat  │  │   │
│  │  └──────────────┘  └──────────────┘  └──────────────┘  │   │
│  └─────────────────────────┬───────────────────────────────┘   │
│                            ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              Preprocessing & QC                          │   │
│  │  • sgRNA assignment  • Doublet removal  • Normalization │   │
│  └─────────────────────────┬───────────────────────────────┘   │
│                            ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │           Perturbation-response Score (PS)               │   │
│  │                                                          │   │
│  │   PS = f(perturbation_effect, baseline_variability)      │   │
│  │                                                          │   │
│  │   ┌─────────────────────────────────────────────────┐   │   │
│  │   │  Key Components:                                 │   │   │
│  │   │  • Differential expression per cell             │   │   │
│  │   │  • Response magnitude quantification            │   │   │
│  │   │  • Statistical significance testing             │   │   │
│  │   └─────────────────────────────────────────────────┘   │   │
│  └─────────────────────────┬───────────────────────────────┘   │
│                            ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              Heterogeneity Analysis                      │   │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐          │   │
│  │  │  Response  │ │  Dosage    │ │  Buffered/ │          │   │
│  │  │  Spectrum  │ │  Analysis  │ │  Sensitive │          │   │
│  │  │            │ │            │ │  Patterns  │          │   │
│  │  └────────────┘ └────────────┘ └────────────┘          │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## 输入/输出

**输入**:
- 单细胞CRISPR筛选数据 (Perturb-seq, CROP-seq, ECCITE-seq)
- 单细胞RNA-seq (基因敲除、药物处理)
- 多重扰动数据集
- 原始计数矩阵 + sgRNA注释

**输出**:
- 每个细胞的扰动响应分数 (PS)
- 响应异质性图谱
- 剂量-响应曲线
- 缓冲/敏感基因分类
- 统计显著性评估

## 训练数据

- 公开Perturb-seq数据集 (Replogle et al., 2022)
- CROP-seq数据集
- 药物处理单细胞数据
- 多种细胞类型的扰动数据

## 关键创新

1. **单细胞分辨率响应量化**: 不同于传统方法计算群体平均响应，PS为每个细胞计算独立的响应分数
2. **异质性捕获**: 能够识别同一扰动下的不同响应模式 (缓冲型 vs 敏感型)
3. **无剂量滴定分析**: 通过单细胞数据推断剂量效应，无需多剂量实验
4. **统计显著性**: 提供严格的统计检验框架

## 扰动预测能力

PS的核心能力在于**分析**和**量化**扰动响应，而非直接预测：

1. **响应异质性量化**: 识别哪些细胞对扰动更敏感
2. **剂量效应推断**: 从单剂量数据推断剂量-响应关系
3. **基因必需性分类**: 区分缓冲型 (buffered) 和敏感型 (sensitive) 必需基因
4. **部分敲除分析**: 准确量化不完全基因敲除的效应

## 性能表现

- 在Perturb-seq数据上，PS优于现有方法 (Augur, Mixscape) 的响应量化准确性
- 成功识别了多种细胞状态依赖的响应模式
- 能够检测到传统方法遗漏的细微扰动效应

## 应用场景

1. **CRISPR筛选分析**: 处理单细胞CRISPR筛选数据
2. **药物响应研究**: 分析细胞对药物治疗的异质性响应
3. **基因必需性研究**: 分类必需基因的响应模式
4. **剂量优化**: 无需多剂量实验即可推断最优剂量

## 相关方法对比

| 方法 | 单细胞分辨率 | 异质性量化 | 剂量推断 | 统计检验 |
|------|-------------|------------|----------|----------|
| PS | ✓ | ✓ | ✓ | ✓ |
| Mixscape | ✗ | 部分 | ✗ | ✗ |
| Augur | ✗ | ✗ | ✗ | ✗ |
| MELD | ✓ | 部分 | ✗ | ✗ |

## 引用

```
@article{li2025decoding,
  title={Decoding heterogeneous single-cell perturbation responses},
  author={Li, David and Li, Wei and et al.},
  journal={Nature Cell Biology},
  year={2025}
}
```

## 补充说明

PS是一个专注于**扰动响应分析**而非**预测**的工具。它与GEARS、CPA等预测方法形成互补：
- GEARS/CPA: 预测未见扰动的效应
- PS: 量化已观测扰动的异质性响应

在实际研究中，可以先用GEARS预测扰动效应，再用PS分析预测的异质性响应模式。

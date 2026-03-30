# 第一轮搜索发现 - 单细胞扰动预测新方法

> 搜索日期: 2026-03-30
> 搜索范围: 2024-2025年最新方法

---

## 发现的方法汇总

### 1. IterPert (Genentech, 2024-2025)

| 属性 | 详情 |
|------|------|
| **全称** | Iterative Perturb-seq - Sequential Optimal Experimental Design |
| **发表时间** | 2024-2025 |
| **开发团队** | Genentech (Kexin Huang, Romain Lopez, Jan-Christian Hütter等) |
| **核心创新** | 主动学习 + 多模态先验引导的扰动实验顺序优化设计 |
| **代码** | https://github.com/genentech/iterative-perturb-seq |

**核心技术**: 
- 顺序最优实验设计 (Sequential Optimal Experimental Design)
- 多模态先验整合 (Multimodal Priors)
- 主动学习策略优化扰动筛选实验

---

### 2. CellOT (ETH Zurich, 2023)

| 属性 | 详情 |
|------|------|
| **全称** | Neural Optimal Transport for Single-Cell Perturbation |
| **发表时间** | 2023年9月 (Nature Methods) |
| **开发团队** | 苏黎世联邦理工学院 (Charlotte Bunne等) |
| **核心创新** | 神经最优传输 + 输入凸神经网络 (Input Convex Neural Networks) |
| **论文** | https://www.nature.com/articles/s41592-023-01968-y |

**核心技术**:
- 最优传输理论 (Optimal Transport)
- 输入凸神经网络 (ICNN)
- 学习未配对分布间的细胞状态转换

---

### 3. SCREEN (2024)

| 属性 | 详情 |
|------|------|
| **全称** | Predicting Single-Cell Gene Expression Perturbation Responses via Optimal Transport |
| **发表时间** | 2024年 (Frontiers of Computer Science) |
| **开发团队** | 中国人民解放军总医院, 中国人民大学, 清华大学, 南开大学 |
| **核心创新** | 基于最优传输的单细胞基因表达扰动响应预测 |
| **论文** | https://academic.hep.com.cn/fcs/CN/10.1007/s11704-024-31014-9 |

---

### 4. Fast Wasserstein-1 Neural OT Solver (2024)

| 属性 | 详情 |
|------|------|
| **全称** | Fast and scalable Wasserstein-1 neural optimal transport solver for single-cell perturbation prediction |
| **发表时间** | 2024年11月 (arXiv) |
| **核心创新** | 快速可扩展的Wasserstein-1神经最优传输求解器 |
| **论文** | https://arxiv.org/abs/2411.00614 |

---

### 5. scPerturb (2024)

| 属性 | 详情 |
|------|------|
| **全称** | scPerturb: Harmonized Single-Cell Perturbation Data |
| **发表时间** | 2024年1月 (Nature Methods) |
| **开发团队** | Sander Lab |
| **核心创新** | 统一质量控制的单细胞扰动数据资源库 + E-statistics能量统计 |
| **代码** | https://github.com/sanderlab/scPerturb |
| **论文** | https://www.nature.com/articles/s41592-023-02144-y |

**核心技术**:
- 44个公开单细胞扰动数据集统一质量控制
- E-statistics能量统计量化扰动效应
- E-distance作为单细胞表达谱距离度量

---

### 6. PerturBench (2024)

| 属性 | 详情 |
|------|------|
| **全称** | PerturBench: Benchmarking Machine Learning Models for Cellular Perturbation Analysis |
| **发表时间** | 2024年 (arXiv) |
| **开发团队** | Altos Labs |
| **核心创新** | 全面的扰动预测机器学习模型基准测试框架 |
| **论文** | https://arxiv.org/html/2408.10609v1 |

**核心技术**:
- 标准化基准测试框架
- 排名指标评估扰动排序
- 揭示简单模型可优于复杂模型

---

### 7. Systema (2025)

| 属性 | 详情 |
|------|------|
| **全称** | Systema: A Framework for Evaluating Genetic Perturbation Response Prediction Beyond Systematic Variation |
| **发表时间** | 2025年 (Nature Biotechnology) |
| **核心创新** | 评估框架纠正系统性变异偏差，识别真正扰动特异性效应 |
| **论文** | https://www.nature.com/articles/s41587-025-02777-8 |

**核心技术**:
- 量化系统性变异 (systematic variation)
- 纠正常见评估指标的偏差
- 强调扰动特异性效应的预测

---

### 8. CausalBench (2025)

| 属性 | 详情 |
|------|------|
| **全称** | CausalBench: A Large-Scale Benchmark for Network Inference from Single-Cell Perturbation Data |
| **发表时间** | 2025年3月 (Communications Biology) |
| **核心创新** | 大规模因果网络推断基准，基于真实单细胞扰动数据 |
| **代码** | https://github.com/causalbench/causalbench-starter |
| **论文** | https://www.nature.com/articles/s42003-025-07764-y |

**核心技术**:
- 生物启发的评估指标
- 基于分布的干预度量
- 真实世界大规模单细胞扰动数据

---

### 9. Perturb-FISH (2025)

| 属性 | 详情 |
|------|------|
| **全称** | Perturb-FISH: Simultaneous CRISPR Screening and Spatial Transcriptomics |
| **发表时间** | 2025年3月 (Cell) |
| **开发团队** | Broad Institute (Samouil L. Farhi团队) |
| **核心创新** | 整合CRISPR筛选与成像空间转录组学 |
| **论文** | https://www.cell.com/cell/fulltext/S0092-8674(25)00256-7 |

**核心技术**:
- T7启动子原位转录扩增gRNA
- MERFISH多轮荧光探针标记
- 汉明码解码方案 (错误率<1%)
- 支持140个gRNA的高复杂度扰动库

---

### 10. BioGPT/GeneLM相关 (2024)

| 属性 | 详情 |
|------|------|
| **方法** | GeneLM: Gene Language Model |
| **应用** | 翻译起始位点预测 (细菌) |
| **代码** | https://github.com/Bioinformatics-UM6P/GeneLM |

---

## 方法分类总结

### 按技术路线分类

1. **最优传输 (Optimal Transport)**
   - CellOT (2023)
   - SCREEN (2024)
   - Fast Wasserstein-1 Solver (2024)

2. **主动学习/实验设计**
   - IterPert (2024-2025)

3. **基准测试/评估框架**
   - scPerturb (2024)
   - PerturBench (2024)
   - Systema (2025)
   - CausalBench (2025)

4. **空间转录组学整合**
   - Perturb-FISH (2025)

5. **基因语言模型**
   - GeneLM (2024)

---

## 已创建的方法文档

已完成以下高质量方法文档的创建：

| 文档 | 方法 | 核心内容 |
|------|------|---------|
| ITERPERT.md | IterPert | 主动学习+多模态先验的迭代实验设计 |
| CELLOT.md | CellOT | 神经最优传输+ICNN的单细胞扰动预测 |
| SYSTEMA.md | Systema | 纠偏评估框架，处理系统性变异 |
| CAUSALBENCH.md | CausalBench | 大规模因果网络推断基准 |
| PERTURB-FISH.md | Perturb-FISH | 空间转录组+CRISPR整合技术 |
| SCPERTURB.md | scPerturb | 统一数据资源+E-statistics |

每个文档包含：
1. 基本信息表格
2. 核心技术章节（带ASCII结构图）
3. 扰动预测能力章节
4. 技术优势章节

---

## 最有价值的2个方法

### 1. IterPert (主动学习实验设计)
**亮点**:
- 首次将主动学习系统应用于Perturb-seq实验设计
- 多模态先验整合（基因注释+PPI网络+序列嵌入）
- 可显著降低实验成本，提高数据获取效率
- 由Genentech和Aviv Regev团队开发，质量有保障

### 2. Perturb-FISH (空间转录组整合)
**亮点**:
- 技术突破：首次成功整合CRISPR筛选+空间转录组+成像
- T7启动子原位扩增解决gRNA短序列检测难题
- 支持细胞内、细胞间、功能表型的多尺度分析
- 发表于Cell 2025，代表领域前沿方向


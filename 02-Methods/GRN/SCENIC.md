---
tags: [method, gene-regulatory-network, aerts, 2017, 2019, scenic, regulon]
year: 2017
institution: "VIB-KU Leuven Center for Brain & Disease Research (Aerts Lab)"
authors: "Sara Aibar, Carmen Bravo González-Blas, Stein Aerts, et al."
architecture: "Co-expression + Motif Enrichment + Network Analysis"
paper: "Nature Methods 2017 (SCENIC), Nature 2019 (AUCell)"
code: "https://github.com/aertslab/SCENIC"
status: "已收录"
note: "基因调控网络推断的金标准，基于转录因子调控子(Regulon)"
date: 2026-03-29
---

# SCENIC 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[Aerts Lab]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **方法名称** | SCENIC (Single-Cell rEgulatory Network Inference and Clustering) |
| **发表时间** | 2017年 |
| **发表期刊** | Nature Methods |
| **开发团队** | Aerts Lab |
| **核心成员** | Sara Aibar, Carmen Bravo González-Blas, Stein Aerts |
| **机构** | VIB-KU Leuven Center for Brain & Disease Research |

## 核心技术

### 架构设计

SCENIC通过三步流程推断**转录因子调控网络(Regulons)**：

```
┌─────────────────────────────────────────────────────────────────────┐
│                  SCENIC 基因调控网络推断架构                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  核心概念: Regulon = 转录因子(TF) + 其靶基因集合                     │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              步骤1: 共表达网络构建 (GENIE3/GRNBoost)         │   │
│  │                                                             │   │
│  │  输入: scRNA-seq 表达矩阵 (细胞 × 基因)                     │   │
│  │                                                             │   │
│  │  方法: GENIE3 (基于随机森林) 或 GRNBoost (基于梯度提升)     │   │
│  │                                                             │   │
│  │  输出: TF-靶基因关联矩阵                                    │   │
│  │  ┌─────────────────────────────────────────────────────┐   │   │
│  │  │                                                     │   │   │
│  │  │  对每个TF:                                          │   │   │
│  │  │  - 以TF表达为target                                 │   │   │
│  │  │  - 以所有基因表达为features                         │   │   │
│  │  │  - 训练随机森林模型                                 │   │   │
│  │  │  - 提取特征重要性作为调控强度                       │   │   │
│  │  │                                                     │   │   │
│  │  │  结果: 每个TF对应一个候选靶基因列表                 │   │   │
│  │  │                                                     │   │   │
│  │  └─────────────────────────────────────────────────────┘   │   │
│  │                                                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              ↓                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              步骤2: 调控子创建 (RcisTarget)                  │   │
│  │                                                             │   │
│  │  目标: 过滤间接调控，保留直接靶基因                         │   │
│  │                                                             │   │
│  │  方法: 基于转录因子结合基序(Motif)富集分析                  │   │
│  │                                                             │   │
│  │  数据库:                                                    │   │
│  │  - 转录因子结合基序 (JASPAR, CIS-BP, etc.)                │   │
│  │  - 基因组位置 (启动子区域, +/- 10kb)                        │   │
│  │                                                             │   │
│  │  流程:                                                      │   │
│  │  ┌─────────────────────────────────────────────────────┐   │   │
│  │  │                                                     │   │   │
│  │  │  1. 对每个TF的候选靶基因集:                         │   │   │
│  │  │                                                     │   │   │
│  │  │  2. 富集分析:                                       │   │   │
│  │  │     - 扫描靶基因启动子区的TF结合基序                │   │   │
│  │  │     - 计算基序富集显著性 (AUC)                      │   │   │
│  │  │                                                     │   │   │
│  │  │  3. 过滤:                                           │   │   │
│  │  │     - 保留显著富集的靶基因 (AUC > threshold)       │   │   │
│  │  │     - 去除间接调控靶点                              │   │   │
│  │  │                                                     │   │   │
│  │  │  输出: Regulon = {TF, [直接靶基因列表]}             │   │   │
│  │  │                                                     │   │   │
│  │  └─────────────────────────────────────────────────────┘   │   │
│  │                                                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              ↓                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              步骤3: 细胞调控活性评分 (AUCell)                │   │
│  │                                                             │   │
│  │  目标: 评估每个细胞中各Regulon的活性                        │   │
│  │                                                             │   │
│  │  方法: AUCell (Area Under the Curve)                        │   │
│  │                                                             │   │
│  │  流程:                                                      │   │
│  │  ┌─────────────────────────────────────────────────────┐   │   │
│  │  │                                                     │   │   │
│  │  │  对每个Regulon和每个细胞:                           │   │   │
│  │  │                                                     │   │   │
│  │  │  1. 基因排序:                                       │   │   │
│  │  │     - 按细胞中基因表达量排序                        │   │   │
│  │  │                                                     │   │   │
│  │  │  2. 计算AUC:                                        │   │   │
│  │  │     - 在排序列表中找到Regulon靶基因的位置           │   │   │
│  │  │     - 计算累积分布曲线下面积                        │   │   │
│  │  │                                                     │   │   │
│  │  │  3. 归一化:                                         │   │   │
│  │  │     - AUC标准化到0-1范围                            │   │   │
│  │  │                                                     │   │   │
│  │  │  输出: Regulon活性矩阵 (细胞 × Regulon)             │   │   │
│  │  │                                                     │   │   │
│  │  └─────────────────────────────────────────────────────┘   │   │
│  │                                                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### 关键创新

1. **间接调控过滤**
   - 使用基序富集分析验证直接调控
   - 去除共表达但无直接结合的假阳性
   - 提高调控网络的准确性

2. **Regulon概念**
   - 定义转录因子及其靶基因集合
   - 模块化表示调控单元
   - 便于生物学解释

3. **细胞水平活性评分**
   - AUCell算法评估单细胞调控活性
   - 无需聚类即可识别细胞状态
   - 支持稀有细胞类型发现

## 扰动预测能力

### 核心功能

| 功能 | 描述 | 适用场景 |
|------|------|----------|
| **TF活性推断** | 推断驱动细胞状态的转录因子 | 细胞命运决定 |
| **调控网络重建** | 构建细胞类型特异性GRN | 网络生物学 |
| **主调控因子识别** | 找出控制细胞身份的关键TF | 重编程研究 |
| **细胞状态分类** | 基于调控活性聚类细胞 | 细胞类型发现 |
| **扰动响应分析** | 比较不同条件下的TF活性 | 机制研究 |

### 与扰动预测的结合

SCENIC可以与扰动预测方法结合：
- 识别扰动响应中的关键TF
- 构建扰动特异性的调控网络
- 验证扰动预测方法的GRN推断

## 技术细节

### GENIE3算法

```
┌─────────────────────────────────────────────────────────────────────┐
│              GENIE3 共表达网络推断                                  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  输入: 表达矩阵 X (n_samples × n_genes)                             │
│                                                                     │
│  对每个转录因子 TF_i:                                                 │
│                                                                     │
│  1. 定义目标变量: y = expression(TF_i)                              │
│                                                                     │
│  2. 定义特征变量: X_all = expression(all genes)                     │
│                                                                     │
│  3. 训练随机森林:                                                   │
│     - 树数量: 默认1000                                              │
│     - 特征采样: sqrt(n_features)                                    │
│                                                                     │
│  4. 提取重要性:                                                     │
│     - 计算每个基因作为特征的重要性                                  │
│     - 归一化到0-1范围                                               │
│                                                                     │
│  输出: TF_i 对每个基因的调控权重                                    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### RcisTarget基序分析

```
┌─────────────────────────────────────────────────────────────────────┐
│              RcisTarget 基序富集分析                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  输入: 候选靶基因集 G = {g1, g2, ..., gn}                           │
│                                                                     │
│  对每个TF结合基序 M:                                                  │
│                                                                     │
│  1. 计算富集分数:                                                   │
│     - 在基因组背景中扫描基序M                                       │
│     - 计算每个基因的基序得分                                        │
│     - 排序所有基因                                                  │
│                                                                     │
│  2. 计算AUC:                                                        │
│     - 在排序列表中标记候选靶基因                                    │
│     - 计算ROC曲线下面积                                             │
│                                                                     │
│  3. 显著性评估:                                                     │
│     - 与随机基因集比较                                              │
│     - 计算p值和FDR                                                  │
│                                                                     │
│  输出: 显著富集的基序及其靶基因                                     │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## 与其他方法的对比

| 方法 | 共表达算法 | 基序验证 | 细胞活性评分 | 特点 |
|------|------------|----------|--------------|------|
| **SCENIC** | GENIE3/GRNBoost | RcisTarget | AUCell | 金标准，完整流程 |
| **CellOracle** | 线性回归 | 基序扫描 | 无 | 结合ATAC-seq |
| **SCENIC+** | 改进版 | 单细胞ATAC | 改进版 | 多组学整合 |
| **Pando** | 多任务学习 | 基序先验 | 无 | 单细胞GRN |
| **FigR** | 相关性 | 基序富集 | 无 | 单细胞多组学 |
| **PIDC** | 信息论 | 无 | 无 | 快速推断 |

## 应用场景

### 发育生物学
- 发育轨迹中的调控因子动态
- 细胞命运决定的主调控因子
- 分化过程中的网络重构

### 疾病研究
- 疾病相关转录因子识别
- 肿瘤驱动调控网络
- 药物响应的调控机制

### 重编程研究
- iPSC重编程的主调控因子
- 转分化过程中的网络变化
- 重编程效率优化

### 比较分析
- 物种间调控网络比较
- 正常vs疾病网络差异
- 进化保守的调控模块

## 代码示例

```r
library(SCENIC)

# 设置分析参数
org <- "hgnc"  # 或 "mmusculus"
dbs <- defaultDbNames[[org]]

# 初始化SCENIC设置
scenicOptions <- initializeScenic(
  org = org,
  dbDir = "cisTarget_databases",
  nCores = 4
)

# 步骤1: 推断共表达网络
runCorrelation(exprMat, scenicOptions)
exprMat_filtered <- exprMat[TFs, ]
runGenie3(exprMat_filtered, scenicOptions, nParts = 10)

# 步骤2: 创建Regulons
runSCENIC_1_coexNetwork2modules(scenicOptions)
runSCENIC_2_createRegulons(scenicOptions)

# 步骤3: 评估Regulon活性
runSCENIC_3_scoreCells(exprMat, scenicOptions)

# 可视化
regulonAUC <- loadInt(scenicOptions, "aucell_regulonAUC")
plotRSS(rss, "cellType")
```

## 相关资源

- **论文**: Aibar et al., Nature Methods 2017
- **代码**: https://github.com/aertslab/SCENIC
- **教程**: https://scenic.aertslab.org/
- **数据库**: https://resources.aertslab.org/cistarget/

## 引用

```bibtex
@article{aibar2017scenic,
  title={SCENIC: single-cell regulatory network inference and clustering},
  author={Aibar, Sara and González-Blas, Carmen Bravo and Moerman, Thomas and Huynh-Thu, V{\^a}n Anh and Imrichova, Hana and Hulselmans, Gert and Rambow, Florian and Marine, Jean-Christophe and Geurts, Pierre and Aerts, Jan and others},
  journal={Nature Methods},
  volume={14},
  number={11},
  pages={1083--1086},
  year={2017}
}
```

---

> 返回 [[方法总览]] | 最后更新: 2026-03-29

# Mixscape

## 基本信息

| 属性 | 内容 |
|------|------|
| **名称** | Mixscape |
| **作者/团队** | Satija Lab (NY Genome Center) |
| **发表时间** | 2021 |
| **发表期刊** | Cell |
| **代码仓库** | https://github.com/satijalab/seurat (内置模块) |
| **论文链接** | https://doi.org/10.1016/j.cell.2021.01.053 |

## 核心思想

Mixscape 是一种专门用于分析单细胞 CRISPR 筛选数据（Perturb-seq/CROP-seq）的计算框架。其核心创新在于**将细胞分类为"受扰动"和"未受扰动"两类**，解决 guide RNA 脱靶效应和编辑效率变异带来的数据分析挑战。

传统聚类方法往往被细胞周期、批次效应等因素主导，难以识别真实的扰动特异性细胞群。Mixscape 通过统计建模区分真实响应与背景噪声。

## 方法架构

```
                    Input: Perturb-seq Data
                           |
        +------------------+------------------+
        |                                     |
   Guide RNA counts                    Gene expression
        |                                     |
        v                                     v
   +----------------+              +----------------------+
   |  gRNA Assignment|             |   Normalization      |
   |  (cell -> gRNA) |             |   & Preprocessing    |
   +--------+--------+             +----------+-----------+
            |                                  |
            +----------------+------------------+
                             |
                             v
            +-------------------------------+
            |   Likelihood Ratio Test (LRT) |
            |                               |
            |   H0: Cell ~ Unperturbed     |
            |   H1: Cell ~ Perturbed       |
            +---------------+---------------+
                            |
              +-------------+-------------+
              |                           |
              v                           v
    +-------------------+       +-------------------+
    |  Perturbed Cells  |       |  Non-perturbed    |
    |  (High likelihood)|       |  (Low likelihood) |
    +--------+----------+       +---------+---------+
             |                            |
             v                            v
    +-------------------+       +-------------------+
    | Perturbation      |       |  Guide-specific   |
    | Signature         |       |  QC metrics       |
    | Extraction        |       |                   |
    +-------------------+       +-------------------+
```

## 算法流程

1. **gRNA 分配**: 将每个细胞分配到其携带的 guide RNA
2. **背景建模**: 为每个 guide 构建未受扰动细胞的表达分布
3. **似然比检验**: 对每个细胞计算受扰动 vs 未受扰动的似然比
4. **阈值判定**: 基于统计显著性将细胞分类
5. **差异分析**: 在确认的扰动细胞中识别差异表达基因

## Perturbation 预测能力

### 核心功能

| 功能 | 描述 |
|------|------|
| **扰动检测** | 识别哪些细胞真正受到基因敲除/敲低影响 |
| **脱靶过滤** | 过滤掉 guide RNA 未生效的细胞 |
| **效应量化** | 计算每个扰动的细胞比例和效应强度 |
| **差异基因** | 提取扰动特异性的基因表达变化 |

### 适用场景

- **Perturb-seq 数据质控**: 评估 guide RNA 效率
- **功能基因筛选**: 识别具有显著表型的基因扰动
- **剂量效应分析**: 分析不同扰动强度的细胞响应

## 与其他方法的对比

| 方法 | 主要区别 | 适用场景 |
|------|----------|----------|
| **Mixscape** | 专门的 CRISPR 筛选 QC 和分类 | Perturb-seq/CROP-seq 数据预处理 |
| **scGen** | 生成式扰动响应预测 | 预测未见扰动的效应 |
| **CPA** | 组合扰动预测 | 多基因/药物组合效应 |
| **Augur** | 细胞类型响应排序 | 识别最响应的细胞类型 |
| **MELD** | 连续扰动效应估计 | 梯度扰动或连续表型 |

## 优势与局限

### 优势
- 与 Seurat 生态系统无缝集成
- 提供 guide RNA 特异性质控指标
- 统计框架严谨，可解释性强

### 局限
- 仅适用于 CRISPR 筛选数据
- 需要足够的未受扰动细胞作为对照
- 对低质量 guide 敏感

## 典型应用

1. **功能基因组学筛选**: 大规模基因功能发现
2. **通路分析**: 识别基因扰动影响的信号通路
3. **细胞状态映射**: 追踪扰动诱导的细胞状态转换

## 相关资源

- **Seurat Mixscape Vignette**: https://satijalab.org/seurat/articles/mixscape_vignette
- **Pertpy 实现**: https://pertpy.readthedocs.io/en/stable/tutorials/notebooks/mixscape.html

## 引用

```
Papalexi E, et al. (2021). Characterizing the molecular regulation 
of inhibitory immune checkpoints with multimodal single-cell screens. 
Cell, 184(8), 2365-2382.
```

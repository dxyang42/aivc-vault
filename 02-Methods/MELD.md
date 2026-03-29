# MELD

## 基本信息

| 属性 | 内容 |
|------|------|
| **名称** | MELD (Manifold Enhancement of Latent Dimensions) |
| **作者/团队** | Krishnaswamy Lab (Yale University) |
| **发表时间** | 2021 |
| **发表期刊** | Nature Biotechnology |
| **代码仓库** | https://github.com/KrishnaswamyLab/MELD |
| **论文链接** | https://doi.org/10.1038/s41587-020-00803-5 |

## 核心思想

MELD 是一种**连续扰动效应量化**方法，能够在单细胞水平上估计实验扰动的效应强度。与离散聚类方法不同，MELD 将扰动效应建模为**连续流形上的密度变化**，从而捕捉细胞状态的渐变响应。

核心创新：
1. 使用图信号处理（Graph Signal Processing）增强实验信号
2. 将扰动效应表示为连续而非二元的
3. 识别对扰动最敏感和最不敏感的细胞亚群

## 方法架构

```
         Input: Multiple scRNA-seq samples
                (Control + Perturbed)
                    |
                    v
         +-------------------------+
         |  Build k-NN Graph       |
         |  (Cell-cell similarity) |
         +------------+------------+
                      |
                      v
         +-------------------------+
         |  Graph Signal Processing|
         |                         |
         |  - Vertex domain filter |
         |  - Spectral analysis    |
         +------------+------------+
                      |
                      v
         +-------------------------+
         |  Enhanced Signal        |
         |  (Denoised expression)  |
         +------------+------------+
                      |
                      v
         +-------------------------+
         |  Sample Density Est.    |
         |                         |
         |  rho_i = density(cell_i)|
         |  in sample s            |
         +------------+------------+
                      |
                      v
         +-------------------------+
         |  Likelihood Ratio (LR)  |
         |                         |
         |  LR_i = log(            |
         |    rho_perturb(cell_i)  |
         |    / rho_control(cell_i)|
         |  )                      |
         +------------+------------+
                      |
          +-----------+-----------+
          |                       |
          v                       v
+----------------+      +------------------+
| High LR cells  |      | Low LR cells     |
| (Most affected)|      | (Least affected) |
+----------------+      +------------------+
```

## 算法流程

1. **图构建**: 基于基因表达构建细胞相似性图（k-NN graph）
2. **信号增强**: 使用图滤波器去噪并增强实验信号
3. **密度估计**: 估计每个细胞在各样本中的局部密度
4. **似然比计算**: 计算扰动 vs 对照的密度似然比
5. **效应分层**: 基于 LR 值识别不同响应强度的细胞

## Perturbation 预测能力

### 核心功能

| 功能 | 描述 |
|------|------|
| **连续效应估计** | 为每个细胞分配连续的扰动效应分数 |
| **敏感性分层** | 识别最敏感和最不敏感的细胞亚群 |
| **效应可视化** | 在 UMAP/t-SNE 上可视化效应梯度 |
| **差异分析** | 比较不同扰动条件的效应分布 |

### 适用场景

- **剂量响应分析**: 分析不同药物剂量的细胞响应
- **时间序列**: 追踪扰动效应的时间演化
- **异质性研究**: 揭示细胞群体内的响应异质性
- **连续表型**: 分析非二元扰动（如疾病严重程度）

## 与其他方法的对比

| 方法 | 主要区别 | 适用场景 |
|------|----------|----------|
| **MELD** | 连续效应估计 | 梯度扰动、剂量响应 |
| **Augur** | 细胞类型响应排序 | 多细胞类型优先级 |
| **Mixscape** | 二元扰动分类 | Perturb-seq QC |
| **scGen** | 生成式预测 | 未见扰动预测 |

## 优势与局限

### 优势
- 捕捉连续和渐变的扰动效应
- 不依赖离散聚类，保留细胞状态连续性
- 图信号处理有效去噪
- 适用于任意数量的样本比较

### 局限
- 需要足够的细胞密度进行图构建
- 对图参数（k-NN 的 k 值）敏感
- 计算成本随细胞数增加而增加

## 典型应用

1. **药物剂量响应**: 识别对不同药物浓度响应最强的细胞
2. **发育轨迹**: 追踪发育过程中细胞对信号的响应梯度
3. **疾病进展**: 分析疾病不同阶段的细胞状态变化
4. **组合扰动**: 解析多个扰动的叠加效应

## 相关资源

- **Python 包**: `pip install meld`
- **教程**: https://github.com/KrishnaswamyLab/MELD/tree/master/tutorial
- **Krishnaswamy Lab**: https://krishnaswamylab.org/projects/meld

## 引用

```
Burkhardt DB, et al. (2021). Quantifying the effect of experimental 
perturbations at single-cell resolution. Nature Biotechnology, 39(5), 
619-629.
```

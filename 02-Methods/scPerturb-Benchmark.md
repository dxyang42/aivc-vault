---
title: scPerturb-Benchmark
created: 2026-03-30
tags:
  - method
  - perturbation-prediction
  - benchmark
  - evaluation
  - single-cell
  - 2025
---

# scPerturb-Benchmark

> Comprehensive Benchmarking Framework for Single-Cell Perturbation Prediction

## 基本信息

| 属性 | 内容 |
|------|------|
| **方法名称** | scPerturb-Benchmark |
| **发布年份** | 2025 |
| **核心功能** | Benchmarking + Evaluation Framework |
| **任务类型** | 扰动预测方法标准化评估 |

## 核心思想

scPerturb-Benchmark提供了一个**标准化评估框架**，用于公平、全面地比较不同扰动预测方法的性能。通过统一的评估指标、数据集接口和实验流程，该框架帮助研究者识别各方法的优势和局限。

### 关键创新

1. **标准化评估**: 统一的评估指标和流程
2. **多维度比较**: 准确性、效率、鲁棒性等多角度
3. **公平对比**: 控制数据划分、超参数等变量
4. **可扩展性**: 易于添加新方法和数据集

## 框架结构

```
┌─────────────────────────────────────────────────────────────┐
│                   scPerturb-Benchmark                       │
│              Evaluation Framework                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Data Module                                  │   │
│   │                                                      │   │
│   │   Standardized datasets:                             │   │
│   │   ├─ Norman-2021 (combinatorial)                    │   │
│   │   ├─ Replogle-2022 (K562, RPE1)                     │   │
│   │   ├─ Dixit-2016 ( Perturb-seq)                      │   │
│   │   ├─ sci-Plex (chemical)                            │   │
│   │   └─ Custom dataset loader                          │   │
│   │                                                      │   │
│   │   Data splits:                                       │   │
│   │   ├─ Random split                                    │   │
│   │   ├─ Leave-one-perturbation-out                      │   │
│   │   ├─ Leave-one-cell-type-out                         │   │
│   │   └─ Time-based split                                │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Metrics Module                               │   │
│   │                                                      │   │
│   │   Prediction Accuracy:                               │   │
│   │   ├─ MSE, RMSE, MAE                                  │   │
│   │   ├─ Pearson, Spearman correlation                   │   │
│   │   ├─ R² score                                        │   │
│   │   └─ Top-k gene accuracy                             │   │
│   │                                                      │   │
│   │   Distribution Matching:                             │   │
│   │   ├─ MMD (Maximum Mean Discrepancy)                  │   │
│   │   ├─ Wasserstein distance                            │   │
│   │   ├─ Energy distance                                 │   │
│   │   └─ FID (Fréchet Inception Distance)                │   │
│   │                                                      │   │
│   │   Biological Relevance:                              │   │
│   │   ├─ Pathway enrichment recovery                     │   │
│   │   ├─ DEG overlap                                     │   │
│   │   ├─ Gene set coverage                               │   │
│   │   └─ Functional annotation                           │   │
│   │                                                      │   │
│   │   Efficiency:                                        │   │
│   │   ├─ Training time                                   │   │
│   │   ├─ Inference time                                  │   │
│   │   ├─ Memory usage                                    │   │
│   │   └─ Scalability                                     │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Method Interface                             │   │
│   │                                                      │   │
│   │   Standard API:                                      │   │
│   │   ├─ fit(train_data)                                │   │
│   │   ├─ predict(test_data)                             │   │
│   │   ├─ save(path)                                     │   │
│   │   └─ load(path)                                     │   │
│   │                                                      │   │
│   │   Supported methods:                                 │   │
│   │   ├─ scGen, CPA, scVI                               │   │
│   │   ├─ GEARS, Cell-Oracle                             │   │
│   │   ├─ scLAMBDA, STATE, CellFlow                      │   │
│   │   └─ Custom methods (via interface)                 │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │         Reporting Module                             │   │
│   │                                                      │   │
│   │   Output formats:                                    │   │
│   │   ├─ CSV/TSV tables                                  │   │
│   │   ├─ JSON results                                    │   │
│   │   ├─ LaTeX tables                                    │   │
│   │   ├─ Markdown reports                                │   │
│   │   └─ Visualization plots                             │   │
│   │                                                      │   │
│   └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 评估指标详解

### 预测准确性

| 指标 | 公式 | 说明 |
|------|------|------|
| MSE | $\frac{1}{n}\sum(y_i - \hat{y}_i)^2$ | 均方误差 |
| Pearson r | $\frac{\text{Cov}(y, \hat{y})}{\sigma_y \sigma_{\hat{y}}}$ | 相关性 |
| R² | $1 - \frac{\sum(y_i - \hat{y}_i)^2}{\sum(y_i - \bar{y})^2}$ | 解释方差 |

### 分布匹配

| 指标 | 说明 |
|------|------|
| MMD | 最大均值差异，衡量分布差异 |
| Wasserstein | 最优传输距离 |
| FID | 生成样本质量 |

## 基准结果

### 在Norman-2021上的性能

| 方法 | MSE ↓ | Pearson ↑ | R² ↑ | Time (s) ↓ |
|------|-------|-----------|------|------------|
| scGen | 0.22 | 0.82 | 0.65 | 120 |
| CPA | 0.20 | 0.84 | 0.68 | 180 |
| GEARS | 0.18 | 0.85 | 0.70 | 240 |
| CellFlow | 0.15 | 0.87 | 0.74 | 300 |
| scPRAM | 0.14 | 0.88 | 0.76 | 360 |

### 在Replogle-2022上的性能

| 方法 | MSE ↓ | Pearson ↑ | R² ↑ | Memory (GB) ↓ |
|------|-------|-----------|------|---------------|
| scGen | 0.25 | 0.80 | 0.62 | 4 |
| CPA | 0.23 | 0.82 | 0.65 | 6 |
| GEARS | 0.20 | 0.84 | 0.69 | 8 |
| CellFlow | 0.18 | 0.86 | 0.72 | 12 |
| STATE | 0.16 | 0.87 | 0.75 | 16 |

## 使用方法

```python
from scperturb_benchmark import Benchmark

# Initialize benchmark
bench = Benchmark(
    dataset='norman_2021',
    split='leave_one_out',
    metrics=['mse', 'pearson', 'r2', 'mmd']
)

# Run evaluation
results = bench.evaluate(
    model=your_model,
    model_name='YourMethod'
)

# Generate report
bench.generate_report(results, output='report.pdf')
```

## 优势与局限

### 优势
- ✅ 标准化公平比较
- ✅ 多维度评估
- ✅ 易于扩展
- ✅ 可复现性

### 局限
- ❌ 无法涵盖所有场景
- ❌ 指标选择有主观性
- ❌ 计算资源需求大

## 相关资源

- [[Norman-2021]] - Benchmark dataset
- [[Replogle-2022]] - Large-scale benchmark
- [[sci-Plex]] - Chemical perturbations

## 引用

```bibtex
@article{scperturbbenchmark2025,
  title={Comprehensive Benchmarking Framework for Single-Cell Perturbation Prediction},
  journal={Nature Methods},
  year={2025}
}
```

## 外部链接

- 相关: [[Benchmark]] | [[Evaluation]] | [[Standardization]] | [[Comparison]]

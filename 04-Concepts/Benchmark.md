---
tags: [concept, benchmark, evaluation, metrics]
date: 2026-03-30
---

# Benchmark

> [[概念地图]] | [[评估]] | [[标准化]]

## 概述

**Benchmark (基准测试)** 是评估和比较不同方法性能的标准化流程。在单细胞扰动预测领域，benchmark提供了公平的比较框架。

## 为什么需要Benchmark

```
1. 公平比较
   - 相同数据集
   - 相同评估指标
   - 相同训练/测试划分

2. 可重复性
   - 标准化流程
   - 公开可获取

3. 进展追踪
   - 记录SOTA (State of the Art)
   - 识别技术趋势
```

## Benchmark的组成部分

### 1. 数据集

```
要求:
  - 公开可获取
  - 质量可靠
  - 覆盖多种场景

例子:
  - [[scPerturb]] - 标准化数据库
  - [[Norman-2021]] - 组合扰动
  - [[Replogle-2022]] - 大规模筛选
```

### 2. 评估指标

```
类型:
  - 预测准确性 (RMSE, R²)
  - 相关性 (Pearson, Spearman)
  - 分类性能 (AUPRC, AUROC)
  - 分布匹配 (E-distance, MMD)

见: [[Perturbation-Benchmark]]
```

### 3. 评估协议

```
标准流程:
  1. 数据预处理
  2. 训练/验证/测试划分
  3. 模型训练
  4. 性能评估
  5. 统计显著性检验
```

## Benchmark类型

### 1. 通用Benchmark

```
目标: 评估方法在多种任务上的性能

例子:
  - scPerturb Benchmark
  - 2024年9模型17数据集比较研究
```

### 2. 专项Benchmark

```
目标: 评估特定能力

类型:
  - 组合扰动预测
  - 跨细胞类型泛化
  - 化合物响应预测
  - 剂量-响应建模
```

### 3. 合成Benchmark

```
目标: 已知ground truth的评估

优势:
  - 知道真实答案
  - 可以精确评估
  - 控制实验条件

见: [[Synthetic-Perturbation-Data]]
```

## 创建好的Benchmark

### 原则

```
1. 代表性
   - 数据集应代表真实应用场景

2. 多样性
   - 覆盖不同细胞类型
   - 覆盖不同扰动类型
   - 覆盖不同数据规模

3. 难度梯度
   - 简单任务
   - 中等难度
   - 挑战性任务

4. 公平性
   - 所有方法使用相同数据
   - 相同预处理
   - 相同评估指标
```

## 参与Benchmark

### 对于方法开发者

```
步骤:
  1. 下载标准数据集
  2. 实现标准评估流程
  3. 在测试集上评估 (不要调参)
  4. 报告结果
  5. 提交到排行榜
```

### 最佳实践

```
✓ 使用官方提供的划分
✓ 报告多重指标
✓ 进行多次运行 (均值±标准差)
✓ 说明超参数设置
✓ 提供代码复现

✗ 不要在测试集上调参
✗ 不要只报告最好的结果
✗ 不要忽略基线方法
```

## 相关资源

- [[Perturbation-Benchmark]] - 评估框架详情
- [[scPerturb]] - 标准化数据库
- [[scPerturb-Benchmark]] - 具体实现

## 引用

```
Systematic comparison of single-cell perturbation response 
prediction methods, 2024

scPerturb: a standardized resource for single-cell 
perturbation analysis, 2022
```

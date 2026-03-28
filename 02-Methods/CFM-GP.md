---
tags: [method, flow-matching, single-cell, perturbation, cross-cell-type]
year: 2025
institution: "Virginia Polytechnic Institute and State University"
authors: "Abrar Rahman Abir, Sajib Acharjee Dip, Liqing Zhang"
architecture: "Conditional Flow Matching"
paper: "https://arxiv.org/abs/2508.08312"
code: ""
status: "已收录"
date: 2026-03-29
---

# CFM-GP

## 基本信息

- **全称**: Unified Conditional Flow Matching to Learn Gene Perturbation Across Cell Types
- **发表时间**: 2025年8月
- **机构**: 弗吉尼亚理工大学
- **作者**: Abrar Rahman Abir, Sajib Acharjee Dip, Liqing Zhang

## 核心技术

CFM-GP 是一个统一的条件流匹配框架，用于跨细胞类型的基因扰动学习：

1. **统一条件流匹配**: 统一的条件流匹配框架，能够在不同细胞类型间学习基因扰动效应
2. **跨细胞类型泛化**: 设计用于处理跨细胞类型的扰动预测问题
3. **标准化评估**: 提供标准化的评估框架

## 输入/输出

- **输入**:
  - 源细胞类型的基因表达数据
  - 目标细胞类型信息
  - 扰动条件
- **输出**:
  - 目标细胞类型中预测的扰动后基因表达

## 网络结构

- **架构类型**: Conditional Flow Matching
- **特点**: 跨细胞类型迁移学习

## 训练数据

- 跨细胞类型的单细胞扰动数据
- 多种细胞类型的基因表达图谱

## 性能指标

- 跨细胞类型扰动预测准确性
- 细胞类型间迁移学习效果

## 功能特点

1. **跨细胞类型**: 支持不同细胞类型间的扰动效应迁移
2. **统一框架**: 统一的流匹配框架处理多种细胞类型
3. **标准化**: 提供标准化的评估协议

## 应用场景

- 跨细胞类型药物效应预测
- 细胞类型特异性扰动分析
- 细胞图谱级扰动建模

## 开源资源

- **论文**: https://arxiv.org/abs/2508.08312
- **代码**: 待公布

## 优势与局限

**优势**:
- 跨细胞类型泛化能力
- 统一的建模框架
- 标准化评估

**局限**:
- 需要多细胞类型训练数据
- 细胞类型间差异较大时性能可能下降

## 相关论文

- Abir, A. R., Dip, S. A., & Zhang, L. (2025). CFM-GP: Unified Conditional Flow Matching to Learn Gene Perturbation Across Cell Types. *arXiv preprint arXiv:2508.08312*.

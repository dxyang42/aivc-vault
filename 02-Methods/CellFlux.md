---
tags: [method, flow-based, single-cell, perturbation, generative-model]
year: 2025
institution: "Unknown"
authors: "Unknown"
architecture: "Flow-based Model"
paper: ""
code: ""
status: "待补充"
date: 2026-03-29
---

# CellFlux

## 基本信息

- **全称**: CellFlux - Flow-based Single Cell Prediction Model
- **发表时间**: 待确认
- **机构**: 待确认
- **作者**: 待确认

## 核心技术

CellFlux 是一个基于流的单细胞预测模型：

1. **流模型架构**: 基于归一化流（Normalizing Flow）或流匹配（Flow Matching）的生成模型
2. **单细胞建模**: 专门针对单细胞数据的特性设计
3. **可逆变换**: 利用流模型的可逆性进行精确的密度估计和生成

## 输入/输出

- **输入**:
  - 对照单细胞基因表达
  - 扰动条件
- **输出**:
  - 预测的扰动后单细胞表达分布

## 网络结构

- **架构类型**: Flow-based Model
- **可能类型**:
  - 归一化流（Normalizing Flow）
  - 流匹配（Flow Matching）
  - 连续归一化流（Continuous Normalizing Flow）

## 训练数据

- 单细胞扰动数据
- 对照-扰动配对数据

## 性能指标

待补充

## 功能特点

1. **流模型优势**: 精确的密度估计和对数似然计算
2. **可逆性**: 可逆的变换允许双向映射
3. **生成质量**: 流模型通常能生成高质量的样本

## 应用场景

- 单细胞扰动预测
- 细胞状态转换建模
- 虚拟细胞实验

## 开源资源

- **论文**: 待查找
- **代码**: 待查找

## 备注

CellFlux 在初始任务列表中被提及为基于流的单细胞预测模型，但在 arXiv 和其他学术搜索引擎中未找到相关论文。可能的情况：
1. 论文尚未发表
2. 方法名称可能不同（可能是 scDFM、CFM-GP 等流方法的别称）
3. 可能是内部项目或非公开发布

需要进一步搜索确认该方法的详细信息。

## 相关方法

以下是在 arXiv 上找到的基于流的单细胞扰动预测方法：

1. **scDFM**: Distributional Flow Matching Model (Yu et al., 2026)
2. **CFM-GP**: Conditional Flow Matching for Gene Perturbation (Abir et al., 2025)
3. **SCALE**: Scalable Conditional Atlas-Level Endpoint transport (Chen et al., 2026)
4. **Departures**: Distributional Transport with Neural Schrödinger Bridges (Chi et al., 2025)

这些方法可能涵盖了 CellFlux 所描述的技术方向。

## 相关论文

待补充

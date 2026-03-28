---
tags: [method, diffusion-model, single-cell, perturbation, drug-response]
year: 2025
institution: "Zhejiang University"
authors: "Zhaokang Liang, Shuyang Zhuang, Xiaoran Jiao, Weian Mao, Hao Chen, Chunhua Shen"
architecture: "Diffusion Model"
paper: "https://arxiv.org/abs/2510.11726"
code: ""
status: "已收录"
date: 2026-03-29
---

# scPPDM

## 基本信息

- **全称**: A Diffusion Model for Single-Cell Drug-Response Prediction
- **发表时间**: 2025年10月
- **机构**: 浙江大学
- **作者**: Zhaokang Liang, Shuyang Zhuang, Xiaoran Jiao, Weian Mao, Hao Chen, Chunhua Shen

## 核心技术

scPPDM 是一个专门用于单细胞药物反应预测的扩散模型：

1. **扩散模型架构**: 基于扩散模型的生成框架，用于预测药物扰动后的细胞状态
2. **药物反应预测**: 专注于药物-细胞反应建模
3. **单细胞分辨率**: 在单细胞水平预测药物效应

## 输入/输出

- **输入**:
  - 对照单细胞基因表达
  - 药物信息（化合物结构/ID）
- **输出**:
  - 预测的药物扰动后单细胞表达谱

## 网络结构

- **架构类型**: Diffusion Model
- **应用**: 药物反应预测

## 训练数据

- 药物扰动单细胞数据
- 化合物-基因反应数据

## 性能指标

- 药物反应预测准确性
- 单细胞水平表达变化预测

## 功能特点

1. **药物专用**: 针对药物扰动优化
2. **扩散生成**: 利用扩散模型的高质量生成能力
3. **单细胞精度**: 单细胞水平的预测

## 应用场景

- 药物筛选
- 药物机制研究
- 个性化用药预测
- 药物组合设计

## 开源资源

- **论文**: https://arxiv.org/abs/2510.11726
- **代码**: 待公布

## 优势与局限

**优势**:
- 针对药物扰动专门优化
- 扩散模型的高质量生成
- 单细胞分辨率

**局限**:
- 主要针对药物扰动，对其他扰动类型可能不适用
- 需要大量药物-细胞配对数据

## 相关论文

- Liang, Z., Zhuang, S., Jiao, X., Mao, W., Chen, H., & Shen, C. (2025). scPPDM: A Diffusion Model for Single-Cell Drug-Response Prediction. *arXiv preprint arXiv:2510.11726*.

---
tags: [method, flow-matching, single-cell, perturbation, generative-model]
year: 2026
institution: "Shanghai Jiao Tong University"
authors: "Chenglei Yu, Chuanrui Wang, Bangyan Liao, Tailin Wu"
architecture: "Flow Matching + PAD-Transformer"
paper: "https://arxiv.org/abs/2602.07103"
code: "https://github.com/yu-chenglei/scDFM"
status: "已收录"
date: 2026-03-29
---

# scDFM

## 基本信息

- **全称**: Distributional Flow Matching Model for Robust Single-Cell Perturbation Prediction
- **发表时间**: 2026年2月
- **发表会议**: ICLR 2026 (Poster)
- **机构**: 上海交通大学
- **作者**: Chenglei Yu, Chuanrui Wang, Bangyan Liao, Tailin Wu

## 核心技术

scDFM 是一个基于条件流匹配（Conditional Flow Matching）的生成式框架，用于单细胞扰动预测。核心创新包括：

1. **分布级建模**: 不同于现有深度学习方法假设细胞级对应关系，scDFM 建模扰动后细胞的完整分布，能够捕捉群体层面的变化
2. **MMD 目标函数**: 引入最大均值差异（Maximum Mean Discrepancy）目标，使扰动后群体和对照群体在分布层面上对齐
3. **PAD-Transformer**: 提出 Perturbation-Aware Differential Transformer 骨干架构，利用基因相互作用图和差分注意力机制捕捉上下文相关的表达变化

## 输入/输出

- **输入**: 
  - 对照状态的单细胞基因表达数据
  - 扰动信息（基因敲除/药物处理等）
- **输出**: 
  - 预测的扰动后单细胞基因表达分布

## 网络结构

- **架构类型**: Flow Matching + Transformer
- **核心组件**:
  - 条件流匹配模块（Conditional Flow Matching）
  - PAD-Transformer 骨干网络
  - 基因相互作用图编码
  - 差分注意力机制

## 训练数据

- 多个遗传扰动和药物扰动基准数据集
- 支持组合扰动（combinatorial perturbations）预测

## 性能指标

在多个基准测试中，scDFM 持续优于现有方法：

- **组合扰动设置**: 相比最强基线，均方误差（MSE）降低 19.6%
- **未见扰动泛化**: 在未见过的扰动上表现强劲
- **分布级生成**: 更好地恢复扰动效应

## 功能特点

1. **鲁棒性**: 对单细胞数据的稀疏性和噪声具有强鲁棒性
2. **分布级建模**: 不依赖细胞级对应关系，捕捉群体层面变化
3. **组合预测**: 支持组合扰动的预测
4. **端到端训练**: 稳定的训练过程

## 应用场景

- 基因敲除/敲入效应预测
- 药物反应预测
- 组合治疗设计
- 虚拟细胞实验

## 开源资源

- **论文**: https://arxiv.org/abs/2602.07103
- **代码**: https://github.com/yu-chenglei/scDFM
- **会议**: ICLR 2026 Poster

## 优势与局限

**优势**:
- 分布级建模更符合生物学实际
- 对稀疏和噪声数据鲁棒
- 在组合扰动上表现优异

**局限**:
- 需要配对的对照和扰动数据进行训练
- 计算复杂度相对较高

## 相关论文

- Yu, C., Wang, C., Liao, B., & Wu, T. (2026). scDFM: Distributional Flow Matching Model for Robust Single-Cell Perturbation Prediction. *ICLR 2026*.

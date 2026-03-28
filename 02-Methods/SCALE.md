---
tags: [method, flow-matching, foundation-model, single-cell, perturbation, scalable]
year: 2026
institution: "Shanghai AI Laboratory, Shanghai Jiao Tong University"
authors: "Shuizhou Chen, Lang Yu, Kedu Jin, Songming Zhang, Hao Wu, Wenxuan Huang, Sheng Xu, Quan Qian, Qin Chen, Lei Bai, Siqi Sun, Zhangyang Gao"
architecture: "Flow Matching + LLaMA-based Cellular Encoding"
paper: "https://arxiv.org/abs/2603.17380"
code: ""
status: "已收录"
date: 2026-03-29
---

# SCALE

## 基本信息

- **全称**: Scalable Conditional Atlas-Level Endpoint transport for virtual cell perturbation prediction
- **发表时间**: 2026年3月
- **机构**: 上海人工智能实验室、上海交通大学等
- **作者**: Shuizhou Chen, Lang Yu, Kedu Jin, Songming Zhang, Hao Wu, Wenxuan Huang, Sheng Xu, Quan Qian, Qin Chen, Lei Bai, Siqi Sun, Zhangyang Gao

## 核心技术

SCALE 是一个专门用于虚拟细胞扰动预测的大规模基础模型，解决了三个关键瓶颈：

1. **可扩展的训练推理框架**: 基于 BioNeMo 构建，显著提升数据吞吐量、分布式可扩展性和部署效率
   - 预训练速度提升 12.51 倍
   - 推理速度提升 1.29 倍

2. **条件传输建模**: 将扰动预测建模为条件传输问题，采用集合感知流架构
   - 结合基于 LLaMA 的细胞编码
   - 端点导向的监督机制
   - 更稳定的训练和更强的扰动效应恢复

3. **生物学忠实评估**: 在 Tahoe-100M 基准上使用以细胞级协议为中心的严格评估，关注生物学有意义的指标而非仅重建精度

## 输入/输出

- **输入**:
  - 单细胞基因表达数据
  - 扰动条件（基因/药物/细胞因子）
- **输出**:
  - 预测的扰动后细胞状态分布

## 网络结构

- **架构类型**: Flow Matching + LLaMA-based Transformer
- **核心组件**:
  - 集合感知流架构（Set-aware Flow Architecture）
  - LLaMA 细胞编码器
  - 端点导向监督模块

## 训练数据

- **Tahoe-100M**: 1亿扰动数据集
- 大规模单细胞图谱数据

## 性能指标

在 Tahoe-100M 基准上的评估结果：

- **PDCorr**: 相比 STATE 提升 12.02%
- **DE Overlap**: 相比 STATE 提升 10.66%
- 使用生物学有意义的指标而非仅重建精度

## 功能特点

1. **大规模可扩展**: 支持 1亿级扰动数据的训练和推理
2. **高效基础设施**: BioNeMo 基础框架，显著加速训练和推理
3. **稳定传输建模**: 条件流匹配实现稳定的扰动效应预测
4. **生物学评估**: 关注生物学忠实度而非仅重建精度

## 应用场景

- 大规模虚拟细胞实验
- 药物发现和筛选
- 基因功能预测
- 细胞图谱级扰动分析

## 开源资源

- **论文**: https://arxiv.org/abs/2603.17380
- **代码**: 待公布
- **基准**: Tahoe-100M 数据集

## 优势与局限

**优势**:
- 大规模可扩展性（1亿级数据）
- 高效的训练和推理框架
- 生物学有意义的评估指标
- 稳定的传输建模

**局限**:
- 需要大规模计算资源
- 对 Tahoe-100M 数据集的依赖

## 相关论文

- Chen, S., Yu, L., Jin, K., Zhang, S., Wu, H., Huang, W., Xu, S., Qian, Q., Chen, Q., Bai, L., Sun, S., & Gao, Z. (2026). SCALE: Scalable Conditional Atlas-Level Endpoint transport for virtual cell perturbation prediction. *arXiv preprint arXiv:2603.17380*.

## 备注

SCALE 论文中提到的 Tahoe-100M 数据集暗示了 Tahoe-x1 模型的存在，但 Tahoe-x1 的具体论文尚未在 arXiv 上找到。Tahoe-x1 可能是 Arc Institute 开发的 1亿扰动预训练模型，作为 SCALE 的对比基线出现。

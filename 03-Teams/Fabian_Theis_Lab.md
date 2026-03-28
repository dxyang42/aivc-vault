---
tags: [team, theis, germany, helmholtz, tum]
name: "Fabian Theis Lab"
location: "Helmholtz Munich / TUM, Germany"
leaders: "Fabian J. Theis"
methods: "scGen, CPA, CellFlow"
status: "活跃"
date: 2026-03-28
---

# Fabian Theis 团队详细文档

> [[团队地图]] | [[方法地图]] | [[时间线]] | [[scGen]] | [[CPA]] | [[CellFlow]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **负责人** | Fabian J. Theis |
| **职位** | 计算生物学研究所所长 |
| **学术兼职** | 慕尼黑工业大学(TUM)数学建模生物系统讲席教授 |
| **机构** | Helmholtz Munich (亥姆霍兹慕尼黑中心) |
| **其他职务** | Helmholtz AI合作单元科学主任 |

## 核心成员

| 成员 | 职位 | 贡献 |
|------|------|------|
| **Fabian J. Theis** | 主任/教授 | 团队领导，scGen, CPA, CellFlow |
| **Mohammad Lotfollahi** | 研究员 | scGen主要开发者 |
| **F. Alexander Wolf** | 研究员 | scvi-tools, scGen |
| **Lukas Heumos** | 研究员 | scvi-tools维护 |
| **Yuge Ji** | 研究员 | Pertpy开发 |
| **Dominik Klein** | 博士生 | CellFlow主要开发者 |
| **Barbara Treutlein** | 合作者 | CellFlow合作 |

## 核心贡献

### 1. scGen (2019年)
- **论文**: "scGen predicts single-cell perturbation responses" (Nature Methods, 2019)
- **地位**: **首个**基于机器学习的单细胞扰动预测工具
- **技术**: 变分自编码器(VAE)
- **功能**: 
  - 跨细胞类型预测
  - 跨研究预测
  - 跨物种预测
  - 批次效应去除
- **GitHub**: https://github.com/theislab/scgen

### 2. CPA (2023年)
- **论文**: "Predicting cellular responses to complex perturbations" (Molecular Systems Biology, 2023)
- **合作**: Facebook AI Research (FAIR)
- **技术**: 组合扰动自编码器
- **功能**:
  - 解耦扰动、细胞和剂量效应
  - 分布外预测
  - 剂量-响应曲线估计
  - 跨细胞类型转移
- **GitHub**: https://github.com/facebookresearch/CPA

### 3. CellFlow (2025年)
- **论文**: Cell报道 (2025)
- **技术**: **流匹配(Flow Matching)** + **最优传输(Optimal Transport)**
- **性能**: FID提升35%，作用机制预测准确率提升12%
- **GitHub**: https://github.com/theislab/CellFlow2

### 4. Pertpy (2025年)
- **论文**: "Pertpy: an end-to-end framework for perturbation analysis" (Nature Methods, 2025)
- **功能**: 一站式单细胞扰动数据分析框架
- **GitHub**: https://github.com/theislab/pertpy

### 5. scvi-tools (持续更新)
- **功能**: 单细胞数据分析框架
- **影响**: 被广泛使用，支撑scGen等工具
- **GitHub**: https://github.com/scverse/scvi-tools

## 开源生态

Theis Lab建立了完整的开源生态系统：

```
scvi-tools (基础框架)
    ├── scGen (扰动预测)
    ├── CPA (组合扰动)
    ├── CellFlow (流匹配)
    └── Pertpy (扰动分析)
```

## 研究方向

1. **单细胞扰动预测**
   - 基因扰动响应
   - 化学扰动响应
   - 组合扰动预测

2. **深度生成模型**
   - 变分自编码器
   - 流匹配
   - 最优传输

3. **多模态整合**
   - 转录组 + 表观基因组
   - 单细胞 + 空间组学

4. **计算生物学工具开发**
   - 开源软件
   - 最佳实践指南

## 国际合作

- **Facebook AI**: CPA合作
- **同济大学**: CellHermes合作 (刘琦教授)
- **Arc Institute**: Virtual Cell Challenge科学顾问委员会
- **全球单细胞社区**: scvi-tools生态

## 影响力

- **引用**: 论文被广泛引用
- **开源**: 工具被全球研究者使用
- **教育**: 培养多名计算生物学人才
- **标准化**: 推动单细胞分析最佳实践

## 相关论文

1. Lotfollahi et al. (2019). scGen predicts single-cell perturbation responses. *Nature Methods*.
2. Lotfollahi et al. (2023). Predicting cellular responses to complex perturbations. *Molecular Systems Biology*.
3. Klein et al. (2025). CellFlow: Predicting single-cell perturbation responses with flow matching.
4. Heumos et al. (2025). Pertpy: an end-to-end framework for perturbation analysis. *Nature Methods*.

## 网站

- https://www.helmholtz-munich.de/icb
- https://github.com/theislab
- https://www.sc-best-practices.org

---

*文档创建时间: 2026-03-28*

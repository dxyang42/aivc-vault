---
tags: [method, vae, scvi-tools, 2018, foundation, variational-inference]
year: 2018
institution: "UC Berkeley (Nir Yosef Lab)"
authors: "Romain Lopez, Jeffrey Regier, Michael Cole, Michael Jordan, Nir Yosef"
architecture: "VAE (Variational Autoencoder)"
paper: "Nature Methods 2018"
code: "https://github.com/scverse/scvi-tools"
status: "已收录"
note: "单细胞变分推断奠基性工作，scvi-tools生态核心"
date: 2026-03-29
---

# scVI 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **全称** | Single-cell Variational Inference |
| **发表时间** | 2018年 (Nature Methods) |
| **开发团队** | UC Berkeley (Nir Yosef Lab) |
| **核心成员** | Romain Lopez, Jeffrey Regier, Michael Cole, Michael Jordan, Nir Yosef |
| **历史地位** | **单细胞变分推断奠基性工作** |

## 核心技术

### 架构设计
scVI基于**层次贝叶斯模型**和**变分推断**的深度学习框架：

```
数据生成过程:
  z_n ~ Normal(0, I)                    # 细胞潜在状态
  l_n ~ LogNormal(l_μ, l_σ²)            # 文库大小
  ρ_n = f_w(z_n)                        # 解码器输出
  x_nj ~ ZINB(l_n × ρ_nj, θ_j)          # 观测计数 (零膨胀负二项分布)
```

### 关键创新
1. **零膨胀负二项分布 (ZINB)**
   - 建模单细胞RNA-seq的稀疏性和过离散性
   - 区分生物学零值和技术零值

2. **条件变分自编码器**
   - 编码器: q(z_n, l_n | x_n)
   - 解码器: p(x_n | z_n, l_n)

3. **批次效应校正**
   - 内置批次校正，无需预处理
   - 保留生物学变异，去除技术噪声

## 核心能力

| 能力 | 描述 |
|------|------|
| **批次校正** | ✅ 去除技术批次效应 |
| **降维可视化** | ✅ 学习低维潜在表示 |
| **差异表达** | ✅ 校正后的DE分析 |
| **缺失值填充** | ✅ 概率性填充 |
| **不确定性量化** | ✅ 后验分布提供不确定性 |

## 技术特点

- **概率建模**: 完整的生成式概率框架
- **端到端**: 无需预处理标准化
- **可扩展**: 支持百万级细胞
- **模块化**: 为后续方法提供基础架构

## 训练与应用

### 输入数据
- 原始UMI计数矩阵
- 批次/条件标签
- 细胞类型注释 (可选)

### 输出
- 潜在表示 (z)
- 归一化表达估计
- 批次校正后的数据

## scvi-tools 生态系统

scVI催生了完整的单细胞分析工具生态：

| 工具 | 功能 | 年份 |
|------|------|------|
| **scVI** | 基础VAE模型 | 2018 |
| **scANVI** | 半监督细胞注释 | 2019 |
| **totalVI** | 多模态(RNA+Protein) | 2020 |
| **MultiVI** | 多组学整合 | 2021 |
| **scArches** | 迁移学习 | 2021 |
| **DestVI** | 空间转录组解卷积 | 2022 |
| **Stereoscope** | 空间映射 | 2020 |

## 优势与局限

### 优势
- 概率框架，不确定性量化
- 内置批次校正
- 无需启发式预处理
- 开源生态完善
- 理论基础扎实

### 局限
- 计算成本较高
- 需要GPU加速大规模数据
- 超参数调优需要经验

## 开源资源

- **GitHub**: https://github.com/scverse/scvi-tools
- **文档**: https://docs.scvi-tools.org/
- **教程**: 丰富的Jupyter Notebook教程
- **社区**: 活跃的scverse社区

## 影响与引用

- **引用量**: >3000次 (Google Scholar)
- **生态影响**: 催生了scvi-tools生态系统
- **方法基础**: 为scGen、CPA等方法提供架构基础
- **工业应用**: 被多家生物科技公司采用

## 相关论文

"Deep generative modeling for single-cell transcriptomics" (Nature Methods, 2018)

## 引用

```bibtex
@article{lopez2018deep,
  title={Deep generative modeling for single-cell transcriptomics},
  author={Lopez, Romain and Regier, Jeffrey and Cole, Michael B and Jordan, Michael I and Yosef, Nir},
  journal={Nature Methods},
  volume={15},
  number={12},
  pages={1053--1058},
  year={2018},
  publisher={Nature Publishing Group}
}
```

## 后续发展

scVI的成功直接启发了：
- **scGen**: 扰动预测
- **CPA**: 组合扰动
- **CellFlow**: 流匹配
- 整个scvi-tools生态系统

---

*文档创建时间: 2026-03-29*

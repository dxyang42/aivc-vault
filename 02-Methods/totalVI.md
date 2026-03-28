---
tags: [method, vae, multi-modal, protein, 2020, scvi-tools]
year: 2020
institution: "UC Berkeley / UCSF"
authors: "Adam Gayoso, I. K. Ashok S. Raman, et al."
architecture: "VAE (Multi-modal)"
paper: "Nature Methods 2021"
code: "https://docs.scvi-tools.org/en/stable/user_guide/models/totalvi.html"
status: "已收录"
note: "首个联合建模RNA和蛋白质的多模态单细胞方法"
date: 2026-03-29
---

# totalVI 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[scVI]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **全称** | Total Variational Inference |
| **发表时间** | 2021年 (Nature Methods) |
| **开发团队** | UC Berkeley / UCSF |
| **核心成员** | Adam Gayoso, I. K. Ashok S. Raman, Nir Yosef等 |
| **历史地位** | **首个RNA+蛋白质联合建模方法** |

## 核心技术

### 架构设计
totalVI扩展scVI框架，同时建模CITE-seq数据的RNA和蛋白质：

```
联合生成模型:
  z_n ~ Normal(0, I)                    # 共享潜在状态
  l_n^g ~ LogNormal(...)                # RNA文库大小
  l_n^p ~ LogNormal(...)                # Protein文库大小
  
  RNA: x_nj^g ~ ZINB(l_n^g × ρ_nj^g, θ_j^g)
  Protein: x_nj^p ~ NB(l_n^p × ρ_nj^p, θ_j^p)
  
  ρ^g = f_w^g(z_n)                      # RNA解码器
  ρ^p = f_w^p(z_n, x_n^g)               # Protein解码器 (可选条件于RNA)
```

### 关键创新
1. **共享潜在空间**
   - RNA和蛋白质共享相同的细胞状态表示
   - 捕捉跨模态关联

2. **模态特异性噪声模型**
   - RNA: 零膨胀负二项分布
   - Protein: 负二项分布 (考虑背景噪声)

3. **蛋白质背景建模**
   - 显式建模抗体非特异性结合
   - 区分真实信号和背景噪声

## 核心能力

| 能力 | 描述 |
|------|------|
| **联合嵌入** | ✅ RNA+Protein统一潜在空间 |
| **模态插补** | ✅ 跨模态预测缺失值 |
| **批次校正** | ✅ 多模态批次效应校正 |
| **差异分析** | ✅ 蛋白质差异表达 |
| **模态权重** | ✅ 自适应学习模态重要性 |

## 技术特点

- **端到端多模态**: 同时处理RNA和蛋白质
- **背景校正**: 蛋白质特异性背景建模
- **不确定性量化**: 概率框架提供置信度
- **可扩展**: 支持大规模CITE-seq数据

## 输入/输出

### 输入
| 类型 | 描述 |
|------|------|
| RNA计数 | 基因表达矩阵 (UMI) |
| Protein计数 | 抗体衍生标签 (ADT) |
| 批次标签 | 实验批次信息 |

### 输出
| 类型 | 描述 |
|------|------|
| 联合嵌入 | 低维细胞表示 |
| 校正表达 | 去噪后的RNA/Protein |
| 背景估计 | 蛋白质背景水平 |
| 差异表达 | 校正后的DE结果 |

## 应用场景

1. **免疫学研究**
   - 同时分析表面蛋白和转录组
   - 精细免疫细胞分型

2. **肿瘤微环境**
   - 解析肿瘤浸润免疫细胞
   - 功能状态与表型关联

3. **发育生物学**
   - 追踪细胞分化轨迹
   - 蛋白-转录动态关系

## 优势与局限

### 优势
- 首个端到端多模态方法
- 蛋白质背景校正
- 跨模态信息整合
- 与scVI生态兼容

### 局限
- 需要配对的RNA+Protein数据
- 蛋白质panel设计依赖先验知识
- 计算成本高于单模态

## 开源资源

- **文档**: https://docs.scvi-tools.org/en/stable/user_guide/models/totalvi.html
- **教程**: scvi-tools官方教程
- **集成**: 与Scanpy、Seurat兼容

## 影响与引用

- **引用量**: >1000次
- **方法影响**: 成为CITE-seq分析标准
- **后续发展**: 启发了MultiVI等多组学方法

## 相关论文

"Joint probabilistic modeling of single-cell multi-omic data with totalVI" (Nature Methods, 2021)

## 引用

```bibtex
@article{gayoso2021joint,
  title={Joint probabilistic modeling of single-cell multi-omic data with totalVI},
  author={Gayoso, Adam and Steier, Zo{\"e} and Lopez, Romain and Regier, Jeffrey and Nazor, Kristopher L and Streets, Aaron and Yosef, Nir},
  journal={Nature Methods},
  volume={18},
  number={3},
  pages={272--282},
  year={2021},
  publisher={Nature Publishing Group}
}
```

## 与Perturbation的关联

totalVI为扰动预测提供多模态基础：
- **多模态扰动响应**: 同时预测RNA和蛋白质变化
- **蛋白质标记验证**: 用表面蛋白验证扰动效应
- **机制解析**: 转录后调控机制研究

---

*文档创建时间: 2026-03-29*

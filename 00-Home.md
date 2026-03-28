---
tags: [MOC, index, aivc, vault]
date: 2026-03-29
version: V5
---

# 🧬 AIVC Perturbation 预测知识库

> **AI Virtual Cell Perturbation Prediction Knowledge Base**
> 
> 版本: V5 | 最后更新: 2026-03-29

## 🗺️ 导航

```dataview
TABLE WITHOUT ID
  link(file.name) as "笔记",
  tags as "标签",
  dateformat(file.mtime, "yyyy-MM-dd") as "修改日期"
FROM ""
WHERE file.name != this.file.name
SORT file.mtime DESC
LIMIT 20
```

## 📚 核心区域

| 区域 | 描述 | 笔记数 |
|------|------|--------|
| [[01-Maps/方法地图\|方法地图]] | 所有 Perturbation 预测方法总览 | 15+ |
| [[01-Maps/团队地图\|团队地图]] | 全球研究团队与机构 | 6+ |
| [[01-Maps/概念地图\|概念地图]] | 核心概念与技术术语 | - |
| [[01-Maps/时间线\|时间线]] | 方法发展时间线 (2019-2026) | - |

## 🔬 方法分类

### 按技术类型
- **[[02-Methods/Transformer系列|Transformer系列]]**: STATE, scLAMBDA, ...
- **[[02-Methods/GNN系列|GNN系列]]**: GEARS, ...
- **[[02-Methods/VAE系列|VAE系列]]**: scVI, scGen, CPA, totalVI, MultiVI, scANVI, ...
- **[[02-Methods/流匹配系列|流匹配系列]]**: CellFlow, ...
- **[[02-Methods/世界模型系列|世界模型系列]]**: AlphaCell, ...
- **[[02-Methods/语言模型系列|语言模型系列]]**: CellHermes, ...
- **[[02-Methods/迁移学习系列|迁移学习系列]]**: scArches, ...
- **[[02-Methods/多模态系列|多模态系列]]**: totalVI, MultiVI, ...

### 按年份
- [[02-Methods/2019-2021基础期|2019-2021基础期]]: scVI, scGen, scANVI
- [[02-Methods/2022-2023发展期|2022-2023发展期]]: GEARS, CPA, Cell Oracle, scArches
- [[02-Methods/2024-2025爆发期|2024-2025爆发期]]: scLAMBDA, STATE, CellFlow
- [[02-Methods/2026前沿期|2026前沿期]]: AlphaCell, CellHermes

## 👥 顶级团队

- [[03-Teams/Arc Institute]] - 虚拟细胞先驱
- [[03-Teams/Fabian Theis Lab]] - 单细胞深度学习 (scVI生态)
- [[03-Teams/同济 DELTA Lab]] - 国内领军
- [[03-Teams/Jure Leskovec Lab]] - 图神经网络
- [[03-Teams/Samantha Morris Lab]] - GRN建模
- [[03-Teams/郭天南 Lab]] - 蛋白质组+AI
- [[03-Teams/Nir Yosef Lab]] - scVI生态创始人

## 📊 统计

| 类别 | 数量 |
|------|------|
| **方法文档** | 15+ |
| **团队文档** | 7+ |
| **基础方法** | scVI, scGen, GEARS, CPA |
| **前沿方法** | STATE, AlphaCell, CellHermes |
| **多模态方法** | totalVI, MultiVI |
| **迁移学习** | scArches |
| **scVI生态** | scVI, scANVI, totalVI, MultiVI, scArches |

## 🎯 V5 更新

- [x] 补充scVI生态方法 (scVI, totalVI, MultiVI, scArches, scANVI)
- [x] 创建方法选择指南
- [ ] 补充缺失的方法文档 (Tahoe-x1, LPM, CellFlux...)
- [ ] 创建对比分析矩阵
- [ ] 添加数据集资源汇总
- [ ] 补充代码实现笔记
- [ ] 建立方法→团队→论文的关联图谱

---

*Vault 结构: PARA + Zettelkasten*
*创建: 2026-03-28*

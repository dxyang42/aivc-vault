---
tags: [MOC, index, aivc, vault]
date: 2026-03-28
version: V4
---

# 🧬 AIVC Perturbation 预测知识库

> **AI Virtual Cell Perturbation Prediction Knowledge Base**
> 
> 版本: V4 | 最后更新: 2026-03-28

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
| [[01-Maps/方法地图\|方法地图]] | 所有 Perturbation 预测方法总览 | - |
| [[01-Maps/团队地图\|团队地图]] | 全球研究团队与机构 | - |
| [[01-Maps/概念地图\|概念地图]] | 核心概念与技术术语 | - |
| [[01-Maps/时间线\|时间线]] | 方法发展时间线 (2019-2026) | - |

## 🔬 方法分类

### 按技术类型
- **[[02-Methods/Transformer系列\|Transformer系列]]**: STATE, scLAMBDA, ...
- **[[02-Methods/GNN系列\|GNN系列]]**: GEARS, ...
- **[[02-Methods/VAE系列\|VAE系列]]**: scGen, CPA, ...
- **[[02-Methods/流匹配系列\|流匹配系列]]**: CellFlow, ...
- **[[02-Methods/世界模型系列\|世界模型系列]]**: AlphaCell, ...
- **[[02-Methods/语言模型系列\|语言模型系列]]**: CellHermes, ...

### 按年份
- [[02-Methods/2019-2021基础期\|2019-2021基础期]]
- [[02-Methods/2022-2023发展期\|2022-2023发展期]]
- [[02-Methods/2024-2025爆发期\|2024-2025爆发期]]
- [[02-Methods/2026前沿期\|2026前沿期]]

## 👥 顶级团队

- [[03-Teams/Arc Institute]] - 虚拟细胞先驱
- [[03-Teams/Fabian Theis Lab]] - 单细胞深度学习
- [[03-Teams/同济 DELTA Lab]] - 国内领军
- [[03-Teams/Jure Leskovec Lab]] - 图神经网络
- [[03-Teams/Samantha Morris Lab]] - GRN建模
- [[03-Teams/郭天南 Lab]] - 蛋白质组+AI

## 🎯 V4 待办

- [ ] 补充缺失的方法文档 (Tahoe-x1, LPM, CellFlux...)
- [ ] 创建对比分析矩阵
- [ ] 添加数据集资源汇总
- [ ] 补充代码实现笔记
- [ ] 建立方法→团队→论文的关联图谱

---

*Vault 结构: PARA + Zettelkasten*
*创建: 2026-03-28*
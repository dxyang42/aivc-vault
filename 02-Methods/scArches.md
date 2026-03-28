---
tags: [method, transfer-learning, reference-mapping, 2021, scvi-tools]
year: 2021
institution: "Helmholtz Munich / UC Berkeley"
authors: "Mohammad Lotfollahi, Sergei Rybakov, et al."
architecture: "VAE + Transfer Learning"
paper: "Nature Biotechnology 2022"
code: "https://docs.scvi-tools.org/en/stable/user_guide/models/scarches.html"
status: "已收录"
note: "单细胞参考映射与迁移学习框架"
date: 2026-03-29
---

# scArches 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[scVI]] | [[Fabian Theis Lab]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **全称** | Single-cell Architecture Surgery |
| **发表时间** | 2022年 (Nature Biotechnology) |
| **开发团队** | Helmholtz Munich + UC Berkeley |
| **核心成员** | Mohammad Lotfollahi, Sergei Rybakov, Fabian Theis, Nir Yosef |
| **历史地位** | **首个单细胞迁移学习框架** |

## 核心技术

### 架构设计
scArches引入"架构手术"概念，实现预训练模型的迁移学习：

```
参考模型 (预训练):
  编码器: E_ref(x) → z
  解码器: D_ref(z) → x

查询数据映射:
  1. 冻结参考模型参数
  2. 添加条件批次归一化 (cBN) 层
  3. 仅训练cBN参数适应新数据
  
  z = E_ref(x) + cBN_adaptation
  x' = D_ref(z)
```

### 关键创新
1. **架构手术 (Architecture Surgery)**
   - 冻结预训练模型主体
   - 仅训练轻量级适应层
   - 保留参考数据知识

2. **条件批次归一化 (cBN)**
   - 为每个新数据集学习独立的BN参数
   - 高效适应新批次/条件

3. **参考映射 (Reference Mapping)**
   - 将新数据映射到参考坐标系
   - 统一细胞类型注释

## 支持的模型

| 基础模型 | 功能 | 应用场景 |
|---------|------|---------|
| **scVI** | 转录组整合 | 批次校正 |
| **scANVI** | 半监督注释 | 细胞类型转移 |
| **totalVI** | 多模态整合 | CITE-seq映射 |
| **MultiVI** | 多组学整合 | 多组学参考 |
| **CPA** | 扰动预测 | 扰动效应迁移 |

## 核心能力

| 能力 | 描述 |
|------|------|
| **参考映射** | ✅ 将新数据映射到参考图谱 |
| **细胞注释** | ✅ 自动细胞类型注释 |
| **批次校正** | ✅ 查询数据批次效应校正 |
| **增量学习** | ✅ 持续添加新数据 |
| **隐私保护** | ✅ 无需共享原始参考数据 |

## 技术特点

- **参数高效**: 仅需训练少量参数
- **快速适应**: 几分钟完成映射
- **知识保留**: 不遗忘参考数据知识
- **可扩展**: 支持大规模参考图谱

## 应用场景

1. **细胞图谱构建**
   - Human Cell Atlas数据整合
   - 持续更新的参考图谱

2. **临床样本分析**
   - 患者样本映射到健康参考
   - 疾病状态自动注释

3. **扰动实验分析**
   - 扰动数据映射到对照参考
   - 扰动效应量化

4. **跨研究比较**
   - 不同研究数据统一坐标系
   - 可重复性分析

## 优势与局限

### 优势
- 训练速度快 (分钟级)
- 计算成本低
- 保留参考知识
- 支持隐私保护分析

### 局限
- 依赖高质量参考数据
- 新细胞类型可能检测不到
- 批次效应严重时效果下降

## 开源资源

- **GitHub**: https://github.com/theislab/scarches
- **文档**: https://docs.scvi-tools.org/en/stable/user_guide/models/scarches.html
- **教程**: 丰富的参考映射教程
- **预训练模型**: 提供多个参考模型

## 影响与引用

- **引用量**: >1000次
- **应用**: 被Human Cell Atlas等项目采用
- **方法影响**: 单细胞迁移学习的标准方法

## 相关论文

"Mapping single-cell data to reference atlases by transfer learning" (Nature Biotechnology, 2022)

## 引用

```bibtex
@article{lotfollahi2022mapping,
  title={Mapping single-cell data to reference atlases by transfer learning},
  author={Lotfollahi, Mohammad and Rybakov, Sergei and Hrovatin, Karin and others},
  journal={Nature Biotechnology},
  volume={40},
  pages={121--130},
  year={2022}
}
```

## 与Perturbation的关联

scArches对扰动预测的关键价值：
- **扰动参考图谱**: 构建扰动响应参考
- **新扰动映射**: 快速分析新扰动实验
- **效应迁移**: 跨细胞类型迁移扰动知识
- **CPA集成**: 支持CPA模型的迁移学习

---

*文档创建时间: 2026-03-29*

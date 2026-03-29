---
tags: [method, perturbation-prediction, multimodal, spatial-transcriptomics, 2024, spatiotemporal-modeling]
year: 2024
institution: "University of Michigan, Yale University, Broad Institute"
authors: "Sichao Li, Yinqi Bai, Hao Zhu, et al."
architecture: "Spatiotemporal Graph Neural Network"
paper: "Nature Methods 2024"
code: "https://github.com/Sichao25/spateo-release"
status: 已收录
date: 2026-03-29
---

# Spateo

## 基本信息

- **名称**: Spateo
- **年份**: 2024
- **机构**: University of Michigan, Yale University, Broad Institute
- **论文**: Spateo: multidimensional spatiotemporal modeling of single-cell resolution spatial transcriptomics
- **期刊**: Nature Methods 2024
- **代码**: https://github.com/Sichao25/spateo-release
- **作者**: Sichao Li, Yinqi Bai, Hao Zhu, et al.

## 核心思想

Spateo是一个用于单细胞分辨率空间转录组学的多维时空建模框架。它将空间转录组数据视为动态系统，通过整合空间位置信息和基因表达的时间演化，实现细胞命运转变的时空动态建模。该方法特别适用于研究发育过程中的细胞分化、组织形态发生以及疾病进展。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                        Spateo Framework                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐      │
│  │ Spatial Data │    │  Gene Expr   │    │   Metadata   │      │
│  │   (x,y,z)    │───▶│   Matrix     │───▶│  (time,etc)  │      │
│  └──────────────┘    └──────────────┘    └──────────────┘      │
│         │                   │                   │              │
│         └───────────────────┼───────────────────┘              │
│                             ▼                                  │
│              ┌──────────────────────────────┐                  │
│              │   Spatial Graph Construction │                  │
│              │  (Delaunay triangulation)    │                  │
│              └──────────────┬───────────────┘                  │
│                             ▼                                  │
│         ┌────────────────────────────────────────┐             │
│         │      Spatiotemporal GNN Layers         │             │
│         │  ┌────────────────────────────────┐    │             │
│         │  │  Message Passing + Time Conv   │    │             │
│         │  │  Spatial attention mechanism   │    │             │
│         │  └────────────────────────────────┘    │             │
│         └───────────────────┬────────────────────┘             │
│                             ▼                                  │
│              ┌──────────────────────────────┐                  │
│              │    Cell-Cell Interaction       │                  │
│              │     (ligand-receptor)          │                  │
│              └──────────────┬───────────────┘                  │
│                             ▼                                  │
│  ┌────────────────────────────────────────────────────────┐   │
│  │                    Output Modules                       │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐  │   │
│  │  │Velocity  │  │  Cell    │  │ Spatial  │  │ Digital│  │   │
│  │  │Inference │  │  Fate    │  │  Domain  │  │ 3D     │  │   │
│  │  └──────────┘  └──────────┘  └──────────┘  └────────┘  │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## 输入/输出

**输入**:
- 空间转录组数据 (如 Stereo-seq, Slide-seq, MERFISH)
- 细胞坐标 (x, y, z)
- 基因表达矩阵
- 时间戳 (如果是时间序列数据)

**输出**:
- 细胞速度向量 (RNA velocity in space)
- 细胞命运预测
- 空间域识别
- 细胞-细胞相互作用网络
- 3D数字化组织重建

## 训练数据

- 小鼠胚胎发育时空数据集 (Stereo-seq)
- 小鼠大脑发育数据
- 人类器官发育数据
- 肿瘤微环境空间数据

## 关键创新

1. **空间图神经网络**: 使用Delaunay三角剖分构建空间邻域图，通过消息传递机制整合局部空间信息
2. **时间卷积**: 整合时间维度，建模基因表达的动态演化
3. **细胞-细胞相互作用**: 系统性的配体-受体分析框架，揭示空间微环境效应
4. **数字化3D重建**: 将2D切片整合为3D组织模型

## 扰动预测能力

Spateo虽然不是专门的扰动预测方法，但其时空建模框架可用于：

1. **扰动效应的空间传播**: 预测扰动如何在组织中扩散
2. **时间动态响应**: 建模扰动后细胞状态的时序变化
3. **微环境影响**: 分析空间邻域对扰动响应的调节作用
4. **细胞命运重编程**: 预测扰动导致的细胞命运转变

## 性能表现

- 在小鼠胚胎发育数据上实现了高精度的细胞命运预测
- 成功重建了小鼠心脏的3D时空发育图谱
- 识别了多种组织特异的细胞-细胞相互作用模式

## 应用场景

1. **发育生物学**: 追踪胚胎发育过程中的细胞命运决定
2. **肿瘤研究**: 分析肿瘤微环境的时空异质性
3. **神经科学**: 研究大脑发育和神经回路形成
4. **药物筛选**: 评估药物在组织水平的时空效应

## 相关方法对比

| 方法 | 空间建模 | 时间建模 | 细胞-细胞相互作用 | 3D重建 |
|------|----------|----------|-------------------|--------|
| Spateo | ✓ | ✓ | ✓ | ✓ |
| PASTE | ✓ | ✗ | ✗ | ✗ |
| STAligner | ✓ | ✗ | ✗ | ✗ |
| SpaGCN | ✓ | ✗ | ✗ | ✗ |

## 引用

```
@article{li2024spateo,
  title={Spateo: multidimensional spatiotemporal modeling of single-cell resolution spatial transcriptomics},
  author={Li, Sichao and Bai, Yinqi and Zhu, Hao and et al.},
  journal={Nature Methods},
  year={2024}
}
```

## 补充说明

Spateo代表了空间转录组学分析的新范式，将单细胞数据置于时空框架下理解。虽然它不是专门的扰动预测工具，但其时空建模能力为理解扰动效应在组织中的传播和演化提供了重要框架。

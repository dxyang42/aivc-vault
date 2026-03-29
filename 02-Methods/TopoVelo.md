---
tags: [method, perturbation-prediction, spatial-transcriptomics, velocity-inference, cell-fate, 2025]
year: 2025
institution: "Johns Hopkins University, Yale University"
authors: "Jean Fan, et al."
architecture: "Dynamical Systems + Graph Neural Network"
paper: "Nature Biotechnology 2025"
code: "https://github.com/jefworks/TopoVelo"
version: 1.0.0
status: 已收录
date: 2026-03-29
---

# TopoVelo

## 基本信息

- **名称**: TopoVelo (Topological Velocity Inference)
- **年份**: 2025
- **机构**: Johns Hopkins University, Yale University
- **论文**: Topological velocity inference from spatial transcriptomic data
- **期刊**: Nature Biotechnology 2025
- **代码**: https://github.com/jefworks/TopoVelo
- **作者**: Jean Fan, et al.

## 核心思想

TopoVelo是一种从空间转录组数据联合推断细胞命运转变的时空动态的方法。它扩展了传统的RNA velocity框架，通过空间耦合的微分方程建模整个组织的单细胞基因表达动态。TopoVelo能够估计细胞速度、学习可解释的空间细胞状态依赖关系，并揭示与配体-受体基因表达相关的细胞间通讯模式。

## 架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                    TopoVelo Framework                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    Input Data                            │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │   │
│  │  │  Spatial     │  │   Gene       │  │   Imaging    │  │   │
│  │  │  Coordinates │  │   Expression │  │   (optional) │  │   │
│  │  └──────────────┘  └──────────────┘  └──────────────┘  │   │
│  └─────────────────────────┬───────────────────────────────┘   │
│                            ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              Spatial Graph Construction                  │   │
│  │                                                          │   │
│  │   • k-NN graph based on spatial proximity               │   │
│  │   • Topological data analysis for structure             │   │
│  │   • Spatial domain identification                       │   │
│  └─────────────────────────┬───────────────────────────────┘   │
│                            ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │         Spatially Coupled Dynamical System               │   │
│  │                                                          │   │
│  │   dx_i/dt = f(x_i) + Σ_j w_ij · g(x_i, x_j)             │   │
│  │                                                          │   │
│  │   where:                                                │   │
│  │   • x_i: gene expression of cell i                      │   │
│  │   • f(x_i): intrinsic dynamics (RNA velocity)           │   │
│  │   • g(x_i, x_j): spatial coupling function              │   │
│  │   • w_ij: spatial weights                               │   │
│  └─────────────────────────┬───────────────────────────────┘   │
│                            ▼                                    │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    Outputs                               │   │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐          │   │
│  │  │   Cell     │ │  Spatial   │ │  Ligand-   │          │   │
│  │  │  Velocity  │ │  Fate Map  │ │  Receptor  │          │   │
│  │  │  Vectors   │ │            │ │  Network   │          │   │
│  │  └────────────┘ └────────────┘ └────────────┘          │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## 输入/输出

**输入**:
- 空间转录组数据 (Slide-seq, Stereo-seq, MERFISH, Xenium)
- 细胞/spot的空间坐标
- 基因表达矩阵 (spliced/unspliced counts)
- 组织图像 (可选)

**输出**:
- 细胞速度向量 (高维基因表达空间中的方向)
- 空间细胞命运图
- 空间依赖的细胞状态转移概率
- 配体-受体相互作用网络
- 发育轨迹推断

## 训练数据

- 发育中小鼠大脑皮层数据 (Slide-seq)
- 小鼠胚胎发育数据
- 人类肿瘤微环境数据
- 器官发生时空数据集

## 关键创新

1. **空间耦合RNA Velocity**: 将传统RNA velocity扩展到考虑空间邻域效应
2. **拓扑数据分析**: 利用拓扑方法识别组织中的结构特征
3. **可解释的空间依赖**: 学习细胞状态转移的空间约束
4. **细胞间通讯推断**: 通过配体-受体分析揭示空间信号传导

## 扰动预测能力

TopoVelo可用于扰动效应的时空建模：

1. **扰动传播预测**: 预测扰动如何在组织中随时间和空间扩散
2. **命运改变预测**: 预测扰动如何改变细胞的正常发育轨迹
3. **空间效应范围**: 估计扰动影响的空间范围
4. **时间动态**: 建模扰动效应的时间演化过程

### 扰动预测工作流程

```
1. 训练阶段:
   - 在正常组织数据上训练TopoVelo模型
   - 学习组织发育的时空动态规律

2. 扰动模拟:
   - 在特定位置/细胞引入虚拟扰动
   - 模拟扰动后的速度场变化
   - 追踪扰动传播路径

3. 效应预测:
   - 预测扰动后的细胞命运分布
   - 估计受影响的空间区域
   - 推断长期发育后果
```

## 性能表现

- 在小鼠大脑皮层发育数据上准确推断细胞命运转变
- 成功识别与已知生物学一致的空间细胞状态依赖
- 揭示了与配体-受体基因表达相关的细胞间通讯模式

## 应用场景

1. **发育生物学**: 追踪发育过程中的细胞命运决定
2. **肿瘤研究**: 分析肿瘤微环境中的细胞动态
3. **再生医学**: 预测组织修复过程中的细胞行为
4. **扰动效应建模**: 模拟基因敲除或药物处理对组织发育的影响

## 相关方法对比

| 方法 | 空间建模 | 时间建模 | 速度推断 | 细胞通讯 |
|------|----------|----------|----------|----------|
| TopoVelo | ✓ | ✓ | ✓ | ✓ |
| scVelo | ✗ | ✓ | ✓ | ✗ |
| Spateo | ✓ | ✓ | ✓ | ✓ |
| PASTE | ✓ | ✗ | ✗ | ✗ |
| CellPhoneDB | ✗ | ✗ | ✗ | ✓ |

## 引用

```
@article{fan2025topovelo,
  title={Topological velocity inference from spatial transcriptomic data},
  author={Fan, Jean and et al.},
  journal={Nature Biotechnology},
  year={2025}
}
```

## 补充说明

TopoVelo代表了空间转录组学分析的重要进展，它将时间维度（RNA velocity）与空间维度（空间转录组）相结合。虽然它主要用于分析正常发育过程，但其框架可以扩展到扰动效应的时空建模：

1. **与GEARS/CPA结合**: 先用GEARS预测基因敲除的基因表达变化，再用TopoVelo模拟这些变化在组织中的时空传播
2. **与Spateo结合**: Spateo提供空间域和细胞-细胞相互作用分析，TopoVelo提供速度推断，两者结合可全面理解扰动效应
3. **与PS结合**: PS量化扰动响应的异质性，TopoVelo解释这种异质性的空间来源

TopoVelo为扰动效应的时空建模提供了理论基础，是AIVC领域中连接单细胞扰动预测与组织水平效应的重要桥梁。

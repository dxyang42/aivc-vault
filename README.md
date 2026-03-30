# AIVC Perturbation Vault

> AI Virtual Cell Perturbation Prediction Knowledge Base
> 
> 最后更新: 2026-03-30

## 📊 Vault 统计

| 类别 | 数量 | 说明 |
|------|------|------|
| **方法文档** | 89 | 完整方法文档 |
| **团队文档** | 11 | 全球研究团队 |
| **概念文档** | ~14 | 核心概念与技术术语 |
| **数据集文档** | ~12 | 数据集与资源 |
| **总文档数** | ~150 | 全部 Markdown 文档 |

---

## 🏗️ Vault 结构

```
aivc-vault/
├── 📄 00-Home.md                 # 首页/导航入口
├── 🗺️ 01-Maps/                   # 地图 (MOC - Map of Content)
│   ├── 方法地图.md               # 方法全景图
│   ├── 团队地图.md               # 研究团队分布
│   ├── 概念地图.md               # 核心概念词典
│   ├── 时间线.md                 # 发展历程 (2018-2026)
│   └── 完整索引.md               # 所有文档索引
├── 🔬 02-Methods/                # 方法文档 (89个)
│   ├── VAE/                      # VAE系列方法 (8)
│   ├── Transformer/              # Transformer系列 (8)
│   ├── GNN/                      # 图神经网络系列 (5)
│   ├── Flow/                     # 流匹配系列 (7)
│   ├── Diffusion/                # 扩散模型系列 (3)
│   ├── LLM/                      # 语言模型系列 (5)
│   ├── GRN/                      # 基因调控网络 (3)
│   ├── OT/                       # 最优传输 (2)
│   ├── Other/                    # 其他方法 (48)
│   └── README.md                 # 方法目录说明
├── 👥 03-Teams/                  # 团队文档 (11个)
├── 💡 04-Concepts/               # 概念文档 (~14个)
└── 📦 05-Resources/              # 资源 (~12个)
```

---

## 🚀 快速开始

### 1. 浏览入口

从 [[00-Home]] 开始，这是 Vault 的导航中心。

### 2. 核心地图

| 地图 | 用途 |
|------|------|
| [[01-Maps/方法地图\|方法地图]] | 查找特定方法 |
| [[01-Maps/团队地图\|团队地图]] | 了解研究团队 |
| [[01-Maps/概念地图\|概念地图]] | 理解技术术语 |
| [[01-Maps/时间线\|时间线]] | 了解发展历程 |
| [[01-Maps/完整索引\|完整索引]] | 所有文档列表 |

### 3. 方法分类

#### 按技术类型
- **VAE系列**: scVI, scGen, CPA, totalVI, MultiVI, scANVI, CoupleVAE, scCausalVI, PerturbVAE-Pro
- **Transformer系列**: STATE, scLAMBDA, CellFM, scKGBERT, scBERT, scGPT, scGPT-spatial, scPRAM, sc_Perturbation_Transformer
- **GNN系列**: GEARS, Celcomen, NetPerturb, PerturbGNN, scGNN
- **流匹配系列**: CellFlow, CFM-GP, CellFlux, DC-DSB, scDFM, SCALE, CellFlow-Plus
- **扩散模型系列**: scPPDM, scDiffusion-Perturb, X-Cell
- **语言模型系列**: CellHermes, GenePT, Geneformer, scFoundation, scMulan
- **GRN方法**: Cell Oracle, SCENIC, NicheNet
- **因果推断**: CASCADE, scCausalVI, CausalBERT, CausCell
- **时序方法**: CellDrift, PRESCIENT, scVelo, TemporalPerturb
- **时空建模**: Spateo, TopoVelo, SPACEL
- **最优传输**: scOTM, CellFlow, Waddington-OT
- **网络干扰**: NetPerturb, Cell Oracle, SCENIC
- **临床应用**: Perturb-Cancer, DrugCell, CELLOT
- **可视化工具**: Perturb-Viz, scSHAP
- **基准测试**: SCPERTURB, CAUSALBENCH, scPerturb-Benchmark, ITERPERT, PERTURB-FISH, SYSTEMA, Linear-Baseline

#### 按年份
- **2018-2019 奠基期**: scVI, scGen, scANVI
- **2020-2021 多模态期**: totalVI, MultiVI, scArches
- **2022-2023 发展期**: GEARS, CPA, Cell Oracle, Scouter
- **2024-2025 爆发期**: scLAMBDA, STATE, CellFlow, Tahoe-x1, CellDrift, CoupleVAE, scOTM
- **2026 前沿期**: AlphaCell, CellHermes, X-Cell

### 4. 决策指南

不知道用哪个方法？查看 [[04-Concepts/方法选择指南\|方法选择指南]]

| 场景 | 推荐方法 |
|------|----------|
| 单基因扰动预测 | GEARS, scGen, STATE |
| 组合扰动预测 | CPA, GEARS |
| 零样本预测 | AlphaCell, CellHermes |
| 多模态数据 | totalVI, MultiVI |
| 迁移学习 | scArches |
| 细胞注释 | scANVI |
| 大规模数据 | STATE, scVI |

---

## 📚 核心成果

### 方法覆盖

- ✅ **89个方法文档** - 从 scVI (2018) 到 X-Cell (2026)
- ✅ **完整数学公式** - 核心方法的数学推导
- ✅ **统一术语规范** - 60+术语标准化
- ✅ **团队网络** - 11个顶级研究团队
- ✅ **数据集资源** - 主要基准数据集

### 特色内容

1. **方法关联图谱** - 9个Mermaid可视化图表
2. **对比矩阵** - 多维度方法对比
3. **术语规范** - 完整的术语对照表
4. **选择指南** - 场景化方法推荐
5. **发展时间线** - 2018-2026完整历程

---

## 📖 使用建议

### 对于初学者
1. 从 [[00-Home]] 开始
2. 阅读 [[01-Maps/时间线]] 了解发展历程
3. 查看 [[04-Concepts/方法选择指南]] 选择合适方法
4. 深入阅读感兴趣的方法文档

### 对于研究者
1. 使用 [[01-Maps/方法地图]] 快速定位方法
2. 查看 [[04-Concepts/方法对比矩阵]] 进行横向对比
3. 参考 [[04-Concepts/方法关联图谱]] 理解技术演进
4. 查阅 [[04-Concepts/术语统一规范]] 确保术语准确

### 对于开发者
1. 查看 [[05-Resources/代码实现]] 获取代码资源
2. 参考 [[05-Resources/数据集资源]] 选择基准数据
3. 使用 [[05-Resources/templates/方法模板]] 添加新方法

---

## 📄 许可与引用

本知识库仅供学术研究使用。引用方法时请参考原始论文。

---

*Vault 创建: 2026-03-28*  
*最后更新: 2026-03-30*

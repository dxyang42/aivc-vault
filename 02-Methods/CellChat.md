---
tags: [method, cell-communication, ligand-receptor, saeys, 2021, 2024, inference]
year: 2021
institution: "VIB-UGent Center for Inflammation Research (Saeys Lab)"
authors: "Suoqin Jin, Chanhee Lee, Yves Saeys, et al."
architecture: "Statistical Inference + Network Analysis"
paper: "Nature Communications 2021, Nature Protocols 2024"
code: "https://github.com/sqjin/CellChat"
status: "已收录"
note: "细胞间通讯推断的综合性工具，支持多种可视化"
date: 2026-03-29
---

# CellChat 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[Saeys Lab]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **方法名称** | CellChat |
| **发表时间** | 2021年 (方法), 2024年 (Protocol) |
| **发表期刊** | Nature Communications / Nature Protocols |
| **开发团队** | Saeys Lab |
| **核心成员** | Suoqin Jin, Chanhee Lee, Yves Saeys |
| **机构** | VIB-UGent Center for Inflammation Research |

## 核心技术

### 架构设计

CellChat通过整合**配体-受体相互作用**、**信号通路**和**基因调控网络**来推断细胞间通讯：

```
┌─────────────────────────────────────────────────────────────────────┐
│                  CellChat 细胞通讯分析架构                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              步骤1: 数据库构建                               │   │
│  │                                                             │   │
│  │  CellChatDB (人工整理的数据库):                             │   │
│  │  ┌─────────────────────────────────────────────────────┐   │   │
│  │  │                                                     │   │   │
│  │  │  配体-受体对 (Ligand-Receptor)                      │   │   │
│  │  │  ├── 分泌型配体-受体: 2000+ 对                      │   │   │
│  │  │  ├── 细胞接触蛋白: 500+ 对                          │   │   │
│  │  │  └── 细胞外基质相互作用: 300+ 对                    │   │   │
│  │  │                                                     │   │   │
│  │  │  辅助因子 (Cofactors):                              │   │   │
│  │  │  ├── 激动剂 (Agonists)                              │   │   │
│  │  │  ├── 拮抗剂 (Antagonists)                           │   │   │
│  │  │  └── 共受体 (Co-receptors)                          │   │   │
│  │  │                                                     │   │   │
│  │  │  信号通路注释: 40+ 条经典通路                       │   │   │
│  │  │                                                     │   │   │
│  │  └─────────────────────────────────────────────────────┘   │   │
│  │                                                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              ↓                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              步骤2: 表达数据预处理                           │   │
│  │                                                             │   │
│  │  输入: scRNA-seq 表达矩阵                                   │   │
│  │                                                             │   │
│  │  预处理:                                                    │   │
│  │  - 归一化 (NormalizeData)                                   │   │
│  │  - 对数转换                                                 │   │
│  │  - 细胞类型注释                                             │   │
│  │  - 平均表达计算 (按细胞类型聚合)                            │   │
│  │                                                             │   │
│  │  输出: 细胞类型 × 基因 的平均表达矩阵                       │   │
│  │                                                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              ↓                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              步骤3: 通讯概率计算                             │   │
│  │                                                             │   │
│  │  核心思想: 基于质量作用定律的通讯概率模型                     │   │
│  │                                                             │   │
│  │  对每对细胞类型 (sender → receiver):                        │   │
│  │  ┌─────────────────────────────────────────────────────┐   │   │
│  │  │                                                     │   │   │
│  │  │  配体表达: L_sender = avg_expression(L in sender)  │   │   │
│  │  │                                                     │   │   │
│  │  │  受体表达: R_receiver = avg_expression(R in receiver)│  │   │
│  │  │                                                     │   │   │
│  │  │  基础通讯概率:                                      │   │   │
│  │  │  P_comm = L_sender × R_receiver                     │   │   │
│  │  │                                                     │   │   │
│  │  │  考虑辅助因子:                                      │   │   │
│  │  │  P_comm = L × R × Agonist × (1 - Antagonist)        │   │   │
│  │  │                                                     │   │   │
│  │  └─────────────────────────────────────────────────────┘   │   │
│  │                                                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              ↓                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              步骤4: 统计推断                                 │   │
│  │                                                             │   │
│  │  置换检验 (Permutation Test):                               │   │
│  │  - 随机打乱细胞类型标签 1000次                              │   │
│  │  - 计算零分布下的通讯概率                                   │   │
│  │  - 计算p值: P_observed vs P_random                          │   │
│  │                                                             │   │
│  │  结果过滤:                                                  │   │
│  │  - 保留显著通讯 (p < 0.05)                                  │   │
│  │  - 考虑配体/受体表达比例                                    │   │
│  │                                                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              ↓                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              步骤5: 网络分析与可视化                         │   │
│  │                                                             │   │
│  │  多层次可视化:                                              │   │
│  │  - 层次图 (Hierarchical plot)                               │   │
│  │  - 网络图 (Network plot)                                    │   │
│  │  - 弦图 (Chord diagram)                                     │   │
│  │  - 气泡图 (Bubble plot)                                     │   │
│  │  - 热图 (Heatmap)                                           │   │
│  │                                                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### 关键创新

1. **综合数据库**
   - 人工整理的配体-受体数据库
   - 包含辅助因子信息
   - 信号通路注释完整

2. **通讯概率模型**
   - 基于质量作用定律
   - 考虑多亚基复合物
   - 统计显著性检验

3. **多层次可视化**
   - 细胞类型间通讯网络
   - 信号通路水平聚合
   - 配体-受体对水平细节

## 扰动预测能力

### 核心功能

| 功能 | 描述 | 适用场景 |
|------|------|----------|
| **通讯强度比较** | 比较不同条件下的通讯强度 | 疾病vs正常 |
| **通讯模式识别** | 识别自分泌/旁分泌/内分泌 | 微环境分析 |
| **信号通路分析** | 识别活跃的信号通路 | 机制研究 |
| **关键配体识别** | 找出驱动通讯的关键分子 | 药物靶点发现 |
| **网络拓扑分析** | 分析通讯网络的结构特征 | 系统生物学 |

### 扰动响应分析

CellChat支持比较分析来推断扰动效应：
- 正常 vs 疾病状态的通讯差异
- 治疗前后的通讯变化
- 基因敲除/过表达的通讯影响

## 技术细节

### 通讯概率计算

```
┌─────────────────────────────────────────────────────────────────────┐
│              通讯概率计算模型                                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  对配体-受体对 (L, R):                                              │
│                                                                     │
│  基础概率:                                                          │
│  P_base = exp(L_sender) × exp(R_receiver)                           │
│                                                                     │
│  考虑辅助因子:                                                      │
│  P_full = P_base × P_agonist × (1 - P_antagonist)                   │
│                                                                     │
│  其中:                                                              │
│  - L_sender: 配体在发送细胞的平均表达                               │
│  - R_receiver: 受体在接收细胞的平均表达                             │
│  - P_agonist: 激动剂的综合效应                                      │
│  - P_antagonist: 拮抗剂的综合效应                                   │
│                                                                     │
│  归一化:                                                            │
│  P_norm = P_full / max(P_all)                                       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### 比较分析框架

```
┌─────────────────────────────────────────────────────────────────────┐
│              多条件比较分析                                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  输入: 多个条件下的CellChat对象                                     │
│                                                                     │
│  比较维度:                                                          │
│  1. 总体通讯强度变化                                                │
│  2. 特定细胞类型对的通讯变化                                        │
│  3. 信号通路的活性变化                                              │
│  4. 配体-受体对的贡献变化                                           │
│                                                                     │
│  输出:                                                              │
│  - 上调/下调的通讯通路                                              │
│  - 条件特异性的通讯模式                                             │
│  - 功能相似性/结构相似性分析                                        │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## 与其他方法的对比

| 方法 | 数据库大小 | 辅助因子 | 统计检验 | 可视化 | 特点 |
|------|------------|----------|----------|--------|------|
| **CellChat** | 3000+ | 支持 | 置换检验 | 丰富 | 综合可视化 |
| **NicheNet** | 依赖外部 | 支持 | 调控潜力 | 中等 | 结合先验知识 |
| **CellPhoneDB** | 2000+ | 支持 | 无 | 基础 | 结构信息 |
| **DIALOGUE** | 无 | 无 | 统计检验 | 基础 | 细胞状态耦合 |
| **Celcomen** | 依赖外部 | 支持 | 概率模型 | 中等 | 通讯强度估计 |
| **SingleCellSignalR** | 1000+ | 无 | 相关性 | 基础 | 简单快速 |

## 应用场景

### 肿瘤微环境
- 肿瘤细胞与免疫细胞的通讯
- 肿瘤-基质相互作用
- 治疗抵抗机制

### 发育生物学
- 胚胎发育中的信号传递
- 器官形成中的细胞间通讯
- 干细胞微环境

### 免疫学
- 免疫细胞间的相互作用
- 免疫突触分析
- 炎症信号网络

### 神经科学
- 神经元-胶质细胞通讯
- 神经发育信号
- 神经退行性疾病

## 代码示例

```r
library(CellChat)

# 创建CellChat对象
cellchat <- createCellChat(object = data.input, meta = meta)

# 设置数据库
cellchat@DB <- CellChatDB.human  # 或 CellChatDB.mouse

# 预处理
cellchat <- subsetData(cellchat)
cellchat <- identifyOverExpressedGenes(cellchat)
cellchat <- identifyOverExpressedInteractions(cellchat)

# 推断通讯网络
cellchat <- computeCommunProb(cellchat)
cellchat <- filterCommunication(cellchat, min.cells = 10)

# 可视化
netVisual_bubble(cellchat, sources.use = 1, targets.use = 2)
netVisual_chord_gene(cellchat, sources.use = 1, targets.use = 2)
```

## 相关资源

- **论文**: Jin et al., Nature Communications 2021
- **Protocol**: Jin et al., Nature Protocols 2024
- **代码**: https://github.com/sqjin/CellChat
- **教程**: https://htmlpreview.github.io/?https://github.com/sqjin/CellChat/blob/master/tutorial/CellChat-vignette.html

## 引用

```bibtex
@article{jin2021cellchat,
  title={Inference and analysis of cell-cell communication using CellChat},
  author={Jin, Suoqin and Guerrero-Juarez, Christian F and Zhang, Lihua and Chang, Ivan and Ramos, Raul and Kuan, Chen-Hsiang and Myung, Peggy and Plikus, Maksim V and Nie, Qing},
  journal={Nature Communications},
  volume={12},
  number={1},
  pages={1088},
  year={2021}
}
```

---

> 返回 [[方法总览]] | 最后更新: 2026-03-29

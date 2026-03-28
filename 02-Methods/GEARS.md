---
tags: [method, gnn, stanford, 2023, knowledge-graph]
year: 2023
institution: "Stanford University (SNAP Lab)"
authors: "Yusuf Roohani, Kexin Huang, Jure Leskovec"
architecture: "GNN + Knowledge Graph"
paper: "Nature Biotechnology 2023"
code: "https://github.com/snap-stanford/GEARS"
status: "已收录"
date: 2026-03-28
---

# GEARS 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[Jure Leskovec Lab]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **全称** | Graph-enhanced Gene Activation and Repression Simulator |
| **发表时间** | 2023年 (Nature Biotechnology) |
| **预印本** | 2022年7月 (bioRxiv) |
| **开发团队** | Stanford University (SNAP Lab) |
| **核心成员** | Yusuf Roohani, Kexin Huang, Jure Leskovec |

## 核心技术

### 架构设计
GEARS结合**深度学习**与**基因-基因关系知识图谱**：

1. **图神经网络 (GNN)**
   - 融合基因关系图
   - 融合扰动关系图

2. **知识图谱整合**
   - STRING数据库
   - 基因共表达网络
   - 基因本体论 (Gene Ontology)

### 技术特点
- 几何深度学习
- 捕捉基因间相互作用
- 预测多基因扰动的**非加性效应**
- 使用嵌入表示、多维组件

## 数学公式

### 图神经网络消息传递

GEARS使用图神经网络(GNN)在基因关系图上进行消息传递：

#### 基因关系图

定义基因关系图 $\mathcal{G} = (\mathcal{V}, \mathcal{E})$：
- $\mathcal{V}$: 基因节点集合，$|\mathcal{V}| = N_{\text{genes}}$
- $\mathcal{E}$: 边集合，来自STRING数据库、共表达网络等

#### 消息传递机制

对于第 $l$ 层GNN，节点 $i$ 的更新公式：

$$h_i^{(l)} = \text{UPDATE}^{(l)}\left(h_i^{(l-1)}, \text{AGGREGATE}^{(l)}\left(\{h_j^{(l-1)} : j \in \mathcal{N}(i)\}\right)\right)$$

其中：
- $h_i^{(l)} \in \mathbb{R}^{d_l}$ 为节点 $i$ 在第 $l$ 层的表示
- $\mathcal{N}(i)$ 为节点 $i$ 的邻居集合
- $\text{AGGREGATE}$ 为聚合函数
- $\text{UPDATE}$ 为更新函数

#### 具体实现 (Graph Attention Network)

GEARS采用图注意力网络(GAT)进行消息传递：

**注意力系数计算**：
$$e_{ij} = \text{LeakyReLU}\left(a^T [Wh_i \| Wh_j]\right)$$

$$\alpha_{ij} = \frac{\exp(e_{ij})}{\sum_{k \in \mathcal{N}(i)} \exp(e_{ik})}$$

其中：
- $W \in \mathbb{R}^{d' \times d}$ 为可学习的线性变换
- $a \in \mathbb{R}^{2d'}$ 为注意力参数向量
- $\|$ 表示向量拼接

**消息聚合**：
$$h_i' = \sigma\left(\sum_{j \in \mathcal{N}(i)} \alpha_{ij} Wh_j\right)$$

**多头注意力**：
$$h_i' = \|_{k=1}^{K} \sigma\left(\sum_{j \in \mathcal{N}(i)} \alpha_{ij}^{(k)} W^{(k)} h_j\right)$$

### 扰动嵌入

GEARS为每个扰动(基因)学习嵌入向量：

$$e_{\text{pert}}^{(i)} = \text{Embedding}(\text{gene}_i) \in \mathbb{R}^d$$

对于多基因组合扰动：
$$e_{\text{combo}} = f_{\text{combine}}(e_{\text{pert}}^{(1)}, e_{\text{pert}}^{(2)}, ..., e_{\text{pert}}^{(k)})$$

其中 $f_{\text{combine}}$ 可以是：
- 求和: $e_{\text{combo}} = \sum_i e_{\text{pert}}^{(i)}$ (加性假设)
- 神经网络: $e_{\text{combo}} = \text{MLP}([e_{\text{pert}}^{(1)}; ...; e_{\text{pert}}^{(k)}])$ (非加性建模)

### 非加性效应建模

GEARS的核心创新是预测组合扰动的非加性效应：

$$\hat{x}_{\text{combo}} = x_{\text{control}} + \underbrace{\sum_i \Delta x_i}_{\text{加性效应}} + \underbrace{\Delta x_{\text{interaction}}}_{\text{非加性效应}}$$

其中交互项通过GNN学习：
$$\Delta x_{\text{interaction}} = \text{GNN}_{\text{interaction}}(e_{\text{combo}}, \mathcal{G})$$

### 损失函数

$$\mathcal{L} = \|x_{\text{true}} - \hat{x}\|^2 + \lambda_{\text{reg}} \|W\|^2$$

对于组合扰动，增加交互损失：
$$\mathcal{L}_{\text{interaction}} = \|\Delta x_{\text{interaction}} - (x_{\text{combo}} - x_{\text{control}} - \sum_i \Delta x_i)\|^2$$

### 不确定性量化

GEARS使用贝叶斯神经网络或集成方法输出预测不确定性：

$$\sigma^2_{\text{pred}} = \text{Var}(\{\hat{x}^{(k)}\}_{k=1}^{M})$$

其中 $M$ 为蒙特卡洛采样次数或集成模型数量。

## 核心能力

| 能力 | 描述 |
|------|------|
| **单基因扰动预测** | ✅ 预测单基因扰动的转录结果 |
| **多基因扰动预测** | ✅ 预测多基因组合扰动 |
| **未见基因预测** | ✅ 预测未见过的基因扰动 |
| **非加性相互作用** | ✅ 检测协同效应(synergy) |
| **基因组合预测** | ✅ 预测从未实验过的基因组合 |

## 性能表现

| 指标 | 结果 |
|------|------|
| **预测精度提升** | 比现有方法高**40%** |
| **最强交互识别** | 能力是先前方法的**2倍** |
| **遗传交互类型** | 预测四种不同亚型 |
| **不确定性指标** | 贝叶斯公式输出 |

## 训练与验证

### 标准数据集
- **Norman Dataset**: 105单基因 + 131双基因扰动
- 91k观测值
- 用于验证双基因组合扰动预测

## 开源资源

- **GitHub**: https://github.com/snap-stanford/GEARS
- **文档**: 包含教程和示例

## 应用场景

1. **药物发现**
   - 预测药物组合的协同效应
   - 识别新的药物靶点

2. **癌症生物学**
   - 预测癌细胞对治疗的响应
   - 理解耐药机制

3. **再生医学**
   - 预测细胞重编程结果
   - 优化分化协议

4. **个性化医疗**
   - 基于患者基因型预测治疗反应

## 优势与局限

### 优势
- 结合先验知识(知识图谱)
- 可解释性强
- 组合扰动预测能力突出
- 开源，社区活跃

### 局限
- 需要高质量的基因关系数据
- 对未见细胞类型的泛化能力有限

## 相关论文

"Predicting transcriptional outcomes of novel multigene perturbations with GEARS" (Nature Biotechnology, 2023)

## 引用

```bibtex
@article{roohani2023gears,
  title={Predicting transcriptional outcomes of novel multigene perturbations with GEARS},
  author={Roohani, Yusuf and Huang, Kexin and Leskovec, Jure},
  journal={Nature Biotechnology},
  year={2023}
}
```

---

*文档创建时间: 2026-03-28*

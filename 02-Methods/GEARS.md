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

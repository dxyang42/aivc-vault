---
tags: [method, grn, washu, 2023, simulation]
year: 2023
institution: "Washington University in St. Louis (Morris Lab)"
authors: "Kenji Kamimoto, Samantha A. Morris"
architecture: "GRN + Simulation"
paper: "Nature 2023"
code: "https://github.com/morris-lab/CellOracle"
status: "已收录"
date: 2026-03-28
---

# Cell Oracle 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[Samantha Morris Lab]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **方法名称** | CellOracle |
| **发表时间** | 2023年 |
| **发表期刊** | Nature |
| **开发团队** | Samantha A. Morris Lab |
| **核心成员** | Kenji Kamimoto (主要开发者), Samantha A. Morris (PI) |
| **机构** | Washington University in St. Louis |

## 核心技术

### 架构设计
CellOracle整合**基因调控网络(GRN)推断**与**计算机模拟**：

```
输入: 单细胞多组学数据
      ↓
GRN推断: 构建细胞特异性基因调控网络
      ↓
TF扰动模拟: 计算转录因子扰动效应
      ↓
细胞身份预测: 估计细胞身份转换概率
      ↓
输出: 细胞命运转换矢量图
```

### 技术特点
- **GRN推断**: 从单细胞数据推断基因调控网络
- **计算机模拟**: 模拟转录因子扰动
- **细胞命运预测**: 预测细胞身份变化

## 输入/输出

### 输入格式
| 输入类型 | 描述 | 格式 |
|---------|------|------|
| **scRNA-seq** | 单细胞转录组 | count矩阵 |
| **scATAC-seq** | 单细胞染色质可及性 | peak矩阵 |
| **基序注释** | TF结合基序 | 列表 |
| **参考基因组** | 基因组版本 | hg38/mm10 |

### 输出格式
| 输出类型 | 描述 | 格式 |
|---------|------|------|
| **GRN** | 基因调控网络 | 边列表 |
| **TF扰动效应** | 转录因子扰动后的表达变化 | 向量 |
| **细胞身份转换** | 细胞命运转换概率 | 矩阵 |
| **矢量图** | 2D可视化 | 向量场 |

## 网络结构

### GRN推断
```python
GRN推断:
  - 方法: 线性回归 (Ridge/Lasso)
  - 输入: TF表达 + 靶基因表达
  - 输出: TF-靶基因调控边权重
  - 细胞类型特异性: 每个细胞类型独立推断
```

### TF扰动模拟
```python
TF扰动模拟:
  - 输入: GRN, TF标识
  - 操作: 将TF表达设为0 (KO) 或增加 (OE)
  - 计算: 网络传播计算下游基因变化
  - 输出: 预测表达变化
```

### 细胞身份转换
```python
细胞身份预测:
  - 输入: 扰动前后表达
  - 方法: 向量场分析
  - 输出: 转换概率 + 轨迹
```

## 训练数据

| 数据属性 | 要求 |
|---------|------|
| **scRNA-seq** | 必需 |
| **scATAC-seq** | 可选，提高准确性 |
| **细胞数量** | >1000 |
| **细胞类型** | 多种 |

## 分析流程

### 步骤1: 数据预处理
- 质量控制
- 标准化
- 降维

### 步骤2: GRN推断
- 识别TF
- 构建基序-靶基因映射
- 线性回归推断调控强度

### 步骤3: TF扰动模拟
- 选择目标TF
- 模拟KO/OE
- 计算网络传播

### 步骤4: 细胞身份分析
- 计算转换概率
- 生成矢量图
- 可视化

## 性能指标

| 应用 | 验证 |
|------|------|
| **小鼠造血** | 正确预测分化轨迹 |
| **人造血** | 验证已知调控因子 |
| **斑马鱼胚胎** | 预测发育过程 |

## 功能特点

- ✅ **GRN推断**: 从单细胞数据推断基因调控网络
- ✅ **TF扰动模拟**: 模拟转录因子敲除/过表达
- ✅ **细胞命运预测**: 预测细胞身份转换
- ✅ **可视化**: 生成细胞命运矢量图
- ✅ **多物种**: 支持人和小鼠

## 应用场景

1. **发育生物学**
   - 预测发育轨迹
   - 识别关键调控因子

2. **细胞重编程**
   - 预测重编程结果
   - 优化重编程因子

3. **疾病建模**
   - 模拟疾病相关TF扰动
   - 理解疾病机制

## 支持基因组版本

| 物种 | 版本 |
|------|------|
| **Human** | hg38, hg19 |
| **Mouse** | mm39, mm10, mm9 |

## 开源资源

- **GitHub**: https://github.com/morris-lab/CellOracle
- **文档**: https://morris-lab.github.io/CellOracle.documentation/
- **教程**: 包含详细教程和示例数据

## 优势与局限

### 优势
- 机制驱动，可解释性强
- 整合多组学数据
- 可视化直观
- 开源，社区活跃
- 多物种支持

### 局限
- 依赖GRN推断准确性
- 线性假设可能不适用
- 需要多组学数据
- 计算资源需求高

## 相关论文

"Dissecting cell identity via network inference and in silico gene perturbation" (Nature, 2023)

## 引用

```bibtex
@article{kamimoto2023celloracle,
  title={Dissecting cell identity via network inference and in silico gene perturbation},
  author={Kamimoto, Kenji and others},
  journal={Nature},
  year={2023}
}
```

---

*文档创建时间: 2026-03-28*

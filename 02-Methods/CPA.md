---
tags: [method, vae, fair, theis, 2023, compositional]
year: 2023
institution: "FAIR + Helmholtz Munich"
authors: "Mohammad Lotfollahi, Fabian J. Theis et al."
architecture: "VAE (Compositional)"
paper: "Molecular Systems Biology 2023"
code: "https://github.com/facebookresearch/CPA"
status: "已收录"
date: 2026-03-28
---

# CPA (Compositional Perturbation Autoencoder) 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[Fabian Theis Lab]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **全称** | Compositional Perturbation Autoencoder |
| **发表时间** | 2023年 |
| **发表期刊** | Molecular Systems Biology |
| **开发团队** | Facebook AI Research (FAIR) + Fabian Theis Group |
| **核心成员** | Mohammad Lotfollahi, Fabian J. Theis等 |
| **机构** | FAIR, Helmholtz Munich |

## 核心技术

### 架构设计
CPA是**组合扰动自编码器**，解耦扰动、细胞和剂量效应：

```
输入: 基因表达 x
      ↓
编码器: z = Encoder(x)
      ↓
条件嵌入: z' = z + z_perturbation + z_cell + z_dose
      ↓
解码器: x' = Decoder(z')
      ↓
输出: 重建/预测基因表达
```

### 技术特点
- **解耦表示**: 分离扰动、细胞类型、剂量效应
- **组合建模**: 支持多扰动组合
- **剂量-响应**: 建模剂量-响应曲线

## 输入/输出

### 输入格式
| 输入类型 | 描述 | 示例 |
|---------|------|------|
| **基因表达** | 原始表达计数 | 20,000+ 基因 |
| **扰动标识** | 药物/基因ID | One-hot |
| **细胞类型** | 细胞类型标签 | One-hot |
| **剂量** | 药物浓度 | 标量 |
| **时间** | 扰动时间 | 标量 |

### 输出格式
| 输出类型 | 描述 | 用途 |
|---------|------|------|
| **重建表达** | 重建基因表达 | 去噪 |
| **预测表达** | 扰动后表达 | 预测 |
| **潜在表示** | 解耦的潜在向量 | 分析 |
| **剂量-响应** | 剂量-响应曲线 | 药物筛选 |

## 网络结构

### 编码器
```python
Encoder:
  - 输入: 基因表达 (20,000)
  - 隐藏层: [512, 256, 128]
  - 输出: 潜在表示 z (50-100维)
```

### 条件嵌入
```python
条件嵌入:
  - 扰动嵌入: z_p = Embedding(perturbation_id)
  - 细胞嵌入: z_c = Embedding(cell_type)
  - 剂量嵌入: z_d = MLP(dose)
  - 组合: z' = z + z_p + z_c + z_d
```

### 解码器
```python
Decoder:
  - 输入: 条件潜在表示 z'
  - 隐藏层: [128, 256, 512]
  - 输出: 基因表达 (20,000)
```

## 训练数据

| 数据属性 | 规模 |
|---------|------|
| **药物数量** | 100+ |
| **细胞类型** | 10+ |
| **剂量水平** | 多剂量 |
| **组合扰动** | 支持 |

## 训练策略

### 预训练任务
1. **重建任务**: 重建原始基因表达
2. **扰动预测**: 预测扰动后表达
3. **组合预测**: 预测组合扰动效应

### 损失函数
```
L = L_reconstruction + L_adversarial + L_kl
```

### 优化器
- **类型**: Adam
- **学习率**: 1e-3
- **批次大小**: 128

## 性能指标

| 能力 | 性能 |
|------|------|
| **分布外预测** | 支持未见药物组合 |
| **跨细胞类型** | 可转移扰动效应 |
| **剂量-响应** | 准确估计 |
| **组合效应** | 预测遗传相互作用 |

## 功能特点

- ✅ **分布外预测**: 预测未见药物组合
- ✅ **可解释潜在空间**: 学习可解释的扰动/细胞嵌入
- ✅ **剂量-响应曲线**: 估计每个扰动的剂量-响应
- ✅ **跨细胞类型**: 将扰动效应转移到未见细胞类型
- ✅ **组合预测**: 预测组合遗传相互作用
- ✅ **不确定性**: 提供预测不确定性

## 应用场景

1. **药物组合发现**
   - 预测协同/拮抗效应
   - 优化药物配比

2. **剂量优化**
   - 确定最佳剂量
   - 避免毒性

3. **细胞类型特异性**
   - 预测不同细胞类型的响应
   - 个性化医疗

## 开源资源

- **GitHub**: https://github.com/facebookresearch/CPA
- **文档**: 包含教程和示例
- **安装**: `pip install cpa`

## 优势与局限

### 优势
- 解耦表示，可解释性强
- 支持组合扰动
- 剂量-响应建模
- 跨细胞类型泛化
- 开源，FAIR支持

### 局限
- 需要多条件训练数据
- 对未见扰动的泛化有限
- 计算成本较高

## 相关论文

"Predicting cellular responses to complex perturbations in high-throughput screens" (Molecular Systems Biology, 2023)

## 引用

```bibtex
@article{lotfollahi2023cpa,
  title={Predicting cellular responses to complex perturbations in high-throughput screens},
  author={Lotfollahi, Mohammad and others},
  journal={Molecular Systems Biology},
  year={2023}
}
```

---

*文档创建时间: 2026-03-28*

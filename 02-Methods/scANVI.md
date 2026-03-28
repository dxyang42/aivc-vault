---
tags: [method, semi-supervised, cell-annotation, 2019, scvi-tools]
year: 2019
institution: "UC Berkeley"
authors: "Chenling Xu, Romain Lopez, et al."
architecture: "VAE (Semi-supervised)"
paper: "eLife 2019"
code: "https://docs.scvi-tools.org/en/stable/user_guide/models/scanvi.html"
status: "已收录"
note: "半监督单细胞注释，结合标签和未标记数据"
date: 2026-03-29
---

# scANVI 方法详细文档

> [[方法地图]] | [[团队地图]] | [[时间线]] | [[scVI]]

## 基本信息

| 属性 | 详情 |
|------|------|
| **全称** | Single-cell Annotation using Variational Inference |
| **发表时间** | 2019年 (eLife) |
| **开发团队** | UC Berkeley (Nir Yosef Lab) |
| **核心成员** | Chenling Xu, Romain Lopez, Nir Yosef等 |
| **历史地位** | **首个深度生成模型的半监督细胞注释方法** |

## 核心技术

### 架构设计
scANVI扩展scVI，引入半监督学习框架：

```
生成模型:
  标记细胞 (有标签y):
    y ~ Categorical(π)                    # 细胞类型先验
    z ~ Normal(μ_y, σ_y²)                 # 类型条件潜在状态
    x ~ ZINB(f(z))                        # 观测数据
    
  未标记细胞:
    z ~ Normal(0, I)                      # 标准先验
    x ~ ZINB(f(z))                        # 观测数据
    
  分类器:
    y' = Classifier(z)                    # 从潜在状态预测标签
```

### 关键创新
1. **半监督变分推断**
   - 同时利用标记和未标记数据
   - 标签信息引导潜在空间结构

2. **类型条件先验**
   - 每个细胞类型有特定的高斯先验
   - 潜在空间反映细胞类型层次

3. **端到端分类**
   - 联合训练生成模型和分类器
   - 生成模型提升分类性能

## 核心能力

| 能力 | 描述 |
|------|------|
| **半监督学习** | ✅ 少量标签 + 大量未标记数据 |
| **细胞注释** | ✅ 自动细胞类型预测 |
| **批次校正** | ✅ 跨批次标签转移 |
| **不确定性** | ✅ 预测置信度估计 |
| **新类型检测** | ✅ 识别未见细胞类型 |

## 技术特点

- **标签高效**: 仅需少量标记细胞
- **端到端**: 整合降维和分类
- **概率框架**: 不确定性量化
- **可扩展**: 支持大规模数据集

## 训练策略

### 半监督目标函数
```
L = L_labeled + α × L_unlabeled + β × L_classification

L_labeled: 标记细胞的生成损失
L_unlabeled: 未标记细胞的生成损失  
L_classification: 分类器交叉熵损失
```

### 训练流程
1. 预训练scVI (无监督)
2. 添加分类器分支
3. 联合训练生成模型和分类器

## 应用场景

1. **细胞图谱注释**
   - 大规模细胞类型注释
   - 专家标注辅助

2. **新数据注释**
   - 新研究数据自动注释
   - 与scArches结合使用

3. **质量控制**
   - 双细胞检测
   - 低质量细胞过滤

4. **疾病研究**
   - 疾病特异性细胞识别
   - 异常细胞检测

## 优势与局限

### 优势
- 标签效率高
- 结合生成和判别信息
- 批次校正同时完成注释
- 开源生态完善

### 局限
- 需要代表性标记样本
- 新细胞类型可能误分类
- 类别不平衡敏感

## 开源资源

- **文档**: https://docs.scvi-tools.org/en/stable/user_guide/models/scanvi.html
- **教程**: scvi-tools官方教程
- **集成**: 与scArches无缝集成

## 影响与引用

- **引用量**: >800次
- **应用**: 被多个细胞图谱项目采用
- **方法影响**: 半监督单细胞分析的标准工具

## 相关论文

"Harmonization and annotation of single-cell transcriptomics data with deep generative models" (eLife, 2019)

## 引用

```bibtex
@article{xu2019harmonization,
  title={Harmonization and annotation of single-cell transcriptomics data with deep generative models},
  author={Xu, Chenling and Lopez, Romain and Mehlman, Edouard and Regier, Jeffrey and Jordan, Michael I and Yosef, Nir},
  journal={eLife},
  volume={8},
  pages={e45590},
  year={2019}
}
```

## 与Perturbation的关联

scANVI对扰动预测的价值：
- **扰动后注释**: 自动识别扰动后细胞状态
- **状态分类**: 区分不同扰动响应状态
- **新状态检测**: 发现新的扰动诱导细胞类型
- **与CPA集成**: 支持CPA的半监督训练

---

*文档创建时间: 2026-03-29*

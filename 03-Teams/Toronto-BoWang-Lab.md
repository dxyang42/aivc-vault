# 多伦多大学 Bo Wang Lab - scGPT 团队

## 基本信息

| 属性 | 内容 |
|------|------|
| **实验室名称** | Wang Lab, University of Toronto |
| **所在机构** | 多伦多大学 (University of Toronto) |
| **所在院系** | Temerty Faculty of Medicine |
| **PI** | Bo Wang (王波) |
| **实验室主页** | https://wanglab.ml/ |
| **所在城市** | 加拿大多伦多 |

## 核心成员

### Bo Wang (王博) - PI
- **职位**: 多伦多大学医学系助理教授
- **兼职**: Vector Institute  faculty member
- **研究方向**: 机器学习、计算生物学、单细胞基因组学
- **教育背景**: 斯坦福大学博士 (Jure Leskovec 实验室)
- **荣誉**: Canada Research Chair in AI for Medicine

### scGPT 核心开发团队
| 成员 | 贡献 | 背景 |
|------|------|------|
| **Haotian Cui** | scGPT 第一作者 | 博士生 |
| **Chloe Wang** | 模型开发 | 研究助理 |
| **Kieran Ma** | 数据分析 | 硕士生 |
| **Lara Ibrahim** | 实验验证合作 | 合作者 |

### 其他实验室成员
- **多位博士生和博士后**: 专注于单细胞 AI 方法开发
- **临床合作者**: 与多伦多多家医院合作

## 代表方法

### scGPT (Single-cell Generative Pre-trained Transformer)
- **发表时间**: 2023 (Nature Methods)
- **预印本**: 2023年2月 (bioRxiv)
- **正式发表**: 2024年 (Nature Methods)

### scGPT 技术规格

| 属性 | 详情 |
|------|------|
| **模型架构** | Transformer (GPT-like) |
| **参数量** | 初始: 1.5M - 51M; 扩展版: 100M+ |
| **输入表示** | 基因作为 token |
| **预训练数据** | 3300万+ 细胞 |
| **细胞类型** | 覆盖多种组织和物种 |

### 核心创新
1. **基因 Tokenization**
   - 将基因表达视为序列
   - 每个基因是一个 token
   - 表达值作为 token 属性

2. **预训练策略**
   - 掩码基因建模 (Masked Gene Modeling)
   - 基因表达预测
   - 细胞类型分类

3. **迁移学习能力**
   - 零样本预测
   - 少样本微调
   - 跨组织/物种迁移

### 功能特性
| 功能 | 描述 |
|------|------|
| **批次校正** | 整合多批次数据 |
| **细胞注释** | 自动细胞类型标注 |
| **扰动预测** | 基因扰动效应预测 |
| **多组学整合** | scRNA + scATAC |
| **基因调控推断** | 从注意力权重推断 |

## 研究方向

### 1. 单细胞基础模型
- **scGPT**: 首个单细胞大语言模型
- **scGPT 扩展**: 持续预训练更大模型
- **多模态整合**: 结合空间转录组、蛋白质组

### 2. 细胞扰动建模
- **虚拟扰动**: 预测未见过的基因扰动
- **组合扰动**: 多基因扰动效应预测
- **药物响应**: 预测细胞对药物的反应

### 3. 临床转化应用
- **癌症分型**: 基于单细胞的肿瘤分类
- **免疫治疗**: 预测免疫治疗响应
- **药物重定位**: 发现现有药物的新用途

### 4. 网络生物学
- **基因调控网络**: 从 scGPT 注意力推断
- **通路分析**: 自动通路富集
- **疾病机制**: 疾病相关基因模块识别

## 影响力

### 学术影响
- **引用量**: scGPT 论文发表后迅速成为高引论文
- **社区采用**: 被全球数百个实验室使用
- **方法学创新**: 开创了单细胞大模型范式

### 关键论文
1. **Cui et al. (2024)** "scGPT: toward building a foundation model for single-cell multi-omics using generative AI" - *Nature Methods*
   - scGPT 主论文

2. **Cui & Wang (2023)** "scGPT: a foundation model for single-cell multi-omics" - *bioRxiv*
   - 预印本版本

3. **其他相关论文**:
   - 空间转录组分析方法
   - 单细胞图神经网络
   - 多组学整合方法

### 开源贡献
- **scGPT 代码**: https://github.com/bowang-lab/scGPT
- **预训练模型**: Hugging Face 公开
- **教程文档**: 详细的使用指南

### 社区生态
| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/bowang-lab/scGPT |
| **文档** | https://scgpt.readthedocs.io/ |
| **Hugging Face** | https://huggingface.co/bowang-lab |
| **教程** | Colab notebooks 提供 |

## 与 AIVC 的关系

### 核心技术
- **scGPT**: 可作为 AIVC 的 encoder/decoder 组件
- **扰动预测**: 直接支持虚拟细胞模拟
- **预训练权重**: 可用于迁移学习

### 集成方式
```python
# scGPT 在 AIVC 中的典型应用
from scgpt.model import TransformerModel

# 加载预训练模型
model = TransformerModel.load_from_checkpoint("scgpt_human")

# 编码细胞状态
cell_embedding = model.encode(gene_expression)

# 预测扰动效应
perturbed_state = model.predict_perturbation(
    cell_embedding, 
    perturbed_genes=["TP53", "KRAS"]
)
```

## 相关链接

- **实验室主页**: https://wanglab.ml/
- **GitHub**: https://github.com/bowang-lab
- **scGPT 仓库**: https://github.com/bowang-lab/scGPT
- **Vector Institute**: https://vectorinstitute.ai/
- **Google Scholar**: Bo Wang 的论文列表

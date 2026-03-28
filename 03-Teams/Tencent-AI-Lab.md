# 腾讯AI Lab - scBERT 团队

## 基本信息

| 属性 | 内容 |
|------|------|
| **部门名称** | 腾讯 AI Lab (Tencent AI Lab) |
| **成立时间** | 2016年 |
| **总部地点** | 中国深圳、西雅图、纽约 |
| **研究方向** | 机器学习、计算机视觉、NLP、语音、生物计算 |
| **负责人** | 张正友 (主任) |

## 核心成员

### 管理层
| 姓名 | 职位 | 背景 |
|------|------|------|
| **张正友** | 腾讯 AI Lab 主任 | 计算机视觉专家，ACM Fellow |
| **姚星** | 腾讯集团副总裁 (前) | AI Lab 创始人 |

### scBERT 核心团队
| 成员 | 贡献 | 所属团队 |
|------|------|----------|
| **Juexiao Zhou (周觉晓)** | scBERT 第一作者 | 生物计算组 |
| **Bin Zhang (张斌)** | 模型开发 | 生物计算组 |
| **Ying Xu** | 技术指导 | 生物计算组 |
| **其他合作者** | 实验验证 | 外部合作 |

### 生物计算团队
- **团队负责人**: 多位资深研究员
- **研究方向**: 基因组学、蛋白质结构、药物发现
- **合作网络**: 与腾讯医典、外部医院合作

## 代表方法

### scBERT (Single-cell BERT)
- **发表时间**: 2022
- **发表会议**: Nature Machine Intelligence
- **预印本**: 2022年初 (bioRxiv)

### scBERT 技术规格

| 属性 | 详情 |
|------|------|
| **模型架构** | BERT (双向 Transformer) |
| **参数量** | ~110M |
| **输入表示** | 基因作为 token，表达值离散化 |
| **预训练数据** | 数百万单细胞 |
| **训练目标** | 掩码基因预测 + 细胞类型分类 |

### 核心创新
1. **基因表达离散化**
   - 将连续表达值分箱 (binning)
   - 转换为离散 token
   - 适应 BERT 的离散输入

2. **双向注意力**
   - 利用 BERT 的双向编码
   - 捕获基因间双向关系
   - 更好的上下文理解

3. **预训练-微调范式**
   - 大规模无监督预训练
   - 下游任务微调
   - 跨数据集迁移

### 功能特性
| 功能 | 描述 |
|------|------|
| **细胞类型注释** | 自动识别细胞类型 |
| **批次效应校正** | 整合多来源数据 |
| **缺失值插补** | 基因表达补全 |
| **新细胞类型发现** | 识别未知细胞群体 |

## 研究方向

### 1. 单细胞基础模型
- **scBERT**: 首个将 BERT 应用于单细胞的工作
- **模型优化**: 持续改进模型架构
- **多组学扩展**: 整合多种数据类型

### 2. 生物计算
- **基因组学**: 序列分析、变异检测
- **蛋白质结构**: 结构预测、功能注释
- **药物发现**: 分子生成、靶点预测

### 3. 医疗 AI
- **疾病诊断**: 基于基因表达的疾病分类
- **预后预测**: 患者结局预测
- **精准医疗**: 个性化治疗方案

### 4. 多模态学习
- **文本-基因**: 结合医学文献
- **图像-基因**: 病理图像与转录组整合
- **知识图谱**: 生物医学知识嵌入

## 影响力

### 学术影响
- **开创性**: 首个将 NLP 预训练模型成功应用于单细胞
- **引用量**: scBERT 论文被广泛引用
- **方法学影响**: 启发了后续大量单细胞大模型工作

### 关键论文
1. **Zhou et al. (2022)** "scBERT as a large-scale pretrained deep language model for cell type annotation of single-cell RNA-seq data" - *Nature Machine Intelligence*
   - scBERT 主论文

2. **其他相关论文**:
   - 腾讯 AI Lab 在蛋白质结构预测的工作
   - 药物发现相关研究
   - 多组学整合方法

### 开源贡献
- **scBERT 代码**: GitHub 开源
- **预训练模型**: 提供下载
- **使用教程**: 详细文档

### 社区生态
| 资源 | 链接 |
|------|------|
| **GitHub** | https://github.com/TencentAILabHealthcare/scBERT |
| **论文** | Nature Machine Intelligence |
| **腾讯 AI Lab** | https://ai.tencent.com/ailab/ |

## 与 AIVC 的关系

### 核心技术
- **scBERT**: 可作为 AIVC 的 encoder 组件
- **预训练策略**: 掩码语言建模适用于基因表达
- **细胞表示**: 提供高质量的细胞嵌入

### 技术对比: scBERT vs scGPT

| 特性 | scBERT | scGPT |
|------|--------|-------|
| **架构** | BERT (双向) | GPT (自回归) |
| **注意力** | 双向 | 单向 |
| **预训练目标** | 掩码预测 | 自回归生成 |
| **生成能力** | 有限 | 强 |
| **细胞表示** | [CLS] token | 平均/聚合 |
| **发布时间** | 2022 | 2023 |

### 集成方式
```python
# scBERT 在 AIVC 中的典型应用
from scbert.model import scBERT

# 加载预训练模型
model = scBERT.from_pretrained("scbert_human")

# 获取细胞表示
cell_embedding = model.get_cell_embedding(gene_expression)

# 细胞类型预测
cell_type = model.predict_cell_type(gene_expression)
```

## 相关链接

- **腾讯 AI Lab 官网**: https://ai.tencent.com/ailab/
- **scBERT GitHub**: https://github.com/TencentAILabHealthcare/scBERT
- **腾讯医典**: https://baike.qq.com/
- **招聘信息**: 腾讯 AI Lab 持续招聘生物计算人才

## 备注

腾讯 AI Lab 的 scBERT 是中国团队在单细胞 AI 领域的重要贡献，展示了国内在生物计算领域的研究实力。与 scGPT 相比，scBERT 更早发表，采用了不同的技术路线（BERT vs GPT），两者各有优势，可根据具体应用场景选择使用。

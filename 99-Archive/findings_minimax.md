# AI Virtual Cell (AIVC) 研究 - MiniMax 搜索结果

> 搜索日期: 2026-03-28  
> 搜索工具: MiniMax MCP Web Search

---

## 1. AI Virtual Cell / AIVC 相关论文和项目

### 1.1 核心概念与定义

**人工智能虚拟细胞 (AI Virtual Cell, AIVC)** 是一种基于多尺度、多模态的大型神经网络模型，能够在不同状态下表示和模拟分子、细胞和组织的行为。

**核心目标**: 构建一个能够动态模拟真实细胞行为的"数字孪生体"，通过分析海量超高分辨率的细胞数据，进行"假说—模拟"形式的自主学习，既能还原肿瘤的"个性"，实现从分子到肿瘤形态的多尺度模拟，又能预测药物反应和治疗靶点。

### 1.2 重要论文

#### Cell 期刊观点文章 (2024年12月)
- **来源**: 斯坦福大学、基因泰克公司(Genentech)和陈-扎克伯格基金会
- **发表**: Cell 期刊，2024年12月12日
- **核心贡献**: 提出结合人工智能和组学技术构建AIVC，提供模拟细胞功能和行为的新途径
- **链接**: https://www.163.com/dy/article/JKLMGBEM0511B8LM.html

#### Cell Research - 西湖大学郭天南团队 (2025年3月)
- **标题**: "Grow AI Virtual Cells: Three Data Pillars and Closed-Loop Learning"
- **发表**: Cell Research, 2025年3月25日
- **核心观点**: 
  - 提出AIVC构建的三大数据支柱：先验知识、静态结构、动态状态
  - 强调高通量组学数据（特别是微扰蛋白质组学数据）在动态模拟中的关键作用
  - 提出闭环主动学习系统(Closed-Loop Active Learning Systems)
- **建议**: 从酵母(S. cerevisiae)等较简单但信息丰富的细胞模型入手，逐步扩展到更复杂的人类癌细胞系

### 1.3 三大数据支柱

1. **先验知识 (Prior Knowledge)**
   - 细胞的"百科全书"，包含所有已知的细胞生物学信息

2. **静态结构 (Static Structure)**
   - 细胞的"3D建模器"
   - 通过冷冻电镜、空间组学等技术精确还原细胞的纳米级三维结构
   - 呈现细胞器的空间排布，解析分子间的相互作用网络

3. **动态状态 (Dynamic State)**
   - 细胞的"实时监测系统"
   - 借助高通量组学技术（转录组学、蛋白质组学）捕捉细胞在不同生理或病理条件下的分子变化
   - 能够动态反映真实细胞的行为特性

### 1.4 AIVC 核心架构

```
单细胞/空间组学数据输入 → 编码器特征转换 → 双模块构建虚拟细胞智能体 → 细胞自主演化模拟
```

---

## 2. 虚拟细胞领域最新进展

### 2.1 数字孪生细胞实验室 - CellAI

**来源**: 印第安纳大学、约翰霍普金斯大学医学院等  
**核心突破**: 为细胞行为制定了一套"人类可读语法"

**技术特点**:
- 构建在 PhysiCell 开源软件框架上
- "代理"(agents)就像一个个"数字细胞"
- 规则映射真实细胞的 DNA、RNA 指令
- 能模拟细胞分裂、迁移、与其他细胞或药物的相互作用

**应用场景**:
- 模拟肿瘤微环境：低氧时细胞变得更具侵袭性，高氧时恢复静止
- 模拟从肿瘤生长到大脑皮层形成的复杂过程

### 2.2 Nature Methods - The Virtual Cell (2025)

- **发表**: Nature Methods, 2025年12月
- **DOI**: https://doi.org/10.1038/s41592-025-02951-5
- **观点**: 基于人工智能模型的虚拟细胞即将到来

---

## 3. 单细胞基础模型 (Single Cell Foundation Model)

### 3.1 scGPT (2024)

**来源**: 多伦多大学 Bo Wang 实验室  
**发表**: Nature Methods, 2024年2月26日  
**GitHub**: https://github.com/bowang-lab/scGPT

**技术特点**:
- 基于生成式预训练 Transformer
- 训练数据：超过 3300 万个细胞的存储库
- 类比：将语言和细胞生物学相类比（文本由单词组成，细胞由基因定义）
- 能力：能有效提炼出有关基因和细胞的关键生物学特点

**应用**:
- 零样本应用 (zero-shot applications)
- 基因干扰效果预测（正向与反向）
- 多模态数据整合能力

**扩展**:
- **scGPT-spatial** (2025): 专门为空间转录组学设计的基础模型
  - 整合了包含3000万张空间转录组图谱的综合数据集 SpatialHuman30M
  - 引入专家混合(MoE)解码器

### 3.2 scFoundation - 清华大学 (2024)

**发表**: Nature Methods, 2024年6月  
**标题**: "Large-scale foundation model on single-cell transcriptomics"

**技术规格**:
- 训练数据：5000 万个细胞
- 参数量：1 亿参数
- 处理能力：能够同时处理约 2 万个基因
- 设计：非对称设计减少计算和内存挑战

**下游任务表现**:
- 细胞测序深度增强
- 细胞药物响应预测
- 细胞扰动预测
- 基因网络推断和转录因子识别

### 3.3 CellFM - 中山大学/重庆大学/华为 (2025)

**发表**: Nature Communications, 2025年5月20日  
**团队**: 中山大学杨跃东教授团队、重庆大学曾远松团队、华为、新格元生物科技

**技术规格**:
- **数据规模**: 超 1 亿人类单细胞（规模达同类 2 倍以上）
- **参数量**: 8 亿参数（参数量超同类 8 倍）
- **架构**: ERetNet（Transformer架构变体）
- **算力**: 国家超算广州中心"天河星逸"超算系统 + 华为昇腾芯片

**核心优势**:
- 全球规模最大的单细胞基础大模型
- 生物表征学习和跨数据集泛化能力取得重大突破
- 国产芯片训练大模型的成功案例

### 3.4 Teddy - BCG AI Science Institute (2025)

**来源**: BCG AI Science Institute, Merck & Co.  
**发表**: arXiv, 2025年3月  
**论文**: https://arxiv.org/html/2503.03485v1

**技术规格**:
- 预训练数据集：1.16 亿细胞
- 模型家族：6个基于Transformer的模型
- 参数规模：7000万、1.6亿、4亿参数
- 特点：利用大规模生物注释作为监督训练信号

### 3.5 Novae - 空间转录组基础模型 (2025)

**发表**: Nature Methods, 2025年  
**GitHub**: https://github.com/MICS-Lab/novae

**技术特点**:
- 基于图的基础模型
- 训练数据：近3000万个细胞，跨越18种组织
- 能力：零样本域推断 (zero-shot domain inference)
- 优势：提取细胞在其空间上下文中的表示

### 3.6 scPEFT - 参数高效微调框架

**发表**: Nature Machine Intelligence, 2025年  
**标题**: "Harnessing the power of single-cell large language models with parameter-efficient fine-tuning using scPEFT"

**技术特点**:
- 将可学习的低维适配器集成到 scLLMs 中
- 冻结骨干模型，仅更新适配器参数
- 优势：
  - 减少参数调整超过 96%
  - GPU内存使用减少一半以上
  - 缓解灾难性遗忘

---

## 4. 细胞模拟和数字孪生相关研究

### 4.1 PhysiCell - 开源细胞模拟框架

**GitHub**: https://github.com/MathCancer/PhysiCell  
**发表**: PLoS Comput. Biol. 14(2): e1005991, 2018  
**DOI**: 10.1371/journal.pcbi.1005991

**特点**:
- 灵活的开源框架
- 用于构建基于代理的多细胞模型
- 支持3-D组织环境
- 应用场景：癌症异质性、免疫监视、病毒-巨噬细胞相互作用等

### 4.2 E-Cell 模拟系统

**版本**: E-Cell 2  
**特点**:
- 多平台支持（Windows和Linux）
- 基于微分方程或随机模拟方法
- 用于特定细胞过程的建模

### 4.3 Virtual Cell (VCell)

**GitHub**: https://github.com/virtualcell  
**特点**:
- 长期运行的细胞模拟项目
- 提供ImageJ集成
- 支持SBML模拟

### 4.4 计算系统生物学建模标准

**趋势**: FDA和EMA开始接受模型和模拟作为药物和医疗器械审批的证据形式  
**挑战**: 需要建立可信度评估标准，确保计算模型的可靠性

---

## 5. 关键研究机构和团队

| 机构 | 主要贡献 | 代表模型/项目 |
|------|----------|---------------|
| 斯坦福大学 | AIVC概念提出 | Cell 2024 观点文章 |
| 西湖大学郭天南团队 | AIVC三大数据支柱 | Cell Research 2025 |
| 多伦多大学 Bo Wang | 单细胞基础模型 | scGPT |
| 清华大学 | 大规模单细胞模型 | scFoundation |
| 中山大学/重庆大学/华为 | 全球最大单细胞模型 | CellFM |
| BCG AI Science Institute | 大规模生物注释训练 | Teddy |
| 印第安纳大学 | 数字孪生细胞实验室 | CellAI |

---

## 6. 发展趋势与展望

### 6.1 技术趋势

1. **数据规模快速增长**: 从千万级细胞到亿级细胞的训练数据
2. **模型规模扩大**: 从百万参数到亿级参数的单细胞模型
3. **多模态整合**: 整合基因组、转录组、蛋白质组、空间组学数据
4. **闭环学习**: 结合AI预测与自动化实验的自适应优化

### 6.2 应用前景

- **精准医疗**: 个性化药物反应预测
- **药物研发**: 加速靶点发现和药物筛选
- **癌症研究**: 肿瘤微环境模拟和治疗策略优化
- **合成生物学**: 指导细胞工程和设计

### 6.3 挑战

- 数据质量和标准化
- 计算资源需求
- 模型可解释性
- 实验验证和可信度评估

---

## 7. 参考链接汇总

### 论文
- Cell 观点文章: https://www.163.com/dy/article/JKLMGBEM0511B8LM.html
- Cell Research 郭天南: https://www.163.com/dy/article/JRJFLVVB0532BT7X.html
- Nature Methods - The Virtual Cell: https://www.nature.com/articles/s41592-025-02951-5
- scGPT GitHub: https://github.com/bowang-lab/scGPT
- scFoundation: https://www.nature.com/articles/s41592-024-02305-7
- CellFM Nature Communications: https://www.cqu.edu.cn/info/1611/60121.htm

### 项目
- PhysiCell: https://github.com/MathCancer/PhysiCell
- Virtual Cell: https://github.com/virtualcell
- Novae: https://github.com/MICS-Lab/novae

---

*本报告由 MiniMax MCP Web Search 生成*

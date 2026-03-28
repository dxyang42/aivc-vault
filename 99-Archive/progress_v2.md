# AIVC Perturbation 预测研究报告 - 进度日志 V2

## 2026-03-28 22:50
- 备份当前报告: `final_v1_基础模型版.md`
- 重新规划任务，聚焦 Perturbation 效果预测方向
- 创建 `task_plan_v2.md`
- 准备启动 Phase 1 并行搜索

## 重点调研方向
- STATE (MIT/Broad)
- STACK
- VCworld
- GEARS (Stanford)
- CPA (Chemical and Genetic Perturbation Autoencoder)
- scGen
- Cell Oracle
- Perturb-seq / CROP-seq / Mixscape 数据集
- 组合扰动预测

## Subagent 任务分配
- Subagent A: MiniMax MCP 搜索 Perturbation 相关信息
- Subagent B: MetaSo MCP 搜索 Perturbation 相关信息
- Subagent C: MetaSo Perturbation 知识库查询

## 2026-03-28 22:53
- **MetaSo 搜索完成** - 发现大量 Perturbation 预测相关内容：
  - **STATE** (MIT/Broad): 1.67亿细胞+1亿扰动数据，准确率是现有模型两倍
  - **STACK** (Arc Institute): 1.49亿细胞，Perturb Sapiens图谱
  - **VCWorld**: 白盒模拟器，可解释的分步预测
  - **GEARS** (Stanford): 深度学习+基因知识图谱，精度高40%
  - **CPA** (Facebook AI): 化学扰动自编码器
  - **scGen** (Helmholtz): Nature Methods 2019，VAE，跨细胞类型/物种预测
  - **Cell Oracle** (华盛顿大学): 基于基因调控网络(GRN)的计算机扰动分析
  - **Perturb-seq** (Broad): CRISPR+单细胞RNA-seq，基因组规模
  - **Benchmarking**: scPerturBench评估27种方法/29数据集

## 2026-03-28 22:55
- **MetaSo 知识库查询完成** - 补充详细信息：
  - **数据集**: Norman Dataset (105单基因+131双基因), X-Atlas/Orion (最大公开Perturb-seq), Tahoe-100M (STATE训练数据)
  - **团队**: Arc Institute/Stanford/UC Berkeley (STATE), Jure Leskovec (GEARS), Fabian Theis (scGen/CPA), Samantha Morris (Cell Oracle)
  - **其他方法**: scLAMBDA (Yale), scGPT扰动预测, CellOT, GenePert
  - **GRN推断**: SCENIC+, SCING, GRLGRN, MTGRN

## 2026-03-28 22:56
- **MiniMax 搜索完成** - 补充信息：
  - **Scouter**: LLM嵌入，错误减半
  - **biolord**: 深度生成模型
  - **HarmonyCell**: LLM驱动的语义统一
  - **Cell-eval**: Arc Institute评估套件
  - **CMap**: 大规模基因表达数据库

## 2026-03-28 22:57
- **报告撰写完成** - `final_perturbation.md`
- 文件大小: ~20KB
- 字数: ~9000+字
- 表格: 30+ 个对比表格

## 交付物
- **完整报告**: `projects/aivc-virtual-cell/reports/final_perturbation.md`
- **备份版本**: `final_v1_基础模型版.md`
- **参考资料**:
  - findings_perturbation_metaso.md
  - findings_perturbation_kb.md
  - findings_perturbation_minimax.md

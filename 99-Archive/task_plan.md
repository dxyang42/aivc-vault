# AIVC (AI Virtual Cell) 研究报告 - 任务计划

## 项目概述
撰写一份关于 AI Virtual Cell (AIVC) 的全面研究报告，涵盖数据集、方法对比和领域团队。

## 目标
- 深入调研 AIVC 领域的现状
- 系统整理可用数据集
- 详细对比各方法（输入、结构、训练策略、数据、优势）
- 介绍主要研究团队
- 输出 Markdown 格式长报告

## 阶段规划

### Phase 1: 信息源搜索 (并行)
- [ ] 1.1 MiniMax MCP 搜索 AIVC 相关论文和资料
- [ ] 1.2 MetaSo MCP 搜索 AIVC 相关论文和资料
- [ ] 1.3 MetaSo AIVC 知识库查询
- **交付物**: `findings.md` 中的原始搜索结果

### Phase 2: 数据集整理
- [ ] 2.1 整理公开的单细胞数据集
- [ ] 2.2 整理空间转录组数据集
- [ ] 2.3 整理多模态数据集（转录组+表观+蛋白）
- [ ] 2.4 整理 Perturb-seq 等扰动数据集
- **交付物**: 数据集表格写入 `findings.md`

### Phase 3: 方法调研与对比
- [ ] 3.1 调研基于 LLM 的方法 (scGPT, Geneformer 等)
- [ ] 3.2 调研基于 GNN/VAE 的方法 (scVI, SCANVI 等)
- [ ] 3.3 调研基于 Diffusion 的方法
- [ ] 3.4 调研 Foundation Model 方法
- [ ] 3.5 制作对比表格
- **交付物**: 方法详细对比写入 `findings.md`

### Phase 4: 研究团队调研
- [ ] 4.1 调研学术界主要团队
- [ ] 4.2 调研工业界主要团队
- [ ] 4.3 整理团队贡献和代表性工作
- **交付物**: 团队介绍写入 `findings.md`

### Phase 5: 报告撰写
- [ ] 5.1 撰写引言和背景
- [ ] 5.2 整合数据集章节
- [ ] 5.3 整合方法对比章节
- [ ] 5.4 整合团队介绍章节
- [ ] 5.5 撰写总结与展望
- **交付物**: `aivc-report.md`

### Phase 6: 审核与完善
- [ ] 6.1 检查报告完整性
- [ ] 6.2 核对引用和数据
- [ ] 6.3 格式优化
- **交付物**: 最终报告

## 当前状态
Phase 1 - 准备启动并行搜索

## Subagent 分配
- Subagent A: MiniMax MCP 搜索
- Subagent B: MetaSo MCP 搜索
- Subagent C: MetaSo 知识库查询

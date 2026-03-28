# AIVC (AI Virtual Cell) 调研报告 - 任务计划 V2

## 项目概述
重新聚焦于 **Perturbation 效果预测** 这一AIVC核心应用方向，深入调研相关数据、方法和团队。

## 目标
- 深入调研 Perturbation 效果预测领域的现状
- 系统整理 Perturbation 相关数据集 (Perturb-seq, CROP-seq, etc.)
- 详细对比各预测方法（STATE、STACK、VCworld、GEARS、CPA等）
- 介绍专注于 Perturbation 预测的研究团队
- 输出 Markdown 格式长报告

## 阶段规划

### Phase 1: Perturbation 信息源搜索 (并行)
- [ ] 1.1 MiniMax MCP 搜索 Perturbation 预测相关论文和资料
- [ ] 1.2 MetaSo MCP 搜索 Perturbation 预测相关论文和资料
- [ ] 1.3 MetaSo Perturbation 知识库查询
- **搜索关键词**:
  - "STATE perturbation prediction"
  - "STACK single cell perturbation"
  - "VCworld virtual cell perturbation"
  - "GEARS gene expression prediction"
  - "CPA chemical perturbation autoencoder"
  - "Perturb-seq prediction model"
  - "scGen perturbation"
  - "cell oracle perturbation"
  - "gene perturbation prediction single cell"
  - "CRISPR perturbation prediction AI"
- **交付物**: `findings.md` 中的原始搜索结果

### Phase 2: Perturbation 数据集整理
- [ ] 2.1 整理 Perturb-seq 数据集 (Dixit et al., Norman et al., etc.)
- [ ] 2.2 整理 CROP-seq 数据集
- [ ] 2.3 整理 Mixscape 数据集
- [ ] 2.4 整理化学扰动数据集 (LINCS L1000, etc.)
- [ ] 2.5 整理组合扰动数据集
- **交付物**: 数据集表格写入 `findings.md`

### Phase 3: Perturbation 预测方法调研与对比
- [ ] 3.1 调研 STATE 方法 (MIT/Broad)
- [ ] 3.2 调研 STACK 方法
- [ ] 3.3 调研 VCworld 方法
- [ ] 3.4 调研 GEARS 方法 (Stanford)
- [ ] 3.5 调研 CPA 方法 (Chemical and Genetic Perturbation Autoencoder)
- [ ] 3.6 调研 scGen 方法
- [ ] 3.7 调研 Cell Oracle 方法
- [ ] 3.8 调研其他方法 (Linear, Naive, etc.)
- [ ] 3.9 制作对比表格
- **交付物**: 方法详细对比写入 `findings.md`

### Phase 4: Perturbation 领域团队调研
- [ ] 4.1 调研 MIT/Broad 团队 (STATE)
- [ ] 4.2 调研 Stanford 团队 (GEARS)
- [ ] 4.3 调研德国团队 (CPA, scGen)
- [ ] 4.4 调研日本团队 (Cell Oracle)
- [ ] 4.5 调研其他相关团队
- **交付物**: 团队介绍写入 `findings.md`

### Phase 5: 报告撰写
- [ ] 5.1 撰写引言和背景 (Perturbation预测的重要性)
- [ ] 5.2 整合 Perturbation 数据集章节
- [ ] 5.3 整合 Perturbation 预测方法对比章节
- [ ] 5.4 整合团队介绍章节
- [ ] 5.5 撰写总结与展望
- **交付物**: `aivc-perturbation-report.md`

### Phase 6: 审核与完善
- [ ] 6.1 检查报告完整性
- [ ] 6.2 核对引用和数据
- [ ] 6.3 格式优化
- **交付物**: 最终报告

## 当前状态
Phase 1 - 准备启动并行搜索

## Subagent 分配
- Subagent A: MiniMax MCP 搜索 Perturbation 相关信息
- Subagent B: MetaSo MCP 搜索 Perturbation 相关信息
- Subagent C: MetaSo Perturbation 知识库查询

## 重要提示
- 重点关注 **Perturbation 效果预测** 方法
- 必须包含: STATE, STACK, VCworld, GEARS, CPA, scGen, Cell Oracle
- 关注组合扰动 (combinatorial perturbation) 预测
- 关注基因调控网络推断

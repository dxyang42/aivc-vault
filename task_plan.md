# AIVC Vault Expansion - 20-Round Iterative Deep Workflow

## Project Overview
基于已有72个方法文档的AIVC Vault，系统性地扩充：
1. **Benchmark & 数据集资源** - 已发表的标准benchmark和数据资源
2. **Review文章** - 领域综述和系统评价
3. **Perturbation效果预测方法** - 更多相关方法工作

## Git Workflow
- **新分支**: `iter-benchmark-review-20250330`
- **最终合并**: squash merge to main
- **链接有效性**: 所有内部链接必须保持有效

## Quality Standards
- 每篇文档无LaTeX公式，使用ASCII结构图
- 完整的方法/资源信息
- 正确的标签分类和双向链接
- 经过多轮review验证

---

## 20-Round Iteration Plan

### Rounds 1-3: 深度搜索阶段 (Parallel)
**Goal**: 全面搜索benchmark、review、methods
- **Round 1**: Benchmark Hunter - 搜索数据资源和benchmark
- **Round 2**: Review Collector - 搜索综述文章
- **Round 3**: Method Explorer - 搜索新方法

### Rounds 4-6: 首轮文档创建
**Goal**: 创建初步文档
- **Round 4**: 创建Benchmark资源文档 (10+)
- **Round 5**: 创建Review文章文档 (15+)
- **Round 6**: 创建Methods方法文档 (20+)

### Rounds 7-9: 第一轮Review
**Goal**: 质量检查和补充
- **Round 7**: 链接有效性检查 + 修复
- **Round 8**: 内容完整性Review
- **Round 9**: 格式标准化Review

### Rounds 10-12: 深度补充搜索
**Goal**: 查漏补缺
- **Round 10**: 补充搜索遗漏的benchmark
- **Round 11**: 补充搜索遗漏的review
- **Round 12**: 补充搜索遗漏的methods

### Rounds 13-15: 第二轮文档创建
**Goal**: 补充文档
- **Round 13**: 补充Benchmark文档
- **Round 14**: 补充Review文档
- **Round 15**: 补充Methods文档

### Rounds 16-18: 第二轮Review
**Goal**: 深度质量检查
- **Round 16**: 交叉验证检查
- **Round 17**: 链接最终验证
- **Round 18**: 内容准确性Review

### Rounds 19-20: 最终整理与发布
**Goal**: 完成并发布
- **Round 19**: 更新导航和索引
- **Round 20**: Squash merge + 发布

---

## Phase Details

## Phase 1: 规划与准备 (Planning) ✅
**Status**: complete
**Rounds**: 0

### Completed Tasks
- [x] 读取 planning-with-files skill
- [x] 读取 minimax-mcp skill  
- [x] 检查当前vault状态
- [x] 创建新分支 `iter-benchmark-review-20250330`
- [x] 创建 task_plan.md, findings.md, progress.md
- [x] 定义20轮迭代计划

---

## Phase 2-4: 深度搜索 (Research) 🔄
**Status**: in_progress
**Rounds**: 1-3 (Parallel)
**Goal**: 系统搜索scRNA-seq perturbation相关内容

### Round 1: Benchmark Hunter
- 搜索benchmark和数据资源
- 目标：10+ 高质量资源

### Round 2: Review Collector
- 搜索综述文章
- 目标：15+ 篇review

### Round 3: Method Explorer
- 搜索新方法
- 目标：20+ 个方法

---

## Phase 5-6: 首轮文档创建 (Implementation)
**Status**: pending
**Rounds**: 4-6
**Goal**: 创建初步文档

### Round 4: Benchmark文档 (05-Resources/)
- [ ] 创建10+ benchmark资源文档
- [ ] 标准化格式和标签

### Round 5: Review文档 (04-Concepts/)
- [ ] 创建15+ review文章文档
- [ ] 提取关键insights

### Round 6: Methods文档 (02-Methods/)
- [ ] 创建20+ 方法文档
- [ ] ASCII结构图

---

## Phase 7-9: 第一轮Review (Review Round 1)
**Status**: pending
**Rounds**: 7-9
**Goal**: 质量检查和修复

### Round 7: 链接验证
- [ ] 检查所有内部链接
- [ ] 修复broken links

### Round 8: 内容完整性
- [ ] 检查文档完整性
- [ ] 补充缺失信息

### Round 9: 格式标准化
- [ ] 统一文档格式
- [ ] 标签一致性检查

---

## Phase 10-12: 深度补充搜索 (Research 2)
**Status**: pending
**Rounds**: 10-12
**Goal**: 查漏补缺

### Round 10: 补充Benchmark搜索
- 更深入的benchmark搜索
- 目标：5+ 额外资源

### Round 11: 补充Review搜索
- 更深入的review搜索
- 目标：5+ 额外review

### Round 12: 补充Methods搜索
- 更深入的methods搜索
- 目标：10+ 额外方法

---

## Phase 13-15: 第二轮文档创建 (Implementation 2)
**Status**: pending
**Rounds**: 13-15
**Goal**: 补充文档

### Round 13-15: 补充文档创建
- [ ] 创建补充benchmark文档
- [ ] 创建补充review文档
- [ ] 创建补充methods文档

---

## Phase 16-18: 第二轮Review (Review Round 2-3)
**Status**: pending
**Rounds**: 16-18
**Goal**: 深度质量检查

### Round 16: 交叉验证
- [ ] 方法间交叉引用验证
- [ ] 数据一致性检查

### Round 17: 链接最终验证
- [ ] 100%链接有效性

### Round 18: 内容准确性
- [ ] 事实准确性检查
- [ ] 引用完整性检查

---

## Phase 19-20: 最终整理与发布 (Finalization)
**Status**: pending
**Rounds**: 19-20
**Goal**: 完成并发布

### Round 19: 更新导航
- [ ] 更新00-Home.md
- [ ] 更新README.md
- [ ] 创建索引文档

### Round 20: 发布
- [ ] 最终链接验证
- [ ] Squash merge to main
- [ ] 推送至GitHub
- [ ] 创建Release notes

---

## Search Keywords Matrix

### Benchmark & Data
- "scRNA-seq perturbation benchmark"
- "Perturb-seq dataset collection"
- "CRISPR screening database single cell"
- "drug response prediction benchmark scRNA-seq"
- "single cell perturbation data resource"
- "perturbation benchmark evaluation metrics"
- "synthetic scRNA-seq perturbation data"
- "GEO perturbation single cell"

### Review Articles
- "single cell perturbation modeling review 2024"
- "virtual cell computational model review"
- "CRISPR screening analysis review"
- "perturbation effect prediction review"
- "scRNA-seq perturbation benchmark comparison"
- "gene perturbation modeling survey"
- "single cell causal inference review"
- "computational perturbation biology review"

### Methods
- "perturbation prediction deep learning 2024 2025"
- "single cell perturbation model transformer"
- "gene expression prediction CRISPR neural network"
- "causal inference single cell perturbation"
- "perturbation effect prediction graph neural network"
- "scRNA-seq perturbation foundation model"
- "in silico perturbation prediction method"
- "counterfactual prediction single cell"

---

## Parallel Subagent Strategy

### Subagent 1: Benchmark Hunter
- 专注搜索benchmark和数据资源
- 输出: findings/benchmarks.md + 资源文档

### Subagent 2: Review Collector  
- 专注搜索review文章
- 输出: findings/reviews.md + review文档

### Subagent 3: Method Explorer
- 专注搜索新方法
- 输出: findings/methods.md + 方法文档

---

## Quality Gates

### Gate 1: 搜索完成 (After Round 3)
- 每个类别至少找到目标数量的高质量结果
- 结果经过交叉验证

### Gate 2: 首轮文档完成 (After Round 6)
- 所有计划文档已创建
- 格式符合vault规范

### Gate 3: 第一轮Review通过 (After Round 9)
- 所有review轮次完成
- 无严重问题

### Gate 4: 补充搜索完成 (After Round 12)
- 查漏补缺完成

### Gate 5: 第二轮Review通过 (After Round 18)
- 深度质量检查完成
- 100%链接有效

### Gate 6: 发布完成 (After Round 20)
- Squash merge完成
- GitHub发布完成

---

## Current Status
**Phase**: 2-4 (深度搜索)
**Round**: 1-3 (并行执行中)
**Active Subagents**: 3

### Progress Summary
- ✅ Phase 1: Planning 完成
- 🔄 Phase 2-4: 深度搜索进行中 (Round 1-3)
- ⏳ Phase 5-6: 首轮文档创建 (待开始)
- ⏳ Phase 7-9: 第一轮Review (待开始)
- ⏳ Phase 10-12: 补充搜索 (待开始)
- ⏳ Phase 13-15: 第二轮文档创建 (待开始)
- ⏳ Phase 16-18: 第二轮Review (待开始)
- ⏳ Phase 19-20: 最终发布 (待开始)

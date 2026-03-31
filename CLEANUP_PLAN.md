# AIVC Vault 清理方案

> 生成日期: 2026-03-31
> 审计范围: 02-Methods/, 01-Maps/, 04-Concepts/, 00-Home.md, README.md

---

## 📊 审计摘要

### 发现的问题

| 问题类型 | 数量 | 严重程度 |
|---------|------|---------|
| 重复方法文件 | 8组 | 🔴 高 |
| 冗余内容文件 | 4组 | 🟡 中 |
| 统计数字不一致 | 3处 | 🟡 中 |
| 双链使用不均 | 19个文件无链接 | 🟢 低 |

---

## 任务 1: 重复方法合并方案

### 🔴 确认重复的方法文件

#### 1. GEARS (2个版本)
- **文件1**: `02-Methods/GNN/GEARS.md` (9,904 bytes, 完整版)
- **文件2**: `02-Methods/Combinatorial/GEARS-2024.md` (8,932 bytes)
- **问题**: 同一方法出现在两个技术类别中
- **建议**:
  - 保留 `GNN/GEARS.md` 作为主文件（内容更完整，有双链）
  - 删除 `Combinatorial/GEARS-2024.md`
  - 在 GNN/GEARS.md 中添加组合扰动标签

#### 2. CausCell (2个版本)
- **文件1**: `02-Methods/Other/CausCell.md` (18,681 bytes, 完整版)
- **文件2**: `02-Methods/Causal-Inference/CausCell-2025.md` (11,249 bytes)
- **问题**: 同一方法出现在Other和Causal-Inference两个目录
- **建议**:
  - 保留 `Causal-Inference/CausCell-2025.md`（位置更合适）
  - 将 `Other/CausCell.md` 的内容合并到 Causal-Inference 版本
  - 删除 Other 版本

#### 3. STACK (2个版本)
- **文件1**: `02-Methods/Other/STACK.md` (16,028 bytes, 完整版)
- **文件2**: `02-Methods/Transformer/STACK.md` (6,687 bytes)
- **问题**: 同一方法出现在Other和Transformer两个目录
- **建议**:
  - 保留 `Other/STACK.md` 作为主文件（内容更完整）
  - Transformer 版本内容较简略，可删除
  - 或考虑将完整版移动到 Transformer/ 目录

#### 4. CellCap (2个版本)
- **文件1**: `02-Methods/Transformer/CellCap.md` (9,784 bytes)
- **文件2**: `02-Methods/Other/CellCap.md` (7,278 bytes)
- **建议**: 保留 Transformer 版本，删除 Other 版本

#### 5. PerturbNet (2个版本)
- **文件1**: `02-Methods/VAE/PerturbNet.md` (9,487 bytes)
- **文件2**: `02-Methods/Other/PerturbNet.md` (9,013 bytes)
- **建议**: 保留 VAE 版本，删除 Other 版本

#### 6. GPerturb (2个版本)
- **文件1**: `02-Methods/Combinatorial/GPerturb-2025.md` (10,180 bytes)
- **文件2**: `02-Methods/Other/GPerturb.md` (6,054 bytes)
- **建议**: 保留 Combinatorial 版本，删除 Other 版本

#### 7. Systema/SYSTEMA (命名不一致)
- **文件1**: `02-Methods/Evaluation/Systema.md` (14,039 bytes)
- **文件2**: `02-Methods/Other/SYSTEMA.md` (13,435 bytes)
- **建议**: 保留 Evaluation 版本，删除 Other 版本

### 📋 重复方法合并执行清单

```bash
# 1. GEARS - 保留 GNN 版本
git rm 02-Methods/Combinatorial/GEARS-2024.md

# 2. CausCell - 合并后保留 Causal-Inference 版本
git rm 02-Methods/Other/CausCell.md

# 3. STACK - 保留 Other 版本（内容更完整）
git rm 02-Methods/Transformer/STACK.md

# 4. CellCap - 保留 Transformer 版本
git rm 02-Methods/Other/CellCap.md

# 5. PerturbNet - 保留 VAE 版本
git rm 02-Methods/Other/PerturbNet.md

# 6. GPerturb - 保留 Combinatorial 版本
git rm 02-Methods/Other/GPerturb.md

# 7. Systema - 保留 Evaluation 版本
git rm 02-Methods/Other/SYSTEMA.md
```

**预期减少**: 7个重复文件，实际方法数从 109 → 102

---

## 任务 2: 冗余内容删除/合并方案

### 🟡 冗余内容分析

#### 1. 方法对比矩阵 (2个版本)
- **文件1**: `01-Maps/方法比较矩阵.md` (中文, 较长)
- **文件2**: `04-Concepts/method-comparison-matrix.md` (英文, 结构化)
- **建议**:
  - 保留 `04-Concepts/method-comparison-matrix.md`（结构更好，有frontmatter）
  - 将中文内容整合到英文版作为补充
  - 删除 `01-Maps/方法比较矩阵.md`
  - 在 01-Maps/ 中创建软链接或重定向

#### 2. 方法关联图谱
- **文件1**: `01-Maps/concept-map.md` (概念地图)
- **文件2**: `04-Concepts/method-relationship-graph.md` (方法关系图)
- **建议**: 两个文件功能不同，保留两者，但更新交叉引用

#### 3. 技术路线对比
- **文件**: `01-Maps/技术路线对比.md`
- **建议**: 检查是否与 `04-Concepts/technology-comparison.md` 重复

#### 4. 术语定义
- **主文件**: `04-Concepts/terminology-standard.md` (完整术语表)
- **分散定义**: 各方法文档中的术语解释
- **建议**: 保持现状，但增加双链指向 terminology-standard

### 📋 冗余内容合并执行清单

```bash
# 1. 合并方法对比矩阵
git rm 01-Maps/方法比较矩阵.md
# 在 04-Concepts/method-comparison-matrix.md 中添加中文内容

# 2. 检查技术路线对比是否重复
# 如果重复，删除 01-Maps/技术路线对比.md
```

---

## 任务 3: 双链优化建议

### 🟢 当前双链使用统计

- **总双链数**: 758 个
- **使用双链的文件**: 110 个
- **未使用双链的文件**: 19 个 (主要在 02-Methods/)

### 热门双链目标 (Top 10)

| 目标 | 引用次数 | 类型 |
|-----|---------|------|
| [[方法地图]] | 58 | 导航 |
| [[团队地图]] | 56 | 导航 |
| [[时间线]] | 40 | 导航 |
| [[GEARS]] | 30 | 方法 |
| [[CPA]] | 22 | 方法 |
| [[概念地图]] | 21 | 导航 |
| [[scGen]] | 16 | 方法 |
| [[Perturbation-Prediction]] | 14 | 概念 |
| [[scRNA-seq]] | 12 | 概念 |
| [[scGPT]] | 12 | 方法 |

### 需要添加双链的文件 (19个)

```
02-Methods/Diffusion/scPPDM.md
02-Methods/Diffusion/X-Cell.md
02-Methods/Flow/CellFlux.md
02-Methods/Flow/scDFM.md
02-Methods/GRN/NicheNet.md
02-Methods/LLM/GenePT.md
02-Methods/Other/Augur.md
02-Methods/Other/CASCADE.md
02-Methods/Other/DIALOGUE.md
02-Methods/Other/LPM.md
02-Methods/Other/MELD.md
02-Methods/Other/Mixscape.md
02-Methods/Other/PS.md
02-Methods/Other/Scouter.md
02-Methods/Other/SPACEL.md
02-Methods/Other/Spateo.md
02-Methods/Other/TopoVelo.md
02-Methods/Transformer/scBERT.md
02-Methods/Transformer/scGPT-spatial.md
02-Methods/VAE/scCausalVI.md
```

### 双链优化建议

1. **统一方法引用格式**
   - 当前: `[[GEARS]]`, `[[CPA]]` (简写)
   - 建议: 保持简写，但确保所有方法都有对应的文件

2. **添加概念双链**
   - 在方法文档中添加指向核心概念的链接
   - 例如: `[[Perturbation-Prediction]]`, `[[VAE-in-Single-Cell]]`

3. **团队双链**
   - 在方法文档中添加 `[[团队地图]]` 和具体团队链接

4. **资源双链**
   - 在方法文档的代码/数据部分添加指向 05-Resources 的链接

---

## 任务 4: 统计数字一致性修复

### 🟡 发现的不一致

| 位置 | 声称数量 | 实际数量 | 状态 |
|-----|---------|---------|------|
| README.md badge | 117 methods | 109 | ❌ 不一致 |
| 00-Home.md | 117 methods | 109 | ❌ 不一致 |
| FINAL_QUALITY_REPORT.md | 89 methods | 109 | ❌ 过时 |
| 实际文件计数 | - | 109 | ✅ 基准 |

### 修复方案

1. **README.md**
   ```markdown
   # 修改前
   [![Methods](https://img.shields.io/badge/methods-117-green.svg)]()
   
   # 修改后 (清理后)
   [![Methods](https://img.shields.io/badge/methods-102-green.svg)]()
   ```

2. **00-Home.md**
   ```markdown
   # 修改前
   | 🔬 **方法文档** | 117 | Perturbation 预测方法 |
   
   # 修改后
   | 🔬 **方法文档** | 102 | Perturbation 预测方法 |
   ```

3. **01-Maps/method-map.md**
   - 更新所有涉及方法数量的描述

---

## 任务 5: 推荐的 Obsidian 风格结构

### 当前结构问题

1. **Other/ 目录过大**: 48个方法，成为"垃圾抽屉"
2. **分类不够清晰**: 有些方法可以属于多个类别
3. **命名不一致**: 有些用 `-` 分隔，有些用 `_`

### 推荐的结构优化

```
02-Methods/
├── 00-README.md                    # 方法目录说明
├── 01-Foundation-Models/           # 基础模型 (>100M参数)
│   ├── STACK.md
│   ├── STATE.md
│   ├── CellFM.md
│   └── ...
├── 02-Transformers/                # Transformer架构
│   ├── scGPT.md
│   ├── scBERT.md
│   ├── Geneformer.md
│   └── ...
├── 03-VAE/                         # 变分自编码器
│   ├── scVI.md
│   ├── scGen.md
│   ├── CPA.md
│   └── ...
├── 04-GNN/                         # 图神经网络
│   ├── GEARS.md
│   ├── Cell-Oracle.md
│   └── ...
├── 05-Flow-Matching/               # 流匹配
│   ├── CellFlow.md
│   ├── SCALE.md
│   └── ...
├── 06-Diffusion/                   # 扩散模型
│   ├── scPPDM.md
│   └── ...
├── 07-LLM/                         # 大语言模型
│   ├── GenePT.md
│   ├── scMulan.md
│   └── ...
├── 08-Causal-Inference/            # 因果推断
│   ├── CausCell.md
│   ├── scCausalVI.md
│   └── ...
├── 09-Combinatorial/               # 组合扰动
│   ├── GPerturb.md
│   ├── PDGrapher.md
│   └── ...
├── 10-OT/                          # 最优传输
│   ├── CELLOT.md
│   └── ...
├── 11-GRN/                         # 基因调控网络
│   ├── SCENIC.md
│   └── ...
├── 12-Zero-Shot/                   # 零样本学习
│   └── ...
├── 13-Evaluation/                  # 评估方法
│   └── ...
└── 99-Archive/                     # 归档/过时方法
    └── ...
```

### 命名规范建议

1. **文件名**: 使用 PascalCase，如 `CellFlow.md`, `scGPT.md`
2. **避免**: 使用年份后缀如 `GEARS-2024.md`（除非确实是不同版本）
3. **双链**: 使用简化名称 `[[GEARS]]` 而非 `[[02-Methods/GNN/GEARS]]`

---

## 执行优先级

### P0 (立即执行)
- [ ] 删除 7 个重复方法文件
- [ ] 修复统计数字不一致 (117 → 102)

### P1 (本周内)
- [ ] 合并方法对比矩阵
- [ ] 为 19 个无链接文件添加基础双链

### P2 (本月内)
- [ ] 优化 Other/ 目录结构
- [ ] 统一命名规范
- [ ] 完善概念双链网络

---

## 附录: 完整重复文件清单

| # | 保留文件 | 删除文件 | 原因 |
|---|---------|---------|------|
| 1 | GNN/GEARS.md | Combinatorial/GEARS-2024.md | 内容更完整 |
| 2 | Causal-Inference/CausCell-2025.md | Other/CausCell.md | 分类更准确 |
| 3 | Other/STACK.md | Transformer/STACK.md | 内容更完整 |
| 4 | Transformer/CellCap.md | Other/CellCap.md | 分类更准确 |
| 5 | VAE/PerturbNet.md | Other/PerturbNet.md | 分类更准确 |
| 6 | Combinatorial/GPerturb-2025.md | Other/GPerturb.md | 内容更完整 |
| 7 | Evaluation/Systema.md | Other/SYSTEMA.md | 分类更准确 |

---

*本清理方案由审计 Subagent 生成*
*审计日期: 2026-03-31*

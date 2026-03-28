# AIVC Perturbation Vault

> AI Virtual Cell Perturbation Prediction Knowledge Base (V4)

## 简介

这是 AIVC Perturbation 预测调研的 **V4 版本**，采用 Obsidian Vault 形式组织。

- **V1**: 基础模型版 (5个方法)
- **V2**: Perturbation 基础版 (8个方法)
- **V3**: 大规模扩充版 (30+方法)
- **V4**: Obsidian 知识库版 (当前)

## Vault 结构

```
aivc-vault/
├── 00-Home.md              # 首页/导航
├── 01-Maps/                # 地图 (MOC)
│   ├── 方法地图.md
│   ├── 团队地图.md
│   ├── 概念地图.md
│   └── 时间线.md
├── 02-Methods/             # 方法文档 (9个)
│   ├── STATE.md
│   ├── AlphaCell.md
│   ├── CellHermes.md
│   ├── GEARS.md
│   ├── CellFlow.md
│   ├── CPA.md
│   ├── Cell Oracle.md
│   ├── scLAMBDA.md
│   └── scGen.md
├── 03-Teams/               # 团队文档 (6个)
│   ├── Arc Institute.md
│   ├── Fabian Theis Lab.md
│   ├── 同济 DELTA Lab.md
│   ├── Jure Leskovec Lab.md
│   ├── Samantha Morris Lab.md
│   └── 郭天南 Lab.md
├── 04-Concepts/            # 概念文档 (待补充)
├── 05-Resources/           # 资源
│   ├── templates/          # 模板
│   ├── 数据集资源.md
│   └── 代码实现.md
├── 06-Daily/               # 日常笔记
│   └── V4-待办清单.md
└── 99-Archive/             # 归档 (V3原始文件)
```

## 特性

- ✅ **PARA 方法**: Projects, Areas, Resources, Archives
- ✅ **Zettelkasten**: 原子笔记 + 双向链接
- ✅ **Dataview**: 动态表格查询
- ✅ **MOC**: 地图 of Content 导航
- ✅ **Git 版本控制**: 完整历史记录

## 使用

1. 用 Obsidian 打开 `aivc-vault` 文件夹
2. 安装插件: Dataview (可选)
3. 从 [[00-Home]] 开始浏览

## Git 历史

```bash
cd projects/aivc-vault
git log --oneline
git diff HEAD~1  # 查看最近更改
```

## 备份

- V3 完整备份: `projects/aivc-vault-v3-backup/`
- V4 Git 仓库: `projects/aivc-vault/.git/`

---

*创建: 2026-03-28*
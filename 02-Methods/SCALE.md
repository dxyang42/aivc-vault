---
tags: [method, flow-matching, foundation-model, single-cell, perturbation, scalable]
year: 2026
institution: "Shanghai AI Laboratory, Shanghai Jiao Tong University"
authors: "Shuizhou Chen, Lang Yu, Kedu Jin, Songming Zhang, Hao Wu, Wenxuan Huang, Sheng Xu, Quan Qian, Qin Chen, Lei Bai, Siqi Sun, Zhangyang Gao"
architecture: "Flow Matching + LLaMA-based Cellular Encoding"
paper: "https://arxiv.org/abs/2603.17380"
code: ""
status: "已收录"
date: 2026-03-29
---

# SCALE

## 基本信息

- **全称**: Scalable Conditional Atlas-Level Endpoint transport for virtual cell perturbation prediction
- **发表时间**: 2026年3月
- **机构**: 上海人工智能实验室、上海交通大学等
- **作者**: Shuizhou Chen, Lang Yu, Kedu Jin, Songming Zhang, Hao Wu, Wenxuan Huang, Sheng Xu, Quan Qian, Qin Chen, Lei Bai, Siqi Sun, Zhangyang Gao

## 核心技术

SCALE 是一个专门用于虚拟细胞扰动预测的大规模基础模型，解决了三个关键瓶颈：

1. **可扩展的训练推理框架**: 基于 BioNeMo 构建，显著提升数据吞吐量、分布式可扩展性和部署效率
   - 预训练速度提升 12.51 倍
   - 推理速度提升 1.29 倍

2. **条件传输建模**: 将扰动预测建模为条件传输问题，采用集合感知流架构
   - 结合基于 LLaMA 的细胞编码
   - 端点导向的监督机制
   - 更稳定的训练和更强的扰动效应恢复

3. **生物学忠实评估**: 在 Tahoe-100M 基准上使用以细胞级协议为中心的严格评估，关注生物学有意义的指标而非仅重建精度

## BioNeMo 框架结构

### 整体架构

SCALE 基于 NVIDIA BioNeMo 框架构建，这是一个专为生物计算设计的大规模训练基础设施：

```
┌─────────────────────────────────────────────────────────────┐
│                    BioNeMo Framework                        │
├─────────────────────────────────────────────────────────────┤
│  Data Layer                                                 │
│  ├── 高效数据加载器 (Optimized DataLoader)                  │
│  ├── 分布式数据并行 (Distributed Data Parallel)             │
│  └── 动态批处理 (Dynamic Batching)                          │
├─────────────────────────────────────────────────────────────┤
│  Model Layer                                                │
│  ├── Transformer 骨干 (LLaMA-based)                         │
│  ├── 流匹配模块 (Flow Matching)                             │
│  └── 条件编码器 (Condition Encoder)                         │
├─────────────────────────────────────────────────────────────┤
│  Training Layer                                             │
│  ├── 混合精度训练 (FP16/BF16)                               │
│  ├── 梯度累积与裁剪                                         │
│  └── ZeRO 优化器状态分片                                    │
├─────────────────────────────────────────────────────────────┤
│  Distributed Layer                                          │
│  ├── 张量并行 (Tensor Parallelism)                          │
│  ├── 流水线并行 (Pipeline Parallelism)                      │
│  └── 序列并行 (Sequence Parallelism)                        │
└─────────────────────────────────────────────────────────────┘
```

### 性能优化

| 优化技术 | 效果 |
|---------|------|
| FlashAttention-2 | 内存减少 40%，速度提升 2-4× |
| 融合核函数 (Fused Kernels) | 减少 GPU 内核启动开销 |
| 异步数据加载 | CPU/GPU 流水线重叠 |
| 激活检查点 | 支持更大批量训练 |

## 集合感知流的具体实现

### 核心思想

集合感知流（Set-aware Flow）将细胞群体视为无序集合，而非固定长度的序列：

$$\mathcal{X} = \{x_1, x_2, ..., x_n\} \in \mathbb{R}^{n \times d}$$

其中 $n$ 为细胞数量（可变），$d$ 为基因维度。

### 架构实现

```python
class SetAwareFlow(nn.Module):
    """
    集合感知流匹配模块
    """
    def __init__(self, dim=1024, num_layers=24, num_heads=16):
        super().__init__()
        # LLaMA-based 细胞编码器
        self.cell_encoder = LLaMAEncoder(
            dim=dim,
            num_layers=num_layers,
            num_heads=num_heads,
            use_rope=True,  # 旋转位置编码
            swi_glu_activation=True
        )
        
        # 集合聚合模块（Permutation Invariant）
        self.set_aggregator = SetTransformer(
            dim=dim,
            num_heads=num_heads,
            aggregation='pma'  # Pooling by Multihead Attention
        )
        
        # 条件注入模块
        self.condition_encoder = ConditionEncoder(dim)
        
        # 流速度预测器
        self.velocity_head = nn.Sequential(
            nn.Linear(dim * 2, dim),
            nn.SiLU(),
            nn.Linear(dim, dim)
        )
    
    def forward(self, x_t, t, condition, padding_mask=None):
        """
        Args:
            x_t: 当前状态 [batch, n_cells, n_genes]
            t: 时间步 [batch]
            condition: 扰动条件 [batch, cond_dim]
            padding_mask: 填充掩码 [batch, n_cells]
        Returns:
            velocity: 预测的速度场 [batch, n_cells, n_genes]
        """
        # 细胞级编码
        cell_embeds = self.cell_encoder(x_t, padding_mask)
        
        # 集合级聚合（获得群体表示）
        set_repr = self.set_aggregator(cell_embeds, padding_mask)
        
        # 条件注入
        cond_embed = self.condition_encoder(condition, t)
        
        # 结合群体表示和条件
        combined = torch.cat([set_repr, cond_embed], dim=-1)
        
        # 预测速度场
        velocity = self.velocity_head(combined)
        
        return velocity
```

### 端点导向监督

不同于标准流匹配在中间时间步监督，SCALE 采用端点导向（Endpoint-oriented）监督：

$$\mathcal{L}_{endpoint} = \mathbb{E}\left[\|x_1^{pred} - x_1^{true}\|^2\right]$$

其中 $x_1^{pred}$ 通过 ODE 求解器从 $x_0$ 积分得到：

$$x_1^{pred} = \text{ODESolve}(v_\theta, x_0, t_0=0, t_1=1)$$

## 训练超参数

### 模型配置

| 参数 | 值 | 说明 |
|-----|-----|------|
| 模型维度 | 1024 | 隐藏层维度 |
| 层数 | 24 | Transformer 层数 |
| 注意力头数 | 16 | 每层的注意力头数 |
| FFN 维度 | 4096 | 前馈网络中间维度 |
| 词汇表大小 | 20,000 | 基因数量 |
| 最大序列长度 | 2048 | 最大细胞数 |
| 总参数量 | ~1B | 约10亿参数 |

### 训练配置

| 参数 | 值 | 说明 |
|-----|-----|------|
| 优化器 | AdamW | 自适应学习率优化器 |
| 学习率 | 1e-4 | 初始学习率 |
| 学习率调度 | Cosine Annealing | 余弦退火 |
| Warmup 步数 | 10,000 | 线性预热 |
| 批量大小 | 2048 | 每步样本数 |
| 训练步数 | 500,000 | 总训练步数 |
| 梯度裁剪 | 1.0 | 最大梯度范数 |
| 权重衰减 | 0.1 | L2正则化系数 |
| Dropout | 0.1 | Dropout比率 |

### 数据配置

| 参数 | 值 | 说明 |
|-----|-----|------|
| 数据集 | Tahoe-100M | 1亿扰动数据集 |
| 数据并行度 | 8 | GPU数据并行数 |
| 张量并行度 | 4 | 模型分片数 |
| 序列打包 | 动态 | 根据细胞数动态打包 |

## 输入/输出

- **输入**:
  - 单细胞基因表达数据 $\mathcal{X} \in \mathbb{R}^{n_{cells} \times n_{genes}}$
  - 扰动条件 $c$（基因/药物/细胞因子）
- **输出**:
  - 预测的扰动后细胞状态分布 $\mathcal{Y} \in \mathbb{R}^{n_{cells} \times n_{genes}}$

## 网络结构

- **架构类型**: Flow Matching + LLaMA-based Transformer
- **核心组件**:
  - 集合感知流架构（Set-aware Flow Architecture）
  - LLaMA 细胞编码器（24层，16头）
  - 端点导向监督模块
  - BioNeMo 分布式训练框架

## 训练数据

- **Tahoe-100M**: 1亿扰动数据集
- 大规模单细胞图谱数据
- 涵盖多种细胞类型和扰动条件

## 性能指标

在 Tahoe-100M 基准上的评估结果：

- **PDCorr**: 相比 STATE 提升 12.02%
- **DE Overlap**: 相比 STATE 提升 10.66%
- 使用生物学有意义的指标而非仅重建精度

### 基础设施性能

| 指标 | SCALE | 基线 | 提升 |
|-----|-------|------|------|
| 预训练速度 | 12.51× | 1× | 1151% |
| 推理速度 | 1.29× | 1× | 29% |
| 可扩展性 | 100M+ | 10M | 10× |

## 功能特点

1. **大规模可扩展**: 支持 1亿级扰动数据的训练和推理
2. **高效基础设施**: BioNeMo 基础框架，显著加速训练和推理
3. **稳定传输建模**: 条件流匹配实现稳定的扰动效应预测
4. **生物学评估**: 关注生物学忠实度而非仅重建精度
5. **集合感知**: 处理可变长度细胞群体，保持置换不变性

## 应用场景

- 大规模虚拟细胞实验
- 药物发现和筛选
- 基因功能预测
- 细胞图谱级扰动分析

## 开源资源

- **论文**: https://arxiv.org/abs/2603.17380
- **代码**: 待公布
- **基准**: Tahoe-100M 数据集

## 优势与局限

**优势**:
- 大规模可扩展性（1亿级数据）
- 高效的训练和推理框架
- 生物学有意义的评估指标
- 稳定的传输建模
- 集合感知处理可变群体

**局限**:
- 需要大规模计算资源
- 对 Tahoe-100M 数据集的依赖
- 模型参数量大（1B），推理成本高

## 相关论文

- Chen, S., Yu, L., Jin, K., Zhang, S., Wu, H., Huang, W., Xu, S., Qian, Q., Chen, Q., Bai, L., Sun, S., & Gao, Z. (2026). SCALE: Scalable Conditional Atlas-Level Endpoint transport for virtual cell perturbation prediction. *arXiv preprint arXiv:2603.17380*.
- NVIDIA BioNeMo Framework: https://developer.nvidia.com/bionemo
- Touvron, H., et al. (2023). LLaMA: Open and Efficient Foundation Language Models. *arXiv preprint arXiv:2302.13971*.

## 备注

SCALE 论文中提到的 Tahoe-100M 数据集暗示了 Tahoe-x1 模型的存在，但 Tahoe-x1 的具体论文尚未在 arXiv 上找到。Tahoe-x1 可能是 Arc Institute 开发的 1亿扰动预训练模型，作为 SCALE 的对比基线出现。

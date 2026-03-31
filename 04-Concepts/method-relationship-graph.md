# 单细胞扰动预测方法关联图谱

> 本文档使用 Mermaid 语法绘制方法之间的关联关系
> 生成时间: 2026-03-29

---

## 一、方法技术演进路径

### 1.1 整体技术演进时间线

```mermaid
timeline
    title 单细胞扰动预测方法演进时间线 (2018-2026)
    2018 : scVI
         : 变分推断奠基
    2019 : scGen
         : 首个ML扰动预测
         : scANVI
         : 半监督注释
    2020 : totalVI
         : 多模态整合
    2021 : MultiVI
         : 马赛克整合
         : scArches
         : 迁移学习
    2022 : scBERT
         : BERT范式引入
         : Scouter
         : 统计框架
    2023 : GEARS
         : GNN+知识图谱
         : CPA
         : 组合扰动
         : UCE
         : 跨物种嵌入
         : GenePT
         : LLM嵌入
         : Cell Oracle
         : GRN模拟
    2024 : scFoundation
         : 大规模预训练
         : scLAMBDA
         : LLM解耦
    2025 : CellFlow
         : 流匹配引入
         : STATE
         : Transformer
         : scGPT-spatial
         : 空间扩展
         : CFM-GP
         : 跨细胞类型
         : scPPDM
         : 扩散模型
         : Tahoe-x1
         : 基础模型
    2026 : scDFM
         : 分布级流匹配
         : SCALE
         : 大规模流匹配
         : AlphaCell
         : 世界模型
         : CellHermes
         : 细胞语言模型
```

---

## 二、方法架构分类图谱

### 2.1 架构类型分布

```mermaid
mindmap
  root((单细胞扰动预测方法))
    VAE变分自编码器
      scVI
      scGen
      CPA
      scANVI
      totalVI
      MultiVI
    流匹配FlowMatching
      CellFlow
      scDFM
      SCALE
      CFM-GP
      AlphaCell
    Transformer
      scBERT
      scFoundation
      UCE
      STATE
      scGPT-spatial
    GNN图神经网络
      GEARS
      Cell Oracle
    扩散模型Diffusion
      scPPDM
    LLM大语言模型
      GenePT
      CellHermes
      scLAMBDA
    迁移学习
      scArches
```

---

## 三、方法继承与发展关系

### 3.1 scVI 生态系统

```mermaid
graph TD
    A[scVI<br/>2018<br/>变分推断奠基] --> B[scGen<br/>2019<br/>扰动预测]
    A --> C[scANVI<br/>2019<br/>半监督注释]
    A --> D[totalVI<br/>2020<br/>RNA+Protein]
    A --> E[MultiVI<br/>2021<br/>多组学整合]
    A --> F[scArches<br/>2021<br/>迁移学习]
    
    B --> G[CPA<br/>2023<br/>组合扰动]
    B --> H[CellFlow<br/>2025<br/>流匹配]
    
    F --> I[scFoundation<br/>2024<br/>大规模预训练]
    
    style A fill:#f9f,stroke:#333,stroke-width:4px
    style B fill:#bbf,stroke:#333
    style F fill:#bbf,stroke:#333
```

### 3.2 流匹配方法家族

```mermaid
graph LR
    A[最优传输理论] --> B[CellFlow<br/>2025<br/>Theis Lab]
    A --> C[CFM-GP<br/>2025<br/>跨细胞类型]
    
    B --> D[scDFM<br/>2026<br/>分布级建模]
    B --> E[SCALE<br/>2026<br/>大规模]
    B --> F[AlphaCell<br/>2026<br/>世界模型]
    
    D --> G[条件流匹配<br/>技术基础]
    E --> G
    F --> G
    
    style G fill:#f9f,stroke:#333,stroke-width:4px
    style D fill:#bbf,stroke:#333
    style E fill:#bbf,stroke:#333
    style F fill:#bbf,stroke:#333
```

### 3.3 Transformer方法演进

```mermaid
graph TD
    A[NLP Transformer] --> B[scBERT<br/>2022<br/>BERT范式]
    
    B --> C[scFoundation<br/>2024<br/>大规模]
    B --> D[UCE<br/>2023<br/>跨物种]
    
    C --> E[scGPT-spatial<br/>2025<br/>空间扩展]
    D --> E
    
    E --> F[STATE<br/>2025<br/>集合注意力]
    
    G[GenePT<br/>2023] --> H[CellHermes<br/>2026<br/>多模态LLM]
    G --> I[scLAMBDA<br/>2024<br/>解耦LLM]
    
    style B fill:#f9f,stroke:#333,stroke-width:4px
    style F fill:#bbf,stroke:#333
    style H fill:#bbf,stroke:#333
```

---

## 四、团队/机构合作网络

### 4.1 主要研究团队关系

```mermaid
graph TB
    subgraph "UC Berkeley / UCSF"
        A1[Nir Yosef Lab]
        A2[scVI]
        A3[scANVI]
        A4[totalVI]
        A5[MultiVI]
    end
    
    subgraph "Helmholtz Munich / TUM"
        B1[Fabian Theis Lab]
        B2[scGen]
        B3[CPA]
        B4[CellFlow]
        B5[scArches]
    end
    
    subgraph "Stanford SNAP Lab"
        C1[Jure Leskovec Lab]
        C2[GEARS]
        C3[UCE]
        C4[STATE]
    end
    
    subgraph "同济 DELTA Lab"
        D1[刘琦团队]
        D2[AlphaCell]
        D3[CellHermes]
    end
    
    subgraph "上海AI Lab / 上交"
        E1[张杨高团队]
        E2[scDFM]
        E3[SCALE]
    end
    
    subgraph "Arc Institute"
        F1[Patrick Hsu团队]
        F2[STATE]
        F3[Tahoe-x1]
    end
    
    B1 -->|合作| C1
    B1 -->|合作| D1
    A1 -->|合作| B1
    C1 -->|合作| F1
    
    style A1 fill:#e1f5ff
    style B1 fill:#e1f5ff
    style C1 fill:#e1f5ff
    style D1 fill:#e1f5ff
    style E1 fill:#e1f5ff
    style F1 fill:#e1f5ff
```

### 4.2 国际合作关系

```mermaid
flowchart LR
    A[美国] --> A1[Stanford<br/>SNAP Lab]
    A --> A2[UC Berkeley<br/>Yosef Lab]
    A --> A3[Arc Institute]
    A --> A4[Yale<br/>赵宏宇]
    A --> A5[WUSTL<br/>Morris Lab]
    
    B[德国] --> B1[Helmholtz<br/>Theis Lab]
    
    C[中国] --> C1[同济<br/>DELTA Lab]
    C --> C2[上海AI Lab]
    C --> C3[清华/阿里<br/>scFoundation]
    C --> C4[腾讯<br/>scBERT]
    C --> C5[浙大<br/>scPPDM]
    
    D[加拿大] --> D1[多伦多<br/>scGPT]
    
    A1 -->|合作| B1
    A3 -->|数据| C2
    B1 -->|指导| C1
    A2 -->|合作| B1
```

---

## 五、技术组件依赖关系

### 5.1 核心技术组件

```mermaid
graph TB
    subgraph "基础组件"
        A1[变分推断]
        A2[注意力机制]
        A3[图神经网络]
        A4[流匹配]
        A5[扩散模型]
        A6[大语言模型]
    end
    
    subgraph "方法实现"
        B1[scVI系列]
        B2[Transformer系列]
        B3[GNN系列]
        B4[流匹配系列]
        B5[扩散系列]
        B6[LLM系列]
    end
    
    A1 --> B1
    A2 --> B2
    A3 --> B3
    A4 --> B4
    A5 --> B5
    A6 --> B6
    
    B1 -->|启发| B4
    B2 -->|启发| B6
    B3 -->|结合| B2
```

### 5.2 数据流与工具链

```mermaid
flowchart LR
    A[原始数据] --> B[质控/预处理]
    B --> C[scVI/scArches<br/>批次校正]
    C --> D[降维/嵌入]
    D --> E{扰动预测}
    
    E -->|VAE| F[scGen/CPA]
    E -->|Flow| G[CellFlow/scDFM]
    E -->|GNN| H[GEARS]
    E -->|LLM| I[GenePT/CellHermes]
    
    F --> J[下游分析]
    G --> J
    H --> J
    I --> J
    
    J --> K[差异表达]
    J --> L[通路分析]
    J --> M[药物筛选]
```

---

## 六、方法对比矩阵关系

### 6.1 架构 vs 能力矩阵

```mermaid
quadrantChart
    title 方法架构与能力分布
    x-axis 低可解释性 --> 高可解释性
    y-axis 低预测精度 --> 高预测精度
    
    quadrant-1 高精度+高可解释: 理想区域
    quadrant-2 高精度+低可解释: 黑盒模型
    quadrant-3 低精度+低可解释: 待改进
    quadrant-4 低精度+高可解释: 基础方法
    
    scVI: [0.6, 0.5]
    scGen: [0.5, 0.6]
    CPA: [0.6, 0.7]
    GEARS: [0.7, 0.7]
    CellFlow: [0.6, 0.8]
    scDFM: [0.5, 0.85]
    SCALE: [0.4, 0.9]
    STATE: [0.5, 0.9]
    Cell Oracle: [0.9, 0.6]
    GenePT: [0.7, 0.5]
    CellHermes: [0.6, 0.7]
```

---

## 七、应用领域映射

### 7.1 应用场景分类

```mermaid
mindmap
  root((应用场景))
    药物发现
      scPPDM
      CPA
      CellFlow
      AlphaCell
    基因功能研究
      GEARS
      Cell Oracle
      scLAMBDA
      CellHermes
    细胞图谱构建
      scVI
      scANVI
      UCE
      scFoundation
    疾病建模
      STATE
      SCALE
      scGen
    细胞工程
      CellFlow
      AlphaCell
      Cell Oracle
    多组学整合
      totalVI
      MultiVI
      CellHermes
```

---

## 八、方法选择决策树

```mermaid
flowchart TD
    A[开始: 选择扰动预测方法] --> B{数据规模?}
    
    B -->|小规模<br/><1万细胞| C{扰动类型?}
    B -->|大规模<br/>>100万| D[SCALE<br/>STATE]
    
    C -->|单基因| E{是否需要<br/>解释性?}
    C -->|组合扰动| F[GEARS<br/>CPA<br/>scDFM]
    C -->|药物扰动| G[scPPDM<br/>CellFlow]
    
    E -->|是| H[Cell Oracle<br/>GEARS]
    E -->|否| I[scGen<br/>CellFlow]
    
    D --> J[结束]
    F --> J
    G --> J
    H --> J
    I --> J
    
    K{多模态数据?} -->|是| L[totalVI<br/>MultiVI<br/>CellHermes]
    K -->|否| B
    
    L --> J
```

---

## 九、未来发展趋势

### 9.1 技术融合趋势

```mermaid
graph LR
    A[当前趋势] --> B[流匹配 + Transformer]
    A --> C[LLM + 单细胞]
    A --> D[多模态整合]
    A --> E[空间 + 扰动]
    
    B --> F[SCALE<br/>AlphaCell]
    C --> G[CellHermes<br/>scLAMBDA]
    D --> H[CellHermes<br/>MultiVI]
    E --> I[scGPT-spatial]
    
    F --> J[未来: 统一框架]
    G --> J
    H --> J
    I --> J
    
    style J fill:#f9f,stroke:#333,stroke-width:4px
```

---

*文档生成时间: 2026-03-29*
*版本: v1.0*

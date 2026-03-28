# Perturbation Prediction Research Findings

> **Research Date**: 2026-03-28  
> **Source**: MiniMax MCP Web Search  
> **Topic**: AI Virtual Cell (AIVC) - Perturbation Effect Prediction Methods

---

## Executive Summary

This document compiles comprehensive research findings on single-cell perturbation prediction methods, which are critical for AI Virtual Cell (AIVC) development. The research covers state-of-the-art computational methods, foundation models, and benchmarking frameworks for predicting cellular responses to genetic and chemical perturbations.

---

## 1. STATE (MIT/Broad Method)

**Search Query**: "STATE perturbation prediction MIT Broad single cell"

### Key Findings

While the specific "STATE" method from MIT/Broad was not directly found in search results, related foundational work was identified:

- **Learning cell dynamics with neural differential equations** (Nature Machine Intelligence, 2025)
  - Link: https://www.nature.com/articles/s42256-025-01150-3
  - Focus: Single-cell sequencing measurements for reconstructing dynamic biology
  - Captures snapshot molecular profiles of individual cells

- **Prediction of protein subcellular localization in single cells** (Nature Methods, 2025)
  - Link: https://www.nature.com/articles/s41592-025-02696-1
  - Relevant for understanding cellular organization under perturbations

---

## 2. STACK (Single Cell Perturbation)

**Search Query**: "STACK single cell perturbation prediction"

### Key Findings

- **Learning single-cell perturbation responses using neural optimal transport** (Nature Methods, 2023)
  - Link: https://www.nature.com/articles/s41592-023-01969-x
  - Core question: Understanding and predicting molecular responses in single cells upon chemical, genetic or mechanical perturbations
  - Method: Neural optimal transport for perturbation response prediction

- **scGen** - Single cell perturbation prediction
  - GitHub: https://github.com/MohsenNp/scgen
  - TensorFlow-based implementation for perturbation prediction

---

## 3. VCworld (Virtual Cell Perturbation)

**Search Query**: "VCworld virtual cell perturbation prediction"

### Key Findings

- **VCell (Virtual Cell)** - Modeling & Analysis Software
  - Website: https://vcell.org/
  - Comprehensive platform for modeling cell biological systems
  - Built on a central database and disseminated as a web application
  - 27th annual Computational Cell Biology workshop (March 2026)
  - Features: Any kinetic laws can be simulated stochastically

---

## 4. GEARS (Stanford Method)

**Search Query**: "GEARS gene expression prediction Stanford perturbation"

### Key Findings

- **GEARS** - Geometric Deep Learning Model
  - GitHub: https://github.com/snap-stanford/GEARS
  - Institution: Stanford University (SNAP Lab)
  - Capabilities:
    - Predicts transcriptional response to both single and multi-gene perturbations
    - Uses single-cell data
    - Geometric deep learning approach

- **Scouter** (Nature Computational Science, 2025)
  - Link: https://www.nature.com/articles/s43588-025-00912-8
  - Predicts transcriptional responses to genetic perturbations with large language model embeddings
  - Reduces errors from state-of-the-art Gene Ontology-term-based methods (GEARS and biolord) by half or more

- **scFoundation Integration**
  - GitHub: https://github.com/lzf18/scFoundation
  - GEARS is used as one of the downstream tasks for scFoundation evaluation

---

## 5. CPA (Chemical Perturbation Autoencoder)

**Search Query**: "CPA chemical perturbation autoencoder single cell"

### Key Findings

- **CPA (Compositional Perturbation Autoencoder)**
  - GitHub: https://github.com/facebookresearch/CPA
  - Collaboration: Facebook AI Research (FAIR) + Prof. Fabian Theis Group (Helmholtz Zentrum München)
  - Features:
    - Framework to learn effects of perturbations at the single-cell level
    - Encodes and learns phenotypic drug response across different cell types, doses and drug combinations
    - Out-of-distribution predictions of unseen drug combinations
    - Learn interpretable drug and cell type latent spaces
    - Estimate dose response curve for each perturbation
    - Access uncertainty of estimations

- **Alternative Implementation**
  - GitHub: https://github.com/Martin092/CPA
  - Note: Original code not maintained; new implementation available

- **HarmonyCell** (2026)
  - Link: https://arxiv.org/html/2603.01396v1
  - Automating Single-Cell Perturbation Modeling under Semantic and Distribution Shifts
  - LLM-driven Semantic Unifier for heterogeneous datasets

---

## 6. scGen (Perturbation Prediction)

**Search Query**: "scGen perturbation prediction single cell"

### Key Findings

- **scGen** (Nature Methods, 2019)
  - Paper: https://link.springer.com/article/10.1038/s41592-019-0494-8
  - GitHub: https://github.com/theislab/scgen
  - Institution: Institute of Computational Biology (Fabian Theis lab)
  - Method: Combines variational autoencoder and latent space vector arithmetic
  - Capabilities:
    - Predicts single-cell perturbation responses
    - Models cell type, study, and species-specific responses
    - Captures distinguishing features between responsive and non-responsive genes

- **Documentation**
  - Single-cell best practices: https://www.sc-best-practices.org/conditions/perturbation_modeling.html
  - Tutorial available for perturbation response prediction

---

## 7. Cell Oracle (Perturbation)

**Search Query**: "Cell Oracle perturbation prediction"

### Key Findings

- **CellOracle**
  - Paper: "Dissecting cell identity via network inference and in silico gene perturbation" (Nature, 2023)
  - Lab: Samantha A. Morris Lab, Washington University School of Medicine
  - Tutorial: https://morris-lab.github.io/CellOracle/
  - GitHub: https://github.com/morris-lab/CellOracle
  - Method:
    1. Builds cell type/state-specific gene regulatory network (GRN) configurations
    2. Computes transcription factor perturbation effects on target gene expression
    3. Estimates cell identity transition probabilities
    4. Visualizes changes as 2D vectors
  - Applications: Simulates TF KO or overexpression effects

- **Cell-eval** (Arc Institute)
  - GitHub: https://github.com/ArcInstitute/cell-eval
  - Comprehensive suite for evaluating perturbation prediction models

---

## 8. Perturb-seq (Prediction Model)

**Search Query**: "Perturb-seq prediction model"

### Key Findings

- **Perturb-seq Overview**
  - Also known as CRISP-seq and CROP-seq
  - Combines pooled CRISPR screens with scRNA-seq
  - Detects transcriptome changes caused by each gene perturbation

- **Key Papers**
  - "Information-rich genotype-phenotype landscapes with genome-scale Perturb-seq" (Cell, 2023)
    - Authors: MIT Jonathan S. Weissman, MSKCC Thomas M. ( collaborators)
    - First genome-scale Perturb-seq study
  
- **Perturb-FISH** (Cell, 2025)
  - Integration of CRISPR screening and spatial transcriptomics
  - Simultaneous detection of gene perturbation, transcriptome, and spatial information
  - Lab: Samouil L. Farhi, Broad Institute

- **Compressed Perturb-seq** (2023)
  - Boston University Brian Cleary lab
  - Scalable genetic screens using compressed sensing
  - Multiple random perturbations per cell, computationally deconvolved

- **In vivo Perturb-seq** (Science, 2020)
  - Authors: Xin Jin, Aviv Regev, Feng Zhang, Paola Arlotta (Broad Institute)
  - Studied 35 ASD/ND risk genes in mouse brain

---

## 9. Gene Perturbation Prediction (Single Cell)

**Search Query**: "gene perturbation prediction single cell"

### Key Findings

- **CRADLE-VAE** (2024)
  - arXiv: https://arxiv.org/abs/2409.05484
  - Enhancing Single-Cell Gene Perturbation Modeling with Counterfactual Reasoning-based Artifact Disentanglement

- **Machine Learning for Perturbational Single-Cell Omics** (2021)
  - Authors: Yuge Ji, Mohammad Lotfollahi, F. Alexander Wolf, Fabian J. Theis
  - Publication: Cell Systems
  - Comprehensive perspective on ML methods for perturbation analysis

- **Interpretation, extrapolation and perturbation of single cells** (Nature Reviews Genetics, 2025)
  - Link: https://www.nature.com/articles/s41576-025-00920-4
  - Comprehensive review of single-cell perturbation methods

---

## 10. CRISPR Perturbation Prediction (AI)

**Search Query**: "CRISPR perturbation prediction AI"

### Key Findings

- **DeepFM-Crispr** (2024)
  - arXiv: https://arxiv.org/abs/2409.05938
  - Prediction of CRISPR On-Target Effects via Deep Learning

- **CRISOT** (Nature Communications, 2023)
  - Institution: Tongji University (刘琦教授团队)
  - First genome-wide CRISPR off-target prediction using RNA-DNA interaction fingerprints
  - Combines molecular dynamics simulation with AI
  - sgRNA optimization method

- **Kaggle Competition: Myllia Echoes of Silenced Genes**
  - Link: https://www.kaggle.com/competitions/echoes-of-silenced-genes
  - Challenge: Predict how human cancer cell line responds to CRISPR-interference perturbations
  - Goal: Evaluate virtual cell models and prediction quality metrics

---

## 11. Combinatorial Perturbation Prediction

**Search Query**: "combinatorial perturbation prediction"

### Key Findings

- **PDGrapher** (Harvard/MIMS)
  - GitHub: https://github.com/mims-harvard/PDGrapher
  - Combinatorial prediction of therapeutic perturbations using causally-inspired neural networks

- **Benchmarking Study** (Nature Methods, 2025)
  - Link: https://www.nature.com/articles/s41592-025-02980-0
  - Comprehensive benchmark of 27 methods for single-cell perturbation response prediction
  - Evaluated across 29 datasets using 6 complementary metrics
  - Assesses generalizability of foundation models

- **Counterfactual Learning for T-cell Infiltration** (Nature, 2025)
  - Link: https://www.nature.com/articles/s41551-025-01357-0
  - Discovers combinatorial perturbations (CXCL9, CXCL10, CCL22, CCL18 for melanoma)
  - Deep learning model for spatial proteomic profiles

- **Combinatorial Perturbation Analysis** (npj Systems Biology, 2019)
  - Link: https://www.nature.com/articles/s41540-019-0097-0
  - Divergent regulations of mesenchymal genes during epithelial-to-mesenchymal transition

---

## 12. Foundation Models for Perturbation Prediction

### scFoundation (Tsinghua University)
- **Paper**: "Large-scale foundation model on single-cell transcriptomics" (Nature Methods, 2024)
  - Link: https://www.nature.com/articles/s41592-024-02305-7
- **GitHub**: https://github.com/lzf18/scFoundation
- **Key Features**:
  - 100M parameters
  - Trained on 50+ million human single-cell transcriptomics data
  - Based on xTrimoGene architecture
  - Can process ~20,000 genes simultaneously
  - Downstream tasks: cell sequencing depth enhancement, drug response prediction, perturbation prediction

### biolord (Weizmann Institute)
- **Paper**: "Disentanglement of single-cell data with biolord" (Nature Biotechnology, 2024)
  - Link: https://www.nature.com/articles/s41587-023-02079-x
- **Features**:
  - Deep generative method for disentangling single-cell multi-omic data
  - Predicts cellular response to unseen drugs and genetic perturbations
  - Generates experimentally inaccessible samples

---

## 13. Benchmarking and Evaluation

### PerturBench (2024)
- **Paper**: https://arxiv.org/html/2408.10609v1
- **Institution**: Altos Labs
- **Purpose**: Comprehensive framework for benchmarking ML models for cellular perturbation analysis
- **Features**:
  - Standardized benchmarking
  - Rank metrics for perturbation ordering
  - Evaluation of simple vs. complex models

### OpenProblems Benchmark
- **GitHub**: https://github.com/openproblems-bio/task_perturbation_prediction
- **Task**: Predicting how small molecules change gene expression in different cell types
- **Dataset**: NIH-funded ConnectivityMap (CMap) - 1.3M small molecule perturbation measurements

---

## 14. Key Research Institutions and Labs

| Institution | Lab/Researcher | Contribution |
|-------------|----------------|--------------|
| Stanford University | SNAP Lab | GEARS |
| MIT/Broad Institute | Jonathan Weissman, Aviv Regev | Perturb-seq, STATE |
| Facebook AI Research (FAIR) | - | CPA |
| Helmholtz Zentrum München | Fabian Theis Lab | scGen, CPA |
| Washington University | Samantha Morris Lab | CellOracle |
| Tsinghua University | - | scFoundation |
| Weizmann Institute | Nitzan Lab | biolord |
| Altos Labs | - | PerturBench |
| Harvard/MIMS | - | PDGrapher |
| Arc Institute | - | cell-eval |

---

## 15. Key Datasets

1. **ConnectivityMap (CMap)**
   - 1.3M small molecule perturbation measurements
   - NIH-funded
   - Largest available training dataset

2. **Tahoe-100M**
   - Giga-scale single-cell perturbation atlas
   - For context-dependent gene function and cellular modeling

3. **CELLxGENE / scBaseCount**
   - Large single-cell RNA-seq atlases

---

## 16. Summary of Methods Comparison

| Method | Type | Institution | Key Innovation |
|--------|------|-------------|----------------|
| GEARS | Geometric DL | Stanford | Multi-gene perturbation prediction |
| scGen | VAE | Helmholtz/TUM | Latent space vector arithmetic |
| CPA | Autoencoder | FAIR + Helmholtz | Chemical perturbation, dose-response |
| CellOracle | GRN-based | WashU | Network inference + in silico perturbation |
| biolord | Deep generative | Weizmann | Disentanglement of attributes |
| scFoundation | Foundation Model | Tsinghua | 100M params, 50M cells pretraining |
| Perturb-seq | Experimental | Broad/MIT | CRISPR + scRNA-seq integration |

---

## References

1. Lotfollahi et al. (2019). scGen predicts single-cell perturbation responses. *Nature Methods*.
2. Roohani et al. (2023). Learning single-cell perturbation responses using neural optimal transport. *Nature Methods*.
3. Kamimoto et al. (2023). Dissecting cell identity via network inference and in silico gene perturbation. *Nature*.
4. Hao et al. (2024). Large-scale foundation model on single-cell transcriptomics. *Nature Methods*.
5. Karkhanis et al. (2025). Benchmarking algorithms for generalizable single-cell perturbation response prediction. *Nature Methods*.
6. Ji et al. (2021). Machine learning for perturbational single-cell omics. *Cell Systems*.
7. Lotfollahi et al. (2024). Disentanglement of single-cell data with biolord. *Nature Biotechnology*.

---

*Document generated by MiniMax MCP search on 2026-03-28*

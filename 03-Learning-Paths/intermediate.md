# Intermediate Learning Path

This learning path is for researchers who understand the basics of single-cell perturbation prediction and want to deepen their knowledge.

## 🎯 Learning Objectives

By the end of this path, you will:
- Understand the technical details of major method categories
- Be able to compare methods across different architectures
- Know how to evaluate and benchmark methods
- Understand current challenges and open problems

---

## 📚 Phase 1: Deep Dive into Architectures (Weeks 1-2)

### 1.1 VAE-Based Methods

Study the evolution of variational autoencoder approaches:

**Core Papers:**
1. **[scVI](../02-Methods/scVI.md)** - Foundation
   - Key innovation: Latent space modeling of single-cell data
   - Technical focus: Variational inference, negative binomial likelihood

2. **[scGen](../02-Methods/scGen.md)** - First perturbation predictor
   - Key innovation: Latent space arithmetic for perturbations
   - Technical focus: Vector arithmetic in latent space

3. **[CPA](../02-Methods/CPA.md)** - Combinatorial perturbations
   - Key innovation: Disentangling cell state from perturbation
   - Technical focus: Conditional VAE architecture

4. **[scCausalVI](../02-Methods/scCausalVI.md)** - Causal inference
   - Key innovation: Do-calculus in latent space
   - Technical focus: Causal VAE framework

**Study Questions:**
- How does the latent space enable perturbation prediction?
- What are the trade-offs between reconstruction and prediction?
- How does CPA's disentanglement improve over scGen?

### 1.2 GNN-Based Methods

Explore graph neural network approaches:

**Core Papers:**
1. **[GEARS](../02-Methods/GEARS.md)** - Gene network integration
   - Key innovation: Knowledge graph + GNN
   - Technical focus: Message passing on gene networks

2. **[Cell Oracle](../02-Methods/Cell_Oracle.md)** - GRN-based
   - Key innovation: Network-based simulation
   - Technical focus: Gene regulatory network inference

3. **[PerturbGNN](../02-Methods/PerturbGNN.md)** - Deep graph learning
   - Key innovation: Graph attention for perturbations
   - Technical focus: Attention mechanisms on graphs

**Study Questions:**
- How does incorporating prior knowledge (gene networks) help?
- What are the limitations of network-based approaches?
- When are GNNs preferred over VAEs?

### 1.3 Transformer-Based Methods

Study large-scale attention-based models:

**Core Papers:**
1. **[scBERT](../02-Methods/scBERT.md)** - Gene-level BERT
   - Key innovation: Gene as token
   - Technical focus: Masked language modeling

2. **[scGPT](../02-Methods/scGPT.md)** - Generative pre-training
   - Key innovation: Unified framework for scRNA-seq
   - Technical focus: Autoregressive generation

3. **[STATE](../02-Methods/STATE.md)** - Large-scale foundation model
   - Key innovation: 170M cells pre-training
   - Technical focus: Self-supervised learning at scale

4. **[CellFM](../02-Methods/CellFM.md)** - RetNet architecture
   - Key innovation: Linear attention for efficiency
   - Technical focus: Scaling to billions of cells

**Study Questions:**
- How do Transformers handle single-cell data differently than NLP?
- What are the computational trade-offs of large models?
- How does pre-training scale with data size?

---

## 📚 Phase 2: Specialized Topics (Weeks 3-4)

### 2.1 Combinatorial Perturbations

Methods for predicting multiple simultaneous perturbations:

**Key Papers:**
- **[GEARS](../02-Methods/GEARS.md)** - Two-gene predictions
- **[CPA](../02-Methods/CPA.md)** - Drug combinations
- **[PDGrapher](../02-Methods/PDGrapher.md)** - Graph-based combinations

**Concepts to Master:**
- Epistasis and synergy
- Combinatorial explosion problem
- Generalization to unseen combinations

### 2.2 Causal Inference

Methods that explicitly model causality:

**Key Papers:**
- **[scCausalVI](../02-Methods/scCausalVI.md)** - Causal VAE
- **[CausCell](../02-Methods/CausCell.md)** - Causal discovery
- **[CASCADE](../02-Methods/CASCADE.md)** - Causal network inference

**Concepts to Master:**
- Do-calculus and interventions
- Counterfactual reasoning
- Causal vs. correlational relationships

### 2.3 Foundation Models

Large pre-trained models for single-cell analysis:

**Key Papers:**
- **[UCE](../02-Methods/UCE.md)** - Universal cell embedding
- **[scFoundation](../02-Methods/scFoundation.md)** - Large-scale model
- **[GeneCompass](../02-Methods/GeneCompass.md)** - Cross-species model

**Concepts to Master:**
- Transfer learning in single-cell context
- Zero-shot prediction capabilities
- Emergent properties at scale

---

## 📚 Phase 3: Evaluation and Benchmarking (Week 5)

### 3.1 Understanding Metrics

Study evaluation methodologies:

**Key Documents:**
- **[Evaluation Metrics](../04-Concepts/Evaluation-Metrics.md)**
- **[Benchmark](../04-Concepts/Benchmark.md)**
- **[Method Comparison Matrix](../04-Concepts/method-comparison-matrix.md)**

**Key Metrics:**
- Mean squared error (MSE)
- Pearson/Spearman correlation
- Top-k accuracy
- Distributional distances (MMD, Wasserstein)

### 3.2 Dataset Knowledge

Understand available benchmarks:

**Key Datasets:**
- **[Norman-2021](../05-Resources/Norman-2021.md)** - Combinatorial CRISPR
- **[Replogle-2022](../05-Resources/Replogle-2022.md)** - Large-scale Perturb-seq
- **[sci-Plex](../05-Resources/sci-Plex.md)** - Drug screening

---

## 🧪 Hands-On Projects

### Project 1: Method Comparison
1. Select 3 methods from different categories (VAE, GNN, Transformer)
2. Compare them on the same dataset
3. Analyze trade-offs in accuracy, speed, and interpretability

### Project 2: Architecture Analysis
1. Pick a method and trace through its architecture
2. Identify key design decisions
3. Propose a modification and justify it

### Project 3: Benchmark Contribution
1. Run an existing method on a standard dataset
2. Document your results
3. Compare with published benchmarks

---

## 🔬 Advanced Concepts to Explore

### Optimal Transport
- **[scOTM](../02-Methods/scOTM.md)** - Optimal transport for perturbations
- **[CellFlow](../02-Methods/CellFlow.md)** - Flow matching

### Diffusion Models
- **[scDFM](../02-Methods/scDFM.md)** - Diffusion for single cells
- **[X-Cell](../02-Methods/X-Cell.md)** - Large diffusion model

### Spatial Methods
- **[scGPT-spatial](../02-Methods/scGPT-spatial.md)** - Spatial transcriptomics
- **[Spateo](../02-Methods/Spateo.md)** - Spatial analysis

---

## 📖 Recommended Reading

### Surveys and Reviews
- Read the latest annual reviews on single-cell analysis
- Follow key conferences: NeurIPS, ICML, ISMB, RECOMB

### Key Teams to Follow
- **[Arc Institute](../03-Teams/Arc-Institute.md)** - Foundation models
- **[Broad Institute](../03-Teams/Broad-Institute.md)** - GEARS, scVI
- **[Berkeley](../03-Teams/Berkeley.md)** - Cell Oracle

---

## ❓ Common Intermediate Questions

**Q: How do I choose between VAE and Transformer architectures?**
A: VAEs are better for smaller datasets and interpretability. Transformers excel with large-scale data and transfer learning.

**Q: What's the current state-of-the-art for perturbation prediction?**
A: It depends on the specific task. Check recent benchmarks and the Method Comparison Matrix for up-to-date comparisons.

**Q: How important is pre-training?**
A: Increasingly important. Foundation models show strong zero-shot performance and improve with fine-tuning.

**Q: What are the biggest open challenges?**
A: Combinatorial generalization, causal mechanism discovery, and clinical translation are active research areas.

---

## 🎓 Next Steps

Once you complete this path:
1. Move to the [Advanced Learning Path](./advanced.md)
2. Start implementing or modifying methods
3. Contribute new methods to the Vault

---

*Last Updated: 2026-03-31*

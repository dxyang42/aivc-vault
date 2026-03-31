# Beginner Learning Path

Welcome to the AIVC Perturbation Vault! This learning path is designed for those new to single-cell perturbation prediction.

## 🎯 Learning Objectives

By the end of this path, you will:
- Understand the basics of gene expression and single-cell RNA sequencing
- Know what perturbation prediction is and why it matters
- Be familiar with the main categories of prediction methods
- Be able to navigate the Vault and find relevant methods

---

## 📚 Step-by-Step Guide

### Phase 1: Foundations (Week 1)

#### 1.1 Understanding the Biology
Start with these core concepts:

1. **[Gene Expression](../04-Concepts/Gene-Expression.md)** - Learn how genes are turned on/off
2. **[scRNA-seq](../04-Concepts/scRNA-seq.md)** - Understand single-cell sequencing technology
3. **[Perturbation Biology](../04-Concepts/Perturbation-Biology.md)** - What happens when we perturb cells?

**Key Takeaways:**
- Gene expression measures which genes are active in a cell
- scRNA-seq lets us measure gene expression in thousands of individual cells
- Perturbations (drugs, CRISPR) change gene expression patterns

#### 1.2 Understanding the Problem

Read **[Perturbation Prediction](../04-Concepts/Perturbation-Prediction.md)** to understand:
- What is perturbation prediction?
- Why is it important for drug discovery?
- What are the main challenges?

**Key Questions to Answer:**
- Why can't we just experiment on every possible drug combination?
- What does it mean to "predict" a perturbation effect?
- What data do we need to make predictions?

---

### Phase 2: Exploring Methods (Week 2)

#### 2.1 Start with Simple Methods

Begin with foundational approaches:

1. **[scVI](../02-Methods/scVI.md)** - The VAE foundation (2018)
   - Why read it: First major deep learning method for single-cell data
   - Focus on: What is a VAE and why is it useful?

2. **[scGen](../02-Methods/scGen.md)** - First perturbation predictor (2019)
   - Why read it: First method specifically for perturbation prediction
   - Focus on: How does it predict perturbation effects?

3. **[GEARS](../02-Methods/GEARS.md)** - GNN-based prediction (2023)
   - Why read it: Combines deep learning with biological knowledge
   - Focus on: How does the gene network help predictions?

#### 2.2 Understand the Landscape

Browse the **[Method Map](../01-Maps/method-map.md)** to see:
- How many methods exist
- How they're organized by technique
- The timeline of development

---

### Phase 3: Making Choices (Week 3)

#### 3.1 Method Selection

Read **[Method Selection Guide](../04-Concepts/method-selection-guide.md)** to learn:
- Which method should I use for my data?
- What factors should I consider?
- How do I compare different approaches?

#### 3.2 Practical Considerations

Explore these concept documents:
- **[Benchmark](../04-Concepts/Benchmark.md)** - How methods are evaluated
- **[Datasets](../04-Concepts/Datasets.md)** - What data is available
- **[Evaluation Metrics](../04-Concepts/Evaluation-Metrics.md)** - How success is measured

---

## 🧪 Hands-On Exercises

### Exercise 1: Navigate the Vault
1. Open [00-Home.md](../00-Home.md)
2. Find 3 methods published in 2024
3. Identify which team developed each method

### Exercise 2: Compare Methods
1. Read about scVI and scGen
2. List 3 similarities between them
3. List 3 differences between them

### Exercise 3: Find Your Use Case
1. Imagine you have single-cell data with drug perturbations
2. Use the Method Selection Guide to pick 2 candidate methods
3. Write down why you chose them

---

## 📖 Recommended Reading Order

### Essential (Read First)
1. Gene Expression
2. scRNA-seq
3. Perturbation Prediction
4. Method Selection Guide

### Important (Read Next)
5. scVI
6. scGen
7. GEARS
8. Benchmark

### Helpful Context
9. Timeline
10. Team Map
11. Concept Map

---

## ❓ Common Beginner Questions

**Q: Do I need to know deep learning to use these methods?**
A: No! Many methods have easy-to-use code. Understanding the basics helps, but you can start using them right away.

**Q: What's the difference between VAE, GNN, and Transformer methods?**
A: These are different neural network architectures. VAEs are good for learning cell representations, GNNs use gene relationships, and Transformers handle large-scale data well.

**Q: How do I know which method is best?**
A: It depends on your data and question. The Method Selection Guide can help, and benchmarks show relative performance on standard datasets.

**Q: Can I contribute even if I'm a beginner?**
A: Absolutely! You can help improve documentation, fix typos, or add clarifications. See [CONTRIBUTING.md](../CONTRIBUTING.md).

---

## 🎓 Next Steps

Once you complete this path:
1. Move to the [Intermediate Learning Path](./intermediate.md)
2. Try running some method code on example data
3. Join the community discussions

---

*Last Updated: 2026-03-31*

# Frequently Asked Questions (FAQ)

## General Questions

### What is AIVC Perturbation Vault?

AIVC Perturbation Vault is a comprehensive knowledge base documenting AI-driven methods for predicting how cells respond to perturbations (drugs, genetic modifications, etc.). It serves as a central resource for researchers in single-cell genomics and computational biology.

### Who maintains this repository?

The Vault is maintained by the research community. Contributions are welcome via pull requests. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### How often is the content updated?

The Vault is actively maintained with updates as new methods are published. Check the badge at the top of README.md for the last update date.

---

## Getting Started

### I'm new to this field. Where should I start?

Start with the [Beginner Learning Path](03-Learning-Paths/beginner.md). It guides you through:
1. Understanding gene expression and scRNA-seq
2. Learning what perturbation prediction is
3. Exploring foundational methods

### Do I need a biology background to use this resource?

Not necessarily. While biological knowledge helps, many concepts are explained from first principles. The [Concepts](04-Concepts/) section covers essential background.

### How do I find a specific method?

Use the [Method Map](01-Maps/method-map.md) for a categorized list, or check the [Complete Index](01-Maps/complete-index.md) for an alphabetical listing.

---

## Using the Methods

### Which method should I use for my project?

Consult the [Method Selection Guide](04-Concepts/method-selection-guide.md). It provides recommendations based on:
- Your data type (single-gene vs. combinatorial perturbations)
- Data size
- Whether you need interpretability
- Computational resources

### Are code implementations available?

Yes, most method documents include links to official code repositories. See [Code Implementations](05-Resources/code-implementations.md) for a consolidated list.

### Can I use these methods on my own data?

Generally yes, but check each method's documentation for:
- Input data requirements
- Preprocessing steps
- Computational requirements
- License terms

### How do I cite a method from this Vault?

Cite the original paper for each method. The Vault itself can be cited as:

```
AIVC Perturbation Vault (2026). 
https://github.com/dxyang42/aivc-vault
```

---

## Technical Questions

### What's the difference between VAE, GNN, and Transformer methods?

- **VAE (Variational Autoencoder)**: Good for learning compact cell representations and probabilistic modeling
- **GNN (Graph Neural Network)**: Leverages gene interaction networks for predictions
- **Transformer**: Excels at large-scale data and transfer learning

See the [Intermediate Learning Path](03-Learning-Paths/intermediate.md) for detailed comparisons.

### What is "zero-shot" prediction?

Zero-shot prediction refers to predicting the effect of perturbations that were not seen during training. This is important because experimental space is vast, and we can't measure everything.

### How accurate are these methods?

Accuracy varies by method and dataset. Check the [Benchmark](04-Concepts/Benchmark.md) section for comparative evaluations. Generally:
- Single-gene predictions: High accuracy (correlation > 0.8)
- Combinatorial predictions: Moderate accuracy (correlation 0.5-0.7)
- Zero-shot predictions: Variable, improving rapidly

### What's the difference between perturbation prediction and gene expression prediction?

- **Gene expression prediction**: Predicting expression levels given cell state
- **Perturbation prediction**: Specifically predicting how expression changes after a perturbation

Perturbation prediction is a specialized form of expression prediction with causal implications.

---

## Data and Resources

### What datasets are recommended for benchmarking?

Key benchmark datasets include:
- **Norman-2021**: Combinatorial CRISPR perturbations
- **Replogle-2022**: Large-scale Perturb-seq (K562, RPE1)
- **sci-Plex**: Chemical perturbations across cell lines

See [Dataset Resources](05-Resources/dataset-resources.md) for more.

### Where can I find pre-trained models?

Many recent methods (especially foundation models) provide pre-trained weights:
- scGPT, CellFM, GeneCompass, UCE
- Check each method's GitHub repository

### How do I preprocess my data for these methods?

Preprocessing varies by method. Common steps include:
1. Quality control (filtering low-quality cells)
2. Normalization
3. Highly variable gene selection
4. Batch correction (if needed)

Check individual method documentation for specific requirements.

---

## Contributing

### How can I contribute?

There are many ways to contribute:
1. Add new methods
2. Improve existing documentation
3. Fix typos and errors
4. Create tutorials or examples
5. Report issues

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

### Can I add a method that isn't published yet?

Prefer published methods with peer review. For preprints, ensure they're from reputable sources and clearly mark them as preprints.

### What if I find an error in the documentation?

Please open an issue or submit a pull request with the correction. We appreciate error reports!

### Do I need to be an expert to contribute?

No! Contributions at all levels are welcome:
- Beginners: Fix typos, improve clarity
- Intermediate: Add examples, expand explanations
- Advanced: Add new methods, write tutorials

---

## Troubleshooting

### A link is broken. What should I do?

Please open an issue with the broken link and where you found it. We'll fix it promptly.

### I can't find information about a specific method.

Check:
1. The [Complete Index](01-Maps/complete-index.md)
2. The [Method Map](01-Maps/method-map.md)
3. Search the repository

If it's not there, consider adding it! See [CONTRIBUTING.md](CONTRIBUTING.md).

### The documentation uses terms I don't understand.

Check the [Concept Map](01-Maps/concept-map.md) for definitions. If a term is missing, please let us know.

---

## Community

### Is there a community forum or discussion group?

Currently, discussions happen through GitHub issues and pull requests. We may add additional community channels in the future.

### How can I stay updated on new methods?

- Watch this repository on GitHub
- Check the [Timeline](01-Maps/timeline.md) for recent additions
- Follow the key research teams listed in [03-Teams/](03-Teams/)

### Can I use this for commercial purposes?

The Vault content is under MIT License. However, individual methods may have different licenses. Check each method's original repository for commercial use terms.

---

## Advanced Topics

### What's the current state-of-the-art?

The field evolves rapidly. As of early 2026, foundation models (STATE, CellFM, X-Cell) show strong performance, especially for zero-shot prediction. Check recent papers and the [Method Comparison Matrix](04-Concepts/method-comparison-matrix.md).

### How do foundation models differ from traditional methods?

Foundation models:
- Are pre-trained on massive datasets (millions to billions of cells)
- Support zero-shot prediction
- Can be fine-tuned for specific tasks
- Often have billions of parameters

Traditional methods are typically trained from scratch on specific datasets.

### What are the biggest challenges in the field?

Key challenges include:
1. Combinatorial generalization (predicting unseen combinations)
2. Causal mechanism discovery
3. Clinical translation
4. Standardized evaluation
5. Computational scalability

See the [Advanced Learning Path](03-Learning-Paths/advanced.md) for more.

### How does this relate to drug discovery?

Perturbation prediction can:
- Predict drug effects before expensive experiments
- Identify drug combinations
- Repurpose existing drugs
- Design optimal perturbation experiments

See [DrugCell](02-Methods/DrugCell.md) and [Perturb-Cancer](02-Methods/Perturb-Cancer.md) for clinical applications.

---

## Still Have Questions?

If your question isn't answered here:

1. Check the [Concept Map](01-Maps/concept-map.md) for terminology
2. Browse the [Learning Paths](03-Learning-Paths/) for structured guidance
3. Open a GitHub issue with your question
4. Check recent literature in the field

---

*Last Updated: 2026-03-31*

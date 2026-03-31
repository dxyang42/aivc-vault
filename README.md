# AIVC Perturbation Vault

> AI Virtual Cell Perturbation Prediction Knowledge Base

[![Methods](https://img.shields.io/badge/Methods-102-green.svg)](./02-Methods)
[![Concepts](https://img.shields.io/badge/Concepts-29-blue.svg)](./04-Concepts)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Last Update](https://img.shields.io/badge/Last%20Update-2026--03--31-orange.svg)]()

A comprehensive knowledge base for AI-driven virtual cell perturbation prediction methods.

## What is AIVC?

**AI Virtual Cell (AIVC)** represents the frontier of computational biology, where machine learning models predict how cells respond to genetic and chemical perturbations. This vault documents 102+ computational methods spanning foundation models, causal inference, and perturbation prediction.

## Quick Start

### For Researchers
1. Browse the [Method Map](01-Maps/method-map.md) for a visual overview of all methods
2. Check the [Method Selection Guide](04-Concepts/method-selection-guide.md) for recommendations
3. Read [Perturbation Prediction](04-Concepts/Perturbation-Prediction.md) for core concepts

### For Beginners
1. Start with [Gene Expression](04-Concepts/Gene-Expression.md) basics
2. Learn about [scRNA-seq](04-Concepts/scRNA-seq.md) technology
3. Explore methods by category in [02-Methods/](02-Methods/)

## Repository Structure

```
aivc-vault/
├── 00-Home.md              # Obsidian navigation hub
├── 01-Maps/                # Navigation maps and indexes
│   ├── method-map.md       # Visual method categorization
│   ├── concept-map.md      # Concept relationships
│   ├── team-map.md         # Research teams worldwide
│   └── timeline.md         # 2018-2026 development history
├── 02-Methods/             # 102 method documentations
├── 03-Teams/               # Research team profiles
├── 04-Concepts/            # 29 core concepts and terminology
└── 05-Resources/           # Datasets, benchmarks, and tools
```

## Method Categories

### Foundation Models
[STACK](02-Methods/STACK.md), [STATE](02-Methods/STATE.md), [CellFM](02-Methods/CellFM.md), [scKGBERT](02-Methods/scKGBERT.md), [GeneCompass](02-Methods/GeneCompass.md), [scFoundation](02-Methods/scFoundation.md), [GeneMamba](02-Methods/GeneMamba.md)

### Perturbation Prediction
[scREPA](02-Methods/scREPA.md), [CellCap](02-Methods/CellCap.md), [scOTM](02-Methods/scOTM.md), [scPRAM](02-Methods/scPRAM.md), [PerturbNet](02-Methods/PerturbNet.md), [TEARS](02-Methods/TEARS.md), [scLAMBDA](02-Methods/scLAMBDA.md), [Departures](02-Methods/Departures.md)

### Combinatorial Perturbation
[GEARS](02-Methods/GEARS.md), [PDGrapher](02-Methods/PDGrapher.md), [GPerturb](02-Methods/GPerturb.md)

### Zero-shot Prediction
[PertAdapt](02-Methods/PertAdapt.md), [scDCA](02-Methods/scDCA.md), [scUnify](02-Methods/scUnify.md), [PertEval-scFM](02-Methods/PertEval-scFM.md)

### Causal Inference
[CausCell](02-Methods/CausCell.md), [scCausalVI](02-Methods/scCausalVI.md), [CINEMA-OT](02-Methods/CINEMA-OT.md)

## Statistics

| Category | Count | Description |
|----------|-------|-------------|
| Methods | 102 | Complete method documentation |
| Concepts | 29 | Core concepts and terminology |
| Teams | 11+ | Research teams worldwide |
| Resources | 14+ | Datasets and code repositories |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding new methods and concepts.

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file.

## Citation

If you use this knowledge base in your research, please cite:

```
AIVC Perturbation Vault (2026). 
https://github.com/dxyang42/aivc-vault
```

---

**Obsidian Users**: Open `00-Home.md` for the full Obsidian experience with graph view and backlinks.

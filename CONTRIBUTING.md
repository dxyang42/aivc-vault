# Contributing to AIVC Perturbation Vault

Thank you for your interest in contributing to the AIVC Perturbation Vault! This document provides guidelines for contributing new methods, concepts, and resources.

## 📋 Table of Contents

- [How to Contribute](#how-to-contribute)
- [Documentation Format](#documentation-format)
- [Pull Request Process](#pull-request-process)
- [Style Guidelines](#style-guidelines)

---

## How to Contribute

### Adding a New Method

1. **Check for duplicates**: Search existing methods to ensure your contribution is not already documented
2. **Use the template**: Copy the method template from `05-Resources/templates/方法模板.md`
3. **Fill in all sections**: Ensure all required sections are completed
4. **Place in correct category**: Add the method to the appropriate subdirectory in `02-Methods/`

### Adding a New Concept

1. **Check existing concepts**: Review `04-Concepts/` for similar content
2. **Create clear definitions**: Provide accessible explanations for technical terms
3. **Link related concepts**: Use internal links to connect related topics

### Adding a New Team

1. **Verify team information**: Ensure accuracy of institutional affiliations
2. **List key publications**: Include 3-5 most relevant papers
3. **Link to methods**: Connect to methods developed by the team

---

## Documentation Format

### Method Documentation Template

Each method document should include:

```markdown
# Method Name

## Overview
- **Year**: YYYY
- **Authors**: Author names
- **Institution**: Institution name
- **Paper**: [Paper Title](link)
- **Code**: [GitHub](link) (if available)

## Problem Statement
Brief description of the problem the method addresses.

## Key Innovation
What makes this method unique compared to existing approaches.

## Method Details

### Architecture
Description of the model architecture.

### Key Components
- Component 1: Description
- Component 2: Description

## Advantages
- Advantage 1
- Advantage 2

## Limitations
- Limitation 1
- Limitation 2

## Applications
Real-world use cases and applications.

## Related Methods
- [[Related Method 1]]
- [[Related Method 2]]

## References
1. Citation 1
2. Citation 2
```

### File Naming Conventions

- **Methods**: Use method name with underscores for spaces (e.g., `Cell_Oracle.md`)
- **Concepts**: Use descriptive names in English (e.g., `Gene-Expression.md`)
- **Teams**: Use institution or group name (e.g., `Arc-Institute.md`)

---

## Pull Request Process

1. **Fork the repository**: Create your own fork of the project
2. **Create a branch**: `git checkout -b feature/your-feature-name`
3. **Make your changes**: Follow the documentation format guidelines
4. **Test locally**: Ensure all links work and formatting is correct
5. **Commit your changes**: Use clear, descriptive commit messages
   ```
   Add: New method X-Cell documentation
   Update: Fix typos in GEARS.md
   Add: New concept document for spatial transcriptomics
   ```
6. **Push to your fork**: `git push origin feature/your-feature-name`
7. **Create a Pull Request**: Submit PR with a clear description

### PR Checklist

Before submitting your PR, ensure:

- [ ] All required sections are filled in
- [ ] No LaTeX math formulas (use natural language descriptions)
- [ ] All internal links use proper wiki-link format `[[filename]]`
- [ ] External links are working
- [ ] No sensitive or copyrighted material included
- [ ] Spell check completed

---

## Style Guidelines

### Writing Style

- **Be concise**: Focus on key information
- **Be accurate**: Verify all technical details
- **Be accessible**: Explain technical terms when first introduced
- **Use active voice**: "The model predicts..." not "Predictions are made by..."

### Formatting Rules

- **Headers**: Use sentence case (e.g., "## Method overview" not "## Method Overview")
- **Lists**: Use bullet points for unordered lists, numbers for sequential steps
- **Code**: Use inline code formatting for method names and technical terms
- **Links**: Use descriptive link text (e.g., "[official implementation](link)" not "[here](link)")

### Mathematical Notation

- **Avoid LaTeX**: Do not use `$...$` or `$$...$$` for math
- **Use natural language**: Describe mathematical operations in words
- **Example**: Instead of `$L = \frac{1}{N}\sum_{i=1}^N (y_i - \hat{y}_i)^2$`, write:
  > "The loss function computes the mean squared error between predicted and actual values."

### ASCII Diagrams

When including diagrams:

- Use ASCII art or Mermaid diagrams
- Keep diagrams simple and focused
- Ensure diagrams render correctly in Markdown viewers

Example:
```
Input Data
    ↓
Preprocessing
    ↓
Model Inference
    ↓
Output Predictions
```

---

## Review Process

All contributions will be reviewed for:

1. **Accuracy**: Technical correctness of content
2. **Completeness**: All required sections present
3. **Formatting**: Adherence to style guidelines
4. **Originality**: No plagiarism or copyright violations

Reviewers may request changes before merging. Please be responsive to feedback.

---

## Questions?

If you have questions about contributing:

- Open an issue with the "question" label
- Check existing documentation for examples
- Refer to the [FAQ](FAQ.md) for common questions

---

Thank you for helping make AIVC Perturbation Vault a comprehensive resource for the research community!

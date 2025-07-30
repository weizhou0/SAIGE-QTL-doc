---
layout: default
title: Home
nav_order: 1
description: "Documentation for SAIGE-QTL: Scalable and accurate expression quantitative trait locus mapping for single-cell studies."
permalink: /
---

# SAIGE-QTL

SAIGE-QTL is an R package developed with Rcpp for **scalable and accurate expression quantitative trait locus (eQTL) mapping** in single-cell studies.

## Key Features

The SAIGE-QTL method provides several advantages for single-cell eQTL analysis:

- **Complex data modeling**: Handles repeated and complex data structures arising from multiple cells per individual and relatedness between individuals
- **Discrete count modeling**: Specifically designed for discrete read count data typical in single-cell RNA sequencing
- **Scalability**: Fast and scalable for large datasets - can analyze:
  - 20,000+ genes
  - Tens to hundreds of cell types
  - Millions of cells
  - Millions of genetic variants
- **Rare variant testing**: Includes set-based tests for rare variation effects where single-variant tests are underpowered

## Supported Input Formats

SAIGE-QTL accepts genotype files in multiple standard formats:
- PLINK (bed, bim, fam)
- BGEN
- VCF
- BCF
- SAV

## Quick Start

1. **[Install SAIGE-QTL](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation.html)** - Choose from multiple installation methods
2. **[Review the workflow overview](https://weizhou0.github.io/SAIGE-QTL-doc/docs/overview.html)** - Understand the analysis pipeline
3. **[Run Step 1](https://weizhou0.github.io/SAIGE-QTL-doc/docs/step1.html)** - Fit the null Poisson mixed model
4. **[Perform association tests](https://weizhou0.github.io/SAIGE-QTL-doc/docs/calling-saigeqtl.html)** - Execute cis-eQTL or genome-wide analyses

## Important Note on Normalization

⚠️ **Critical**: Accounting for total read counts per cell is essential in single-cell eQTL mapping. Consider using:
- SCTransform function from the Seurat R package, OR
- Including log(total read counts) and percentage of mitochondrial read counts as an offset in the Step 1 null model

## What's New

**Version 0.3.2** (July 28, 2025):
- Added `--offsetCol` option for using log of total read counts per cell as an offset
- Enhanced installation process using pixi
- See [Installation logs](Installation.html) for complete bug fixes and updates

## Citation

### SAIGE-QTL
Preprint: https://www.medrxiv.org/content/10.1101/2024.05.15.24307317v1

## License

SAIGE-QTL is distributed under an MIT license.

## Support

For questions about SAIGE-QTL, please contact: [wzhou@broadinstitute.org](mailto:wzhou@broadinstitute.org)

For technical issues and bug reports, please use the GitHub repository issue tracker.

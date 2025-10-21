---
layout: default
title: FAQ
nav_order: 9
description: "Frequently asked questions about SAIGE-QTL installation, usage, and troubleshooting."
---

# Frequently Asked Questions (FAQ)

## Installation & Setup

### Q: Which installation method should I choose?

**A:** We recommend **Pixi** for most users as it provides:
- Automatic dependency management
- Reproducible environments
- Easy setup and maintenance

Choose other methods based on your specific needs:
- **Docker**: For containerized environments or HPC clusters
- **Conda**: If you already have conda environments
- **Source**: For developers or custom builds

### Q: What are the minimum system requirements?

**A:** 
- **RAM**: 8GB minimum, 16GB+ recommended
- **Storage**: 2GB for software, additional space for data
- **OS**: Linux, macOS, or Windows (WSL recommended)
- **R**: Version 4.0+ (automatically installed with Pixi/Docker)

### Q: How do I verify my installation is working?

**A:** Run the help command for any script:
```bash
# Replace with your installation method's prefix
Rscript step1_fitNULLGLMM_qtl.R --help
```

## Data Preparation

### Q: What file formats does SAIGE-QTL support?

**A:** 
- **Genotype files**: PLINK (bed/bim/fam), BGEN, VCF, BCF, SAV
- **Phenotype files**: Tab or space-delimited text files with headers
- **Output formats**: Various statistical output formats and R data files

### Q: How should I handle normalization for single-cell data?

**A:** Critical normalization considerations:
- Use **SCTransform** from Seurat R package, OR
- Include **log(total read counts)** and **percentage of mitochondrial reads** as covariates
- Consider using the new `--offsetCol` parameter for log total read counts as an offset

### Q: What covariates should I include?

**A:**
- **Cell-level covariates** (`--covarColList`): Batch effects, technical factors, QC metrics
- **Individual-level covariates** (`--sampleCovarColList`): Age, sex, population structure
- **Recommended**: Principal components from genotype data for population structure

## Analysis Workflow

### Q: Should I choose cis-eQTL or genome-wide analysis?

**A:** 
- **cis-eQTL**: For targeted analysis, candidate genes, or higher statistical power
- **Genome-wide**: For comprehensive discovery, trans-eQTLs, or regulatory networks
- Both can share Step 1 results if analyzing the same genes

### Q: Can I run Step 1 in parallel?

**A:** Yes! Step 1 can be run independently for each gene, making it highly parallelizable. Each gene analysis can use one CPU core.

### Q: How long does analysis take?

**A:** Depends on dataset size:
- **Step 1**: Minutes to hours per gene
- **Step 2**: Hours to days for genome-wide analysis
- Use parallel processing to reduce total time

## Troubleshooting

### Q: My analysis failed with convergence errors. What should I do?

**A:** Try these solutions:
1. Reduce tolerance: `--tol=0.000001` (smaller value)
2. Check for highly correlated covariates
3. Ensure sufficient sample size
4. Review input data quality

### Q: I'm getting memory errors. How can I fix this?

**A:** Memory optimization strategies:
1. Increase available system memory
2. Use sparse matrix options if available
3. Process smaller batches of variants
4. Consider using HPC resources for large datasets

### Q: File path errors in Docker/Singularity - what's wrong?

**A:** Common path issues:
1. Ensure files are in bound/mounted directories
2. Use absolute paths when possible
3. Check file permissions
4. Verify volume mounting syntax

### Q: What if my results don't make biological sense?

**A:** Validation steps:
1. Check input data quality and formatting
2. Verify covariate specification
3. Review normalization procedures
4. Compare with known eQTLs in your tissue/population
5. Consider multiple testing corrections

## Best Practices

### Q: How should I handle multiple cell types?

**A:** Strategies for multi-cell-type data:
- **Separate analyses**: Run SAIGE-QTL separately for each cell type
- **Pseudobulk**: Aggregate rare cell types before analysis
- **Covariates**: Include cell composition as covariates for mixed populations

### Q: What quality control should I apply?

**A:** Recommended QC steps:
- **Variants**: MAF > 1%, HWE p-value > 1e-6, call rate > 95%
- **Samples**: Remove outliers in PCA
- **Expression**: Filter genes expressed in < 10% of donors, remove technical artifacts

### Q: How do I interpret p-values and effect sizes?

**A:** Interpretation guidelines:
- **Genome-wide significance**: Typically p < 5e-8 for genome-wide tests
- **cis-eQTL significance**: More lenient thresholds (e.g., p < 1e-5) acceptable
- **Effect sizes**: Log fold-change or percentage change in expression
- **Multiple testing**: Apply FDR or Bonferroni correction as appropriate

## Version-Specific Questions

### Q: What's new in version 0.3.2?

**A:** New features:
- `--offsetCol` parameter for using log total read counts as offset
- Enhanced Pixi installation support
- Improved error handling and debugging
- Better memory management for large datasets

### Q: Can I use results from older SAIGE-QTL versions?

**A:** Generally yes, but:
- Check for parameter name changes
- Review new options that might improve your analysis
- Consider re-running critical analyses with the latest version

## Getting Help

### Q: Where can I get additional support?

**A:** Support options:
- **Email**: [wzhou@broadinstitute.org](mailto:wzhou@broadinstitute.org) for technical questions
- **GitHub Issues**: For bug reports and feature requests
- **Documentation**: Browse all guides in this documentation site
- **Community**: Check if others have similar questions in GitHub discussions

### Q: How do I report a bug?

**A:** When reporting bugs, include:
1. SAIGE-QTL version
2. Installation method used
3. Complete error message
4. Command that caused the error
5. System information (OS, R version, etc.)
6. Example data if possible (anonymized)

### Q: Can I contribute to SAIGE-QTL development?

**A:** Yes! Contributions are welcome:
- Report bugs and suggest features via GitHub
- Contribute documentation improvements
- Share analysis examples and use cases
- Contact the development team for larger contributions
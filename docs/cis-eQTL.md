---
layout: default
title: cis-eQTL Analysis (Step 2+3)
nav_order: 6
description: "Complete guide for performing cis-eQTL analysis with SAIGE-QTL, including both common and rare variant testing approaches."
has_children: true
has_toc: true
---

# cis-eQTL Analysis with SAIGE-QTL

**cis-eQTL analysis** tests genetic variants located near genes (typically within 1 Mb) for their effects on gene expression. This approach is particularly powerful for identifying local genetic regulatory elements.

## Analysis Overview

SAIGE-QTL supports two complementary approaches for cis-eQTL mapping:

### 🔹 **Common Variant Analysis**
- **Method**: Single-variant tests  
- **Steps**: 3-step process (Step 1 → Step 2 → Step 3)
- **Best for**: Variants with MAF > 1-5%
- **Output**: Variant-level association statistics

### 🔹 **Rare Variant Analysis**  
- **Method**: Set-based tests (gene burden/variance component tests)
- **Steps**: 2-step process (Step 1 → Step 2)
- **Best for**: Variants with MAF < 1-5%  
- **Output**: Gene-level association statistics

> **Note**: Step 1 (null model fitting) is shared between both approaches

## Workflow Diagram

<img src="{{site.baseurl | prepend: site.url}}/assets/img/SAIGE-pie.png" width="400" alt="SAIGE-QTL cis-eQTL analysis workflow showing three-step process for common variants and two-step process for rare variants">

## When to Use cis-eQTL Analysis

**Choose cis-eQTL analysis when:**
- You want to identify **local regulatory variants** near specific genes
- You have **candidate genes** of interest  
- You need **higher statistical power** (smaller multiple testing burden than genome-wide)
- You're studying **tissue-specific** or **cell-type-specific** regulation
- You want to **validate known eQTLs** in your population/tissue

**Consider genome-wide analysis when:**
- You want to discover **trans-eQTLs** (distant regulatory effects)
- You're performing **unbiased discovery** across the genome
- You have **computational resources** for larger-scale analysis

## Getting Started

### Prerequisites
1. **[Install SAIGE-QTL](Installation.html)** using your preferred method
2. **[Understand the workflow](overview.html)** and analysis pipeline  
3. **Prepare your data**: phenotype files, genotype files, and covariates

### Quick Start Guide
1. **[Step 1: Fit null models](step1.html)** - One model per gene of interest
2. **[Step 2: Run association tests](single_step2.html)** - Test cis-variants for each gene
3. **[Step 3: Gene-level analysis](gene_step3.html)** (optional) - For rare variant burden tests

## Analysis Considerations

### Variant Selection
- **cis-window**: Typically 1 Mb upstream and downstream of gene boundaries
- **MAF filtering**: Consider separate analyses for common (MAF ≥ 5%) and rare (MAF < 5%) variants
- **Quality control**: Apply standard variant QC before analysis

### Statistical Power
- **Multiple testing**: Fewer tests compared to genome-wide analysis
- **Effect sizes**: cis-eQTLs typically have larger effect sizes
- **Sample size**: Power increases with larger sample sizes and more cells per individual

### Cell-Type Specificity
- Analyze **cell-type-specific** expression when possible
- Consider **pseudobulk** approaches for rare cell types
- Account for **cell composition** effects in mixed populations

## Next Steps

Ready to start your cis-eQTL analysis? Follow these detailed guides:

1. **[Step 1: Null Model Fitting](step1.html)** - Detailed parameter explanations and examples
2. **[Step 2: Single-Variant Tests](single_step2.html)** - cis-eQTL association testing
3. **[Step 2: Set-Based Tests](set_step2.html)** - Rare variant burden analysis  
4. **[Step 3: Gene-Level Analysis](gene_step3.html)** - Combine results across variants

### Alternative Analysis
- **[Genome-wide eQTL Analysis](genomewide-eQTL.html)** - For comprehensive variant discovery

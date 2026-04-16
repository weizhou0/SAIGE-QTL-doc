---
layout: default
title: "Step 2b: Optional QC"
nav_order: 1
description: "Step 2b: Optional Quality Control for cis-eQTL results."
parent: cis-eQTL test
---

# Step 2b: Optional QC for cis-eQTL Results

## Overview

Step 2b is optional, it involves user-specified quality control based on dispersion parameter from step 1 results and genomic inflation factor from step2 results.

## Getting Started

### Example Scripts Location

The wrapper scripts and example data are located in the `./extdata` folder: [https://github.com/weizhou0/SAIGEQTL/tree/main/extdata](https://github.com/weizhou0/SAIGEQTL/tree/main/extdata)

Check Installation instruction if you haven't had SAIGE-QTL installed yet: [Install SAIGE-QTL](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation.html)

## Running step 2b

#### Pixi Installation
```bash
# Navigate to SAIGEQTL directory first
cd SAIGEQTL/extdata

pixi run --manifest-path=../pixi.toml Rscript step2b_QC_qtl.R  \
  --SAIGEstep1OutputPrefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1 \
  --SAIGEstep2OutputPrefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis \
  --gene=example_gene \
  --outPrefix=example_gene_output \
  --isPostStep2QC=FALSE \
  --phiUpper 1.5 \
  --lambdaLower 0.1 \
  --lambdaUpper 1.5
```

## Input Files

### 1. step 1 and step 2 results (Required)

Since the optional quality control is based on parameters calculated from step 1 and 2, the output from both steps would be required to calculate corresponding parameters.

## Key Parameters Explained

| Parameter | Description | Example Value |
|-----------|-------------|---------------|
| `--SAIGEstep1OutputPrefix` | Path to step1 output filename (no suffix) | `./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1` |
| `--SAIGEstep2OutputPrefix` | Path to step2 output filename (no suffix) | `./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis` |
| `--gene` | Gene name (for reporting only) | `example_gene` |
| `--outPrefix` | Output prefix for QC summary | `example_gene_output` |
| `--sampleIDColinphenoFile` | Sample ID column name | `IND_ID` |
| `--isPostStep2QC` | Whether to remove step1 and step2 outputs if they fail QC standards (logical) | `FALSE` |
| `--outputPrefix` | Prefix for output files | `./output/analysis_gene1` |
| `--phiUpper` | Upper bound for phi dispersion parameter | User-specified |
| `--lambdaLower` | Lower bound for GC lambda | User-specified |
| `--lambdaUpper` | Upper bound for GC lambda | User-specified |

## Output Files

### Evaluation metric summary file

Contains dispersion and GC lambda estimation for this specific gene for record.







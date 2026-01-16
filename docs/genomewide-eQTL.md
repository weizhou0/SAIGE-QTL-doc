---
layout: default
title: Genome-wide eQTL Analysis (Step 2+3)
nav_order: 7
description: "Comprehensive guide for genome-wide eQTL discovery using SAIGE-QTL, including batch processing strategies and computational optimization."
has_children: true
has_toc: true
---

# Genome-wide eQTL Analysis with SAIGE-QTL

**Genome-wide eQTL analysis** tests all genetic variants across the genome for their effects on gene expression, enabling discovery of both local (cis) and distant (trans) regulatory relationships.

## Key Advantages

### 🚀 **Computational Efficiency**
SAIGE-QTL's genome-wide approach offers significant performance benefits:
- **Batch processing**: Analyze multiple genes simultaneously
- **Reduced I/O overhead**: Minimizes genotype file reading time
- **Parallel computation**: Step 1 can run independently for each gene
- **Scalable**: Handles large datasets efficiently

### 🎯 **Comprehensive Discovery**
- **cis-eQTLs**: Local regulatory variants (same as cis-eQTL analysis)
- **trans-eQTLs**: Distant regulatory effects across chromosomes
- **Pleiotropic effects**: Single variants affecting multiple genes
- **Regulatory networks**: System-level regulatory relationships

## Analysis Workflow

The genome-wide approach follows a streamlined 2-step process:

1. **Step 1**: Fit null Poisson mixed models (one per gene)
2. **Step 2**: Perform genome-wide association tests (batch processing)

> **Note**: Step 1 is identical to cis-eQTL analysis and results can be shared between analyses

## When to Use Genome-wide Analysis

### **Choose genome-wide analysis for:**
- **Unbiased discovery** of regulatory variants
- **trans-eQTL detection** across chromosomes  
- **Regulatory network** construction
- **Pleiotropic effect** identification
- **Large-scale eQTL** mapping projects

### **Consider cis-eQTL analysis for:**
- **Candidate gene** studies
- **Targeted analysis** with limited computational resources
- **Higher statistical power** for local effects
- **Validation studies** of known regulatory regions

## Computational Strategy

### Step 1: Parallel Null Model Fitting

Step 1 can be run independently for each gene, making it highly parallelizable:

**Example batch processing for 100 genes:**

```bash
cd SAIGEQTL/extdata/
for i in {1..100}
do
echo $i
step1prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_${i}
/bin/time -o ${step1prefix}.runinfo.txt -v pixi run --manifest-path=../pixi.toml Rscript step1_fitNULLGLMM_qtl.R \
    --useSparseGRMtoFitNULL=FALSE  \
    --useGRMtoFitNULL=FALSE \
    --phenoFile=./input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt \
    --phenoCol=gene_${i}       \
    --covarColList=X1,X2,pf1,pf2    \
    --sampleCovarColList=X1,X2      \
    --sampleIDColinphenoFile=IND_ID \
    --traitType=count \
    --outputPrefix=${step1prefix}   \
    --skipVarianceRatioEstimation=FALSE  \
    --isRemoveZerosinPheno=FALSE \
    --isCovariateOffset=FALSE  \
    --isCovariateTransform=TRUE  \
    --skipModelFitting=FALSE  \
    --tol=0.00001   \
    --plinkFile=./input/n.indep_100_n.cell_1_01.step1       \
    --IsOverwriteVarianceRatioFile=TRUE &> ${step1prefix}.log
done
```

### Performance Optimization

- **CPU allocation**: One CPU per gene for Step 1 parallelization
- **Memory management**: Batch processing reduces memory overhead
- **I/O efficiency**: Minimizes repeated genotype file access
- **Job scheduling**: Ideal for HPC cluster environments

## Pre-computed Example Data

To save computation time, you can download pre-computed Step 1 results:

📁 **[Download Step 1 outputs](https://drive.google.com/file/d/1zV4l6f6YUCSJcTwXZUrL2PZ6N0_ly_XU/view?usp=sharing)**

Store downloaded files to `./output` directory to skip Step 1 computation.

## Analysis Considerations

### Multiple Testing
- **Genome-wide significance**: Apply appropriate corrections (e.g., Bonferroni, FDR)
- **Trans-eQTL thresholds**: Consider more stringent thresholds for distant effects
- **Population-specific**: Adjust thresholds based on linkage disequilibrium patterns

### Statistical Power
- **Sample size**: Larger samples needed for trans-eQTL detection
- **Effect sizes**: Trans-eQTLs typically have smaller effects than cis-eQTLs  
- **Cell composition**: Account for cell-type-specific effects

### Computational Resources
- **Memory requirements**: Scale with dataset size
- **Storage needs**: Large output files for genome-wide results
- **Processing time**: Hours to days depending on data size

## Next Steps

Ready to perform genome-wide eQTL analysis? Follow these guides:

1. **[Step 1: Batch Null Model Fitting](https://weizhou0.github.io/SAIGE-QTL-doc/docs/step1.html)** - Parallel processing strategies
2. **[Step 2: Genome-wide Association Tests](https://weizhou0.github.io/SAIGE-QTL-doc/docs/genomewide_single_step2.html)** - Single-variant tests
3. **[Step 2: Set-based Tests](https://weizhou0.github.io/SAIGE-QTL-doc/docs/genomewide_set_step2.html)** - Rare variant analysis

### Alternative Analysis
- **[cis-eQTL Analysis](https://weizhou0.github.io/SAIGE-QTL-doc/docs/cis-eQTL.html)** - For focused, local regulatory analysis

## Best Practices

### Resource Planning
- Estimate computational requirements based on your dataset
- Plan for adequate storage for results
- Consider cloud computing for large-scale analyses

### Quality Control
- Apply stringent variant and sample QC
- Monitor convergence across all genes
- Validate top associations with independent data

### Result Interpretation
- Distinguish cis vs trans effects in results
- Consider biological plausibility of trans-eQTLs
- Integrate with functional annotation and regulatory databases

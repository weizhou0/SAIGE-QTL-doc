---
layout: default
title: Step 1
nav_exclude: true
description: "Step 1: fitting the null Poisson mixed model."
---

# Step 1: Fitting the Null Poisson Mixed Model

## Overview

Step 1 involves fitting a null Poisson mixed model for each gene across cells. 

## Getting Started

### Example Scripts Location

The wrapper scripts and example data are located in the `./extdata` folder: [https://github.com/weizhou0/SAIGEQTL/tree/main/extdata](https://github.com/weizhou0/SAIGEQTL/tree/main/extdata)

Check Installation instruction if you haven't had SAIGE-QTL installed yet: [Install SAIGE-QTL](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation.html)


## Fitting the Null Model

### Model Configuration

The example uses the following settings and assumptions:

- **Poisson mixed model**: `--traitType=count`
- **Covariates specification**: `--covarColList=` for all covariates included in the model
- **Sample covariates**: `--sampleCovarColList=` for only individual-level covariates
- **Offset terms**: `--offsetCol=` for normalization (e.g., log total read counts)

### Running Step 1

#### Pixi Installation
```bash
# Navigate to SAIGEQTL directory first
cd SAIGEQTL/extdata

pixi run --manifest-path=../pixi.toml Rscript step1_fitNULLGLMM_qtl.R \
    --useSparseGRMtoFitNULL=FALSE \
    --useGRMtoFitNULL=FALSE \
    --phenoFile=./input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt \
    --phenoCol=gene_1 \
    --covarColList=X1,X2,pf1,pf2 \
    --sampleCovarColList=X1,X2 \
    --sampleIDColinphenoFile=IND_ID \
    --traitType=count \
    --outputPrefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1 \
    --skipVarianceRatioEstimation=FALSE \
    --isRemoveZerosinPheno=FALSE \
    --isCovariateOffset=FALSE \
    --isCovariateTransform=TRUE \
    --skipModelFitting=FALSE \
    --tol=0.00001 \
    --plinkFile=./input/n.indep_100_n.cell_1_01.step1 \
    --IsOverwriteVarianceRatioFile=TRUE
```

#### Docker Installation
```bash
# Set your working directory first and the output from example runs will be stored there
# e.g. /data/wzhougroup/

WKDIR=/data/wzhougroup/

docker run -w ${WKDIR} wzhou88/saigeqtl:latest \
    step1_fitNULLGLMM_qtl.R \
    --useSparseGRMtoFitNULL=FALSE \
    --useGRMtoFitNULL=FALSE \
    --phenoFile=/usr/local/bin/input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt \
    --phenoCol=gene_1 \
    --covarColList=X1,X2,pf1,pf2 \
    --sampleCovarColList=X1,X2 \
    --sampleIDColinphenoFile=IND_ID \
    --traitType=count \
    --outputPrefix=${WKDIR}nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1 \
    --skipVarianceRatioEstimation=FALSE \
    --isRemoveZerosinPheno=FALSE \
    --isCovariateOffset=FALSE \
    --isCovariateTransform=TRUE \
    --skipModelFitting=FALSE \
    --tol=0.00001 \
    --plinkFile=/usr/local/bin/input/n.indep_100_n.cell_1_01.step1 \
    --IsOverwriteVarianceRatioFile=TRUE \
    --solverMethod=pcg
```

#### Singularity Installation
```bash
# Set your working directory first and the output from example runs will be stored there
# e.g. /data/wzhougroup/
# Set the path to the singularity file saigeqtl_latest.sif, e.g. /data/wzhougroup

WKDIR=/data/wzhougroup/
PATHTOSIF=/data/wzhougroup/

singularity exec \
    --bind  ${WKDIR}:${WKDIR} \
    --cleanenv ${PATHTOSIF}saigeqtl_latest.sif \
    step1_fitNULLGLMM_qtl.R	\
    --useSparseGRMtoFitNULL=FALSE \
    --useGRMtoFitNULL=FALSE \
    --phenoFile=/usr/local/bin/input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt \
    --phenoCol=gene_1 \
    --covarColList=X1,X2,pf1,pf2 \
    --sampleCovarColList=X1,X2 \
    --sampleIDColinphenoFile=IND_ID \
    --traitType=count \
    --outputPrefix=${WKDIR}nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1 \
    --skipVarianceRatioEstimation=FALSE \
    --isRemoveZerosinPheno=FALSE \
    --isCovariateOffset=FALSE \
    --isCovariateTransform=TRUE \
    --skipModelFitting=FALSE \
    --tol=0.00001 \
    --plinkFile=/usr/local/bin/input/n.indep_100_n.cell_1_01.step1 \
    --IsOverwriteVarianceRatioFile=TRUE
```

## Input Files

### 1. Phenotype File (Required)

The phenotype file contains expression data and covariates. It can be space or tab-delimited with a header and must include:
- One column for sample IDs
- One column for the phenotype (gene expression)
- Optional columns for covariates

**View the example phenotype file:**

#### Standard/Pixi Installation
```bash
# Make sure you're in SAIGEQTL/extdata/ folder
less -S ./input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt
```

#### Docker Installation
```bash
WKDIR=/data/wzhougroup/

docker run -w ${WKDIR} wzhou88/saigeqtl:latest \
    less -S /usr/local/bin/input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt
```

#### Singularity Installation
```bash
WKDIR=/data/wzhougroup/
PATHTOSIF=/data/wzhougroup/

singularity exec \
    --bind  ${WKDIR}:${WKDIR} \
    --cleanenv ${PATHTOSIF}saigeqtl_latest.sif \
    less -S /usr/local/bin/input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt
```

### 2. Plink File (Required)

Used for estimating variance ratio with random markers:
- Use `--skipVarianceRatioEstimation=FALSE`
- Specify plink file with `--plinkFile` (e.g., `--plinkFile=./input/n.indep_100_n.cell_1_01.step1`)
- Include ~1000 random markers with MAC ≥ 20

## Key Parameters Explained

| Parameter | Description | Example Value |
|-----------|-------------|---------------|
| `--phenoFile` | Path to phenotype file | `./input/phenotype.txt` |
| `--phenoCol` | Column name for phenotype | `gene_1` |
| `--covarColList` | all covariates | `X1(e.g. age),X2(e.g. sex),pf1,pf2` |
| `--sampleCovarColList` | Individual-level covariates | `X1(age),X2(sex)` |
| `--sampleIDColinphenoFile` | Sample ID column name | `IND_ID` |
| `--traitType` | Type of trait (count/binary/quantitative) | `count` |
| `--outputPrefix` | Prefix for output files | `./output/analysis_gene1` |
| `--plinkFile` | Plink file for variance estimation | `./input/markers.step1` |
| `--offsetCol` | Offset column (optional) | `log_total_reads` |

## Output Files

### 1. Model File (Required for Step 2)

Contains the fitted null model parameters:

#### Load and Inspect Model File

```R
# In R session
load("./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.rda")
names(modglmm)
modglmm$theta
```


### 2. Variance Ratio File (Required for Step 2 if estimated)

Contains variance component estimates:

#### View Variance Ratio File

##### Standard/Pixi Installation
```bash
cd /SAIGEQTL/extdata
less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.varianceRatio.txt
```

## Best Practices

### Data Preparation
- Ensure phenotype file has proper headers and formatting
- Include relevant covariates (age, sex, technical factors)
- Use appropriate offset terms for count data normalization
- Verify sample IDs match between phenotype and genotype files

### Parameter Selection
- Set `--traitType=count` for expression count data
- Use `--isCovariateTransform=TRUE` for covariate standardization
- Include batch effects and technical covariates in `--covarColList`
- Specify individual-level covariates in `--sampleCovarColList`

### Quality Control
- Check convergence by examining the tolerance parameter (`--tol`)
- Verify variance ratio estimates are reasonable
- Monitor model fitting logs for warnings or errors

### Memory and Performance
- For large datasets, consider using sparse GRM options
- Adjust tolerance settings based on convergence requirements
- Use appropriate compute resources for the dataset size

## Troubleshooting

### Common Issues

1. **Convergence Problems**
   - Reduce tolerance (`--tol`) to 1e-6 or smaller
   - Check for highly correlated covariates
   - Ensure sufficient sample size

2. **File Path Errors**
   - Use absolute paths when possible
   - Verify file permissions and accessibility
   - Check volume mounting in Docker/Singularity

3. **Memory Issues**
   - Increase available memory for large datasets
   - Consider using sparse matrix options for relatedness

4. **Covariate Issues**
   - Ensure covariates are properly formatted
   - Check for missing values in covariate columns
   - Verify sample-level vs cell-level covariate specification

### Next Steps

After successful completion of Step 1, proceed to Step 2 using:
- The model file (`.rda`)
- The variance ratio file (`.varianceRatio.txt`)

These files serve as inputs for the association testing phase in Step 2.

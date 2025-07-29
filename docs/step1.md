---
layout: default
title: Step 1
nav_order: 5
description: "Step 1: fitting the null Poisson mixed model."
---

# Step 1: Fitting the Null Poisson Mixed Model

## Overview

Step 1 involves fitting a null Poisson mixed model for each gene across cells. 

## Getting Started

### Example Scripts Location

The wrapper scripts and example data are located in the `./extdata` folder: [https://github.com/weizhou0/SAIGEQTL/tree/main/extdata](https://github.com/weizhou0/SAIGEQTL/tree/main/extdata)

```bash
# If not downloaded the SAIGEQTL package already, here we download the repo with /extdata
src_branch=main
repo_src_url=https://github.com/weizhou0/SAIGEQTL
git clone -b $src_branch $repo_src_url
```

### Check Help Information

View available parameters and their descriptions:

#### Standard Installation
```bash
# Navigate to the extdata folder in the SAIGEQTL directory first
cd SAIGEQTL/extdata
Rscript step1_fitNULLGLMM_qtl.R --help
```

#### Docker Installation
```bash
docker run wzhou88/saigeqtl:0.3.2 step1_fitNULLGLMM_qtl.R --help
```

#### Singularity Installation
```bash
cd SAIGEQTL
singularity exec --bind /data:/extdata /path/to/saigeqtl_0.3.2.sif \
    Rscript /usr/local/bin/step1_fitNULLGLMM_qtl.R --help
```

#### Pixi Installation
```bash
# Navigate to the extdata folder in the SAIGEQTL directory first
cd SAIGEQTL/extdata
pixi run --manifest-path=../pixi.toml Rscript step1_fitNULLGLMM_qtl.R --help
```

## Fitting the Null Model

### Model Configuration

The example uses the following settings and assumptions:

- **Poisson mixed model**: `--traitType=count`
- **Covariates specification**: `--covarColList=` for cell-level covariates
- **Sample covariates**: `--sampleCovarColList=` for individual-level covariates
- **Offset terms**: `--offsetCol=` for normalization (e.g., log total read counts)

### Running Step 1

#### Standard Installation
```bash
cd SAIGEQTL/extdata
Rscript step1_fitNULLGLMM_qtl.R \
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
docker run -v /host/data:/SAIGEQTL wzhou88/saigeqtl:0.3.2 \
    step1_fitNULLGLMM_qtl.R \
    --useSparseGRMtoFitNULL=FALSE \
    --useGRMtoFitNULL=FALSE \
    --phenoFile=/extdata/input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt \
    --phenoCol=gene_1 \
    --covarColList=X1,X2,pf1,pf2 \
    --sampleCovarColList=X1,X2 \
    --sampleIDColinphenoFile=IND_ID \
    --traitType=count \
    --outputPrefix=/extdata/output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1 \
    --skipVarianceRatioEstimation=FALSE \
    --isRemoveZerosinPheno=FALSE \
    --isCovariateOffset=FALSE \
    --isCovariateTransform=TRUE \
    --skipModelFitting=FALSE \
    --tol=0.00001 \
    --plinkFile=/extdata/input/n.indep_100_n.cell_1_01.step1 \
    --IsOverwriteVarianceRatioFile=TRUE
```

#### Singularity Installation
```bash
singularity exec --bind /data:/SAIGEQTL /path/to/saigeqtl_0.3.2.sif \
    Rscript /usr/local/bin/step1_fitNULLGLMM_qtl.R \
    --useSparseGRMtoFitNULL=FALSE \
    --useGRMtoFitNULL=FALSE \
    --phenoFile=/extdata/input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt \
    --phenoCol=gene_1 \
    --covarColList=X1,X2,pf1,pf2 \
    --sampleCovarColList=X1,X2 \
    --sampleIDColinphenoFile=IND_ID \
    --traitType=count \
    --outputPrefix=/extdata/output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1 \
    --skipVarianceRatioEstimation=FALSE \
    --isRemoveZerosinPheno=FALSE \
    --isCovariateOffset=FALSE \
    --isCovariateTransform=TRUE \
    --skipModelFitting=FALSE \
    --tol=0.00001 \
    --plinkFile=/extdata/input/n.indep_100_n.cell_1_01.step1 \
    --IsOverwriteVarianceRatioFile=TRUE
```

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

## Input Files

### 1. Phenotype File (Required)

The phenotype file contains expression data and covariates. It can be space or tab-delimited with a header and must include:
- One column for sample IDs
- One column for the phenotype (gene expression)
- Optional columns for covariates

**View the example phenotype file:**

#### Standard/Pixi Installation
```bash
less -S ./input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt
```

#### Docker Installation
```bash
docker run -v /host/data:/container/data wzhou88/saigeqtl:0.3.2 \
    less -S /container/data/input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt
```

#### Singularity Installation
```bash
singularity exec --bind /data:/data /path/to/saigeqtl_0.3.2.sif \
    less -S /data/input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt
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
| `--covarColList` | Cell-level covariates | `X1,X2,pf1,pf2` |
| `--sampleCovarColList` | Individual-level covariates | `X1,X2` |
| `--sampleIDColinphenoFile` | Sample ID column name | `IND_ID` |
| `--traitType` | Type of trait (count/binary/quantitative) | `count` |
| `--outputPrefix` | Prefix for output files | `./output/analysis_gene1` |
| `--plinkFile` | Plink file for variance estimation | `./input/markers.step1` |
| `--offsetCol` | Offset column (optional) | `log_total_reads` |

## Output Files

### 1. Model File (Required for Step 2)

Contains the fitted null model parameters:

#### Load and Inspect Model File

##### Standard/Pixi Installation
```bash
# In R session
load("./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.rda")
names(modglmm)
modglmm$theta
```

##### Docker Installation
```bash
docker run -v /host/data:/SAIGEQTL/extdata wzhou88/saigeqtl:0.3.2 \
    Rscript -e "load('/SAIGEQTL/extdata/output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.rda'); names(modglmm); modglmm\$theta"
```

##### Singularity Installation
```bash
singularity exec --bind /data:/SAIGEQTL/extdata /path/to/saigeqtl_0.3.2.sif \
    Rscript -e "load('/SAIGEQTL/extdata/output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.rda'); names(modglmm); modglmm\$theta"
```

##### Pixi Installation
```bash
cd SAIGEQTL/extdata
pixi run Rscript -e "load('./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.rda'); names(modglmm); modglmm\$theta"
```

### 2. Variance Ratio File (Required for Step 2 if estimated)

Contains variance component estimates:

#### View Variance Ratio File

##### Standard/Pixi Installation
```bash
cd /SAIGEQTL/extdata
less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.varianceRatio.txt
```

##### Docker Installation
```bash
docker run -v /host/data:/SAIGEQTL/extdata wzhou88/saigeqtl:0.3.2 \
    less -S /SAIGEQTL/extdata/output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.varianceRatio.txt
```

##### Singularity Installation
```bash
singularity exec --bind /data:/SAIGEQTL/extdata /path/to/saigeqtl_0.3.2.sif \
    less -S /SAIGEQTL/extdata/output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.varianceRatio.txt
```

##### Pixi Installation
```bash
cd SAIGEQTL/extdata/
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

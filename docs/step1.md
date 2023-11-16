---
layout: default
title: Step 1
nav_order: 4
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
---


## Step 1: fitting the null Poisson mixed model


* The example scripts are located in the ./extdata folder [https://github.com/weizhou0/qtl/tree/main/extdata](https://github.com/weizhou0/qtl/tree/main/extdata)

```bash
#go to the folder
cd extdata
```

```bash
#check the help info for step 1
Rscript step1_fitNULLGLMM_qtl.R --help
```

### Fit the null model 

* the data set does not have sample relatedness --useSparseGRMtoFitNULL=FALSE and --useGRMtoFitNULL=FALSE 
* fit a Poisson mixed model --traitType=count
* specify covariates in the model using --covarColList=
* specify covariates for samples, which have the same values for all cells from the same invidiual, using --sampleCovarColList=. Note that covariates in --sampleCovarColList= need to be specified in --covarColList= 

```bash
Rscript step1_fitNULLGLMM_qtl.R \
        --useSparseGRMtoFitNULL=FALSE  \
        --useGRMtoFitNULL=FALSE \
        --phenoFile=./input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt	\
        --phenoCol=gene_1       \
        --covarColList=X1,X2,pf1,pf2    \
        --sampleCovarColList=X1,X2      \
        --sampleIDColinphenoFile=IND_ID \
        --traitType=count \
        --outputPrefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1 \
        --skipVarianceRatioEstimation=FALSE  \
        --isRemoveZerosinPheno=FALSE \
        --isCovariateOffset=FALSE  \
        --isCovariateTransform=TRUE  \
        --skipModelFitting=FALSE  \
        --tol=0.00001   \
        --plinkFile=./input/n.indep_100_n.cell_1_01.step1       \
        --IsOverwriteVarianceRatioFile=TRUE
```

### Input files

1. **(Required)** Phenotype file (contains covariates if any, such as gender and age)
The file can be either space or tab-delimited with a header. It is required that the file contains one column for sample IDs and one column for the phenotype. It may contain columns for covariates. <br/>

        <!---
        <img src="{{site.baseurl | prepend: site.url}}/assets/img/pheno_head.png" width="300">
        --->


    ```
    less -S ./input/seed_1_100_nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_Poisson.txt
    ```

2. **(Required)** Plink file 
Estimating variance ratio with random markers. 
* Use --skipVarianceRatioEstimation=FALSE
* Specify the plink file containing markers that will be used for variance ratio estimation with --plinkFile. e.g. **--plinkFile=./input/n.indep_100_n.cell_1_01.step1**
* Include 1000 random markers with MAC >= 20 in the plink file




### Output files

1. model file (**input for Step 2**)


    ```
    #load the model file in R
    load("./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.rda")
    names(modglmm)
    modglmm$theta

    ```
<!---
<img src="{{site.baseurl | prepend: site.url}}/assets/img/SAIGE-step1-output.png" width="500">
--->


2. variance ratio file (**if variance ratio is estiamted in Step 1, this will be input for Step 2**)

    ```
    less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.varianceRatio.txt
    ```

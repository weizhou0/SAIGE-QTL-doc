---
layout: default
title: genomewide-eQTL test
nav_order: 7
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
has_children: true
has_toc: true
---


When we run genome-wide association tests for identifying eQTLs, we can run analyses for multiple genes at the same time. We found it substantially reduced the total computation time due to reducing the overhead of reading genotypes
 
- step 1 needs to be run once for each trait/gene expression and can is shared by cis-eQTL and genomewide-eQTL test
- Same as Step 1 - cis-eQTL for each gene

* step 1 can be run independently for each gene, so each gene job can use one CPU.
* You may SKIP the following step 1 jobs and download the generated step 1 output from [here](https://drive.google.com/file/d/1zV4l6f6YUCSJcTwXZUrL2PZ6N0_ly_XU/view?usp=sharing) and store tham to ./output

    ```
    cd SAIGEQTL/extdata/
    for i in {1..100}
    do
    echo $i
    step1prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_${i}
    /bin/time -o ${step1prefix}.runinfo.txt -v Rscript step1_fitNULLGLMM_qtl.R \
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

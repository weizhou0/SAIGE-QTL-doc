---
layout: default
title: Step 3 - gene p-values (common variants)
nav_order: 2
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
parent: cis-eQTL test
---

## Step 3: obtaining gene-level p-values using the ACAT test

### check Help Information and the job scripts should use the corresponding command to call the wrapper function **step3_gene_pvalue_qtl.R**
**Check Step 1 and Step 2 for example header codes


```
#check the help info for step 3

### Check Help Information and the job scripts should use the corresponding command to call the wrapper function **step2_tests_qtl.R**

Note: Pixi script is showing here for examples

#### Pixi Installation
```bash
cd SAIGEQTL/extdata/
pixi run --manifest-path=../pixi.toml Rscript step3_gene_pvalue_qtl.R --help
```

* --assocFile= is used to specify the step 2 output with single-variant association test results from step 2.


```
pixi run --manifest-path=../pixi.toml Rscript step3_gene_pvalue_qtl.R	\
        --assocFile=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis        \
        --geneName=gene_1       \
        --genePval_outputFile=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis_genePval
```

## Input files

1. (Required) assoc file from step 2

    ```
    less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis
    ```

3. (Optional) a file with weights for each single variant for the calculation of ACAT p-values.

The file has two columns: one with marker IDs that are used to match with MarkerID in the assocFile (output by step 2) and the other column contains weights used for each marker. 
The file needs to have a header with column names 'MarkerID' and 'weight'. 
If not specified, equal weights will be used for all markers.

## Output file

1. A file with gene-level p-value for the gene, top nominal p-value, and variant ID with the top nominal p-value

    ```
    less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis_genePval

    ```

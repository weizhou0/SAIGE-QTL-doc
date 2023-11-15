---
layout: default
title: Step 2 - genome-wide (common variants)
nav_order: 1
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
parent: genomewide-eQTL test
---

## Step 2: performing genome-wide single-variant association tests. Multiple genes are tested simutaneously for computation speed-up

* e.g. test eQTLs on chromosome 2  for all 100 genes#use --GMMATmodel_varianceRatio_multiTraits_File
* use --GMMATmodel_varianceRatio_multiTraits_File
    * first create the file containing 3 columns: phenotype name, model file, and variance ratio file. Each line is for one phenotype. This file is used when multiple phenotypes are analyzed simutaneously

    ```
    ##commands to create a file that contains path to step 1 output for all 100 genes
    touch ./input/step1_output_formultigenes.txt
    rm ./input/step1_output_formultigenes.txt
    for i in {1..100}
    do
       step1prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_${i}
       echo -e "gene_${i} ${step1prefix}.rda ${step1prefix}.varianceRatio.txt" >> ./input/step1_output_formultigenes.txt
    done
    ```


    ```
    #run step 2 for all 100 genes 
    step2prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_chr2
    Rscript step2_tests_qtl.R       \
        --bedFile=./input/n.indep_100_n.cell_1.bed      \
        --bimFile=./input/n.indep_100_n.cell_1.bim      \
        --famFile=./input/n.indep_100_n.cell_1.fam      \
        --SAIGEOutputFile=${step2prefix}     \
        --GMMATmodel_varianceRatio_multiTraits_File=./input/step1_output_formultigenes.txt      \
        --chrom=2       \
        --minMAF=0.05 \
        --LOCO=FALSE    \
        --SPAcutoff=2 \
        --markers_per_chunk=10000
    ```

## Output files

The program will generate one output file for each of the tested gene. 

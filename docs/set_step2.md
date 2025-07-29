---
layout: default
title: Step 2 - set-based tests (rare variants)
nav_order: 3
description: "Step 2 - set-based tests (rare variants)"
parent: cis-eQTL test
---

## Step 2: performing set-based association tests for rare variants


```
##same setting as for the common variants tests for cis-eQTLs
step1prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1
step2prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis
regionFile=./input/gene_1_cis_region.txt
echo -e "2\t300001\t610001" > ${regionFile}
```


* The commands are the same as the step 2 for single-variant assoc tests, except for:
    * A group file contains the genetic marker IDs, annotations and weights (if any) for each set/gene to be tested

```
#check the help info for making the group file using region file
Rscript makeGroupFile.R --help
```


```
#make the group file
groupFile=${regionFile}.grp
Rscript makeGroupFile.R \
        --bedFile=./input/n.indep_100_n.cell_1.bed      \
        --bimFile=./input/n.indep_100_n.cell_1.bim      \
        --famFile=./input/n.indep_100_n.cell_1.fam      \
        --regionFile=${regionFile}      \
        --outputPrefix=${regionFile}.grp
```

* In the step 2 job for rare variants
    * --groupFile is used to specify the group file

* Allows for multiple masks for each set (gene or region)
    * Use --annotation_in_groupTest to list different annotations, seperated by comma. Within each annotation combination, annotations are separated by ':'
        * e.g. "lof,missense:lof,missense:lof:synonymous" is to test lof only, missense+lof, and missense+lof+synonymous
    * Use --maxMAF_in_groupTest and --minMAF_in_groupTest_Exclude to specify the maximum and minimun MAF cutoffs to test the rare variants
        * e.g. --maxMAF_in_groupTest=0.1 and --minMAF_in_groupTest_Exclude=0 are specified to tests all variants with 0 < MAF <= 0.1

* Allows for multiple weight settings
    * By default, the program first checks if per-marker-weight is provided in the group file. If not, the program calcuates weights based on MAF from the Beta distribution with parameters weights.beta. By default --weights.beta=1;25, which means weights are calculated using Beta(MAF, 1, 25) that weights rarer weights more
    * --is_equal_weight_in_groupTest=TRUE can be specified to use equal weights for all variants

    

```
##step1 output prefix and step 2 output prefix
step1prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1
step2prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis
```


```
Rscript step2_tests_qtl.R       \
        --bedFile=./input/n.indep_100_n.cell_1.bed      \
        --bimFile=./input/n.indep_100_n.cell_1.bim      \
        --famFile=./input/n.indep_100_n.cell_1.fam      \
        --SAIGEOutputFile=${step2prefix}_rare     \
        --chrom=2       \
        --maxMAF_in_groupTest=0.1   \
        --minMAF_in_groupTest_Exclude=0       \
        --groupFile=${groupFile}        \
        --annotation_in_groupTest=null  \
        --MACCutoff_to_CollapseUltraRare=10     \
        --is_single_in_groupTest=TRUE  \
        --is_equal_weight_in_groupTest=TRUE    \
        --LOCO=FALSE    \
        --GMMATmodelFile=${step1prefix}.rda     \
        --SPAcutoff=2 \
        --varianceRatioFile=${step1prefix}.varianceRatio.txt    \
        --markers_per_chunk=10000
```

* Using **--bgenFile, --bgenFileIndex, --AlleleOrder, --sampleFile** for bgen input

* Using *--vcfFile --vcfFileIndex --vcfField, --chrom* for VCF, BCF, and SAV input


## Conditional analysis

* --condition = Genetic marker ids (**chr:pos:ref:alt**) separated by comma. e.g.chr3:101651171:C:T,chr3:101651186:G:A
* conditioning markers MUST be specified in the same order as stored in the dosage file (e.g. in the VCF file)

```
Rscript step2_tests_qtl.R       \
        --bedFile=./input/n.indep_100_n.cell_1.bed      \
        --bimFile=./input/n.indep_100_n.cell_1.bim      \
        --famFile=./input/n.indep_100_n.cell_1.fam      \
        --SAIGEOutputFile=${step2prefix}_rare_cond     \
        --chrom=2       \
        --maxMAF_in_groupTest=0.1   \
        --minMAF_in_groupTest_Exclude=0       \
        --groupFile=${groupFile}        \
        --annotation_in_groupTest=null  \
        --MACCutoff_to_CollapseUltraRare=10     \
        --is_single_in_groupTest=TRUE  \
        --is_equal_weight_in_groupTest=TRUE    \
        --LOCO=FALSE    \
        --GMMATmodelFile=${step1prefix}.rda     \
        --SPAcutoff=2 \
        --varianceRatioFile=${step1prefix}.varianceRatio.txt    \
        --markers_per_chunk=10000	\
        --condition=2:300114:1:2,2:609965:1:2
```


## Input files

* Please refer to the Single-variant test Step 2 input files for details. In addition, there are additonal input files required for the set-based tests

1. (Required. Specific for set-based tests)** Group file containing marker IDs, annotations, and/or weights for each set (gene or region).
        * The first column contains the set name.
        * Group file has 2 or 3 lines for each set.
        * marker IDs and annotations are required and weights are optional.
        * The second column has var (indicating the line is for marker IDs), anno (indicating the line is for annotations), and weight (indicating the line is for weights of markers used in the set-based tests).

    ```
    ##group file example
    1_10539_1376204 var 1:715265:C:T 1:715367:A:G 1:717485:C:A
    1_10539_1376204 anno null null null
    1_1376205_2215495 var 1:1376214:C:G 1:1376567:A:G 1:1377151:G:T
    1_1376205_2215495 anno null null null

    ##group file can contain user specified weights
    1_10539_1376204 var 1:715265:C:T 1:715367:A:G 1:717485:C:A
    1_10539_1376204 anno null null null
    1_10539_1376204 weight:a 1 2 3
    1_10539_1376204 weight:b 4 5 6 
    ```

## Output file

* A file with set-based association test results

    ```
    head -n 1  ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis_rare
    ```

```
Region: set name
Group: annotation mask
min_MAF: Minimum MAF cutoff
max_MAF: maximum MAF cutoff
Pvalue: p value for ACAT-O test that combines all set-based p-values
Pvalue_ACATV: p value for ACAT-V test that combines all single-variant p-values
Pvalue_SKATO: p value for SKAT-O test (if is_SKATO=TRUE)
Pvalue_Burden: p value for BURDEN test
Pvalue_SKAT: p value for SKAT test
BETA_Burden: effect size of BURDEN test
SE_Burden: standard error of BETA_Burden

MAC: minor allele count in the set
Number_rare: number of markers that are not ultra-rare with MAC > MACCutoff_to_CollapseUltraRare (=10 by default)
Number_ultra_rare: number of markers that are ultra-rare with MAC <= MACCutoff_to_CollapseUltraRare (=10 by default)

```

* A file with association test results for single markers in the set-based tests
    ** Please refer to the Single-variant test Step 2 output files for details.

    ```
    less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis_rare.singleAssoc.txt
    ```

* A file with marker lists for region/set-based tests (--is_output_markerList_in_groupTest=TRUE)
  
    ```
    less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis_rare.markerList.txt
    ```

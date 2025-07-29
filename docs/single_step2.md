---
layout: default
title: Step 2 - single variant test (common variants)
nav_order: 1
description: "Step 2 - single variant test (common variants)"
parent: cis-eQTL test
---

## Step 2: performing single-variant association tests

* File formats for dosages/genotypes of genetic variants to be tested can be used: PLINK, VCF, [BGEN](https://bitbucket.org/gavinband/bgen/overview), [SAV](https://github.com/statgen/savvy)
* Conditional analysis based summary stats can be performed (using the --condition flag) in Step 2, see below 
* To query and test a subset of markers 
* * both variant IDs (chr:pos:ref:alt) and range of chromosome positions (chr start end) (--idstoIncludeFile=./input/includeID.list_1, --rangestoIncludeFile=./input/includerange.txt)
* * for VCF and SAV input, when query with chromosome range,  only one range of chromosome positions can be specified (--rangestoIncludeFile=./input/includerange_oneline.txt)
* --markers_per_chunk can be used to specify the number of markers to test and output as one chunk. default=10000. Note that a small number may slow down the job. It is required that this number is >= 1000.  
* For VCF/BCF/SAV input, --vcfField=DS to test dosages and  --vcfField=GT to test genotypes. Use --vcfFile to specify VCF, BCF or SAV files. For VCF and BCF input, a .csi index file is required in the same directory. For SAV input, a .s1r index file created with savvy is required in the same directory. Index files need to have the same name as the VCF, BCF, or SAV files with suffix: csi or s1r. Note: --vcfFileIndex is used to specify the index file. This option will be removed in the next version (v1.1.4). 
* By default, missing genotypes/dosages will be imputed as the best guessed gentoypes/dosages (as round(2*freq) with --impute_method=best_guess). Note that currently dropping samples with missing genotypes/dosages is not supported.
* --sampleFile is used specify a file with sample IDs for bgen file. 

```
#check the help info for step 2
Rscript step2_tests_qtl.R --help
```

* --SPAcutoff=2. if chi-square stat > 4 (approximately equivalent to p-value < 0.05), saddlepoint approximation is used to obtain accurate p-values than Normal approximation
*--rangestoIncludeFile= is used to specify the range of chromosome positions to be tested, which can correspond to the cis region of the gene

```
regionFile=./input/gene_1_cis_region.txt
echo -e "2\t300001\t610001" > ${regionFile}
```


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
        --SAIGEOutputFile=${step2prefix}     \
        --chrom=2       \
        --minMAF=0 \
        --minMAC=20 \
        --LOCO=FALSE    \
        --GMMATmodelFile=${step1prefix}.rda     \
        --SPAcutoff=2 \
        --varianceRatioFile=${step1prefix}.varianceRatio.txt    \
        --rangestoIncludeFile=${regionFile}     \
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
        --SAIGEOutputFile=${step2prefix}_cond     \
        --chrom=2       \
        --minMAF=0 \
        --minMAC=20 \
        --LOCO=FALSE    \
        --GMMATmodelFile=${step1prefix}.rda     \
        --SPAcutoff=2 \
        --varianceRatioFile=${step1prefix}.varianceRatio.txt    \
        --rangestoIncludeFile=${regionFile}     \
        --markers_per_chunk=10000	\
        --condition=2:300096:2:1,2:609979:1:2
```


## Input files

1. (Required) Dosage file
SAIGE supports different formats for dosages: PLINK, VCF, BCF, [BGEN](http://www.well.ox.ac.uk/~gav/bgen_format/bgen_format_v1.2.html) and [SAV](https://github.com/statgen/savvy).

    * PLINK

    ```
    --bedFile=./input/n.indep_100_n.cell_1.bed      \
    --bimFile=./input/n.indep_100_n.cell_1.bim      \
    --famFile=./input/n.indep_100_n.cell_1.fam
    ```

    * BGEN

    ```
    ./input/genotype_100markers.bgen
    ./input/genotype_100markers.bgen.bgi 
    ```

    * VCF containing genotypes

    ```
    #go to input
    cd ./input
    #index file can be generated using tabix
    tabix --csi -p vcf genotype_10markers.vcf.gz
    zcat genotype_10markers.vcf.gz | less -S
    genotype_10markers.vcf.gz.csi
    ```

    * VCF containing dosages

    ```
    #index file can be generated using tabix
    tabix --csi -p vcf dosage_10markers.vcf.gz    
    zcat dosage_10markers.vcf.gz | less -S
    dosage_10markers.vcf.gz.csi
    ```

    * SAV

    ```
    dosage_10markers.sav
    dosage_10markers.sav.s1r
    ```

2. (Optional. Only for BGEN file not containing sample IDs) Sample file <br\>
The option was originally for BGEN file that does not contain sample information. 

The file can be in two different formats: 
contains one column for sample IDs corresponding to the sample order in the dosage file. **No header is included.** 
    ```
    less -S ./input/samplelist.txt
    ```
or

in the default bgen .sample format

    ```
    less -S ./input/genotype_100markers_2chr.sample
    ```


3. Output in Step 1. Model file from step 1

    ```
    ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.rda
    ```

4. Output in Step 1. Variance ratio file from step 1

    ```
    ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.varianceRatio.txt
    ```


## Output file
1. A file with association test results

    ```
    less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis

    ```
2. An index for the output file. This file contains the progress of the association test. If specifying --is_overwrite_output=FALSE in Step 2, the program will check this index and continue the unfinished analysis instead of starting over from the beginining

    ```
    less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis.index
    ```

NOTE:
* Association results are with regard to Allele2. 


```
#check the header
head -n 1 output/genotype_100markers_marker_vcf_cond.txt
```

<!---
<img src="{{site.baseurl | prepend: site.url}}/assets/img/SAIGE-step2-output-header.png" width="500">
--->



```
CHR: chromosome
POS: genome position 
SNPID: variant ID
Allele1: allele 1
Allele2: allele 2
AC_Allele2: allele count of allele 2
AF_Allele2: allele frequency of allele 2
MissingRate: missing rate (If the markers in the dosage/genotype input are imputed with is_imputed_data=TRUE, imputationInfo will be output instead of MissingRate)
imputationInfo: imputation info (output with is_imputed_data=TRUE). If not in dosage/genotype input file, will output 1
BETA: effect size of allele 2
SE: standard error of BETA
Tstat: score statistic of allele 2
var: estimated variance of score statistic
p.value: p value (with SPA applied for binary traits)
p.value.NA: p value when SPA is not applied (only for binary traits)
Is.SPA: whether SPA is converged or not 

#if --condition= is used for conditioning analysis, the conditional analysis results will be output  
BETA_c, SE_c, Tstat_c, var_c, p.value_c, p.value.NA_c 
```

---
layout: default
title: Step 2 - single variant test (common variants)
nav_order: 1
description: "Step 2 - single variant test (common variants)"
parent: cis-eQTL test
---

# Step 2: Single Variant Association Tests

## Overview

Step 2 performs single-variant association tests using the null model fitted in Step 1. This step supports multiple file formats and allows for conditional analysis and region-specific testing.

## Supported File Formats

SAIGE-QTL supports multiple formats for dosages/genotypes:
- **PLINK** (BED/BIM/FAM)
- **VCF/BCF** (with .csi index)
- **BGEN** (with .bgi index)
- **SAV** (with .s1r index)

## Key Features

- **Conditional analysis** using summary statistics (with `--condition` flag)
- **Subset testing** by variant IDs or chromosome ranges
- **Chunked processing** with `--markers_per_chunk` (default: 10,000, minimum: 1,000)
- **Missing genotype imputation** using best guess method

## Getting Started

### Check Help Information and the job scripts should use the corresponding command to call the wrapper function **step2_tests_qtl.R**

Note: Pixi script is showing here for examples

#### Pixi Installation
```bash
# Navigate to SAIGEQTL directory first
cd SAIGEQTL/extdata/
pixi run --manifest-path=../pixi.toml Rscript step2_tests_qtl.R --help
```

## Basic Association Testing

### Prepare Region File

Define the cis-region for testing:
```bash
regionFile=./input/gene_1_cis_region.txt
echo -e "2\t300001\t610001" > ${regionFile}
```
### Set Output Prefixes
```bash
step1prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1
step2prefix=./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis
```

### Run Association Tests

NOTE: the corresponding command to call the wrapper function **step2_tests_qtl.R** needs to be modified if different ways used for installing the pcakge as shown above

```bash
cd SAIGEQTL/extdata/
pixi run --manifest-path=../pixi.toml Rscript step2_tests_qtl.R \
    --bedFile=./input/n.indep_100_n.cell_1.bed \
    --bimFile=./input/n.indep_100_n.cell_1.bim \
    --famFile=./input/n.indep_100_n.cell_1.fam \
    --SAIGEOutputFile=${step2prefix} \
    --chrom=2 \
    --minMAF=0 \
    --minMAC=20 \
    --LOCO=FALSE \
    --GMMATmodelFile=${step1prefix}.rda \
    --SPAcutoff=2 \
    --varianceRatioFile=${step1prefix}.varianceRatio.txt \
    --rangestoIncludeFile=${regionFile} \
    --markers_per_chunk=10000
```

## Alternative Input Formats

### BGEN Files
```bash
# Add these parameters instead of --bedFile/--bimFile/--famFile
--bgenFile=./input/genotype_100markers.bgen \
--bgenFileIndex=./input/genotype_100markers.bgen.bgi \
--AlleleOrder=ref-first \
--sampleFile=./input/samplelist.txt
```

### VCF/BCF/SAV Files
```bash
# Add these parameters instead of PLINK files
--vcfFile=./input/genotype_10markers.vcf.gz \
--vcfFileIndex=./input/genotype_10markers.vcf.gz.csi \
--vcfField=DS \  # Use DS for dosages, GT for genotypes
--chrom=2
```

## Conditional Analysis

Perform conditional analysis by specifying conditioning markers:

```bash
cd SAIGEQTL/extdata
pixi run --manifest-path=../pixi.toml Rscript step2_tests_qtl.R \
    --bedFile=./input/n.indep_100_n.cell_1.bed \
    --bimFile=./input/n.indep_100_n.cell_1.bim \
    --famFile=./input/n.indep_100_n.cell_1.fam \
    --SAIGEOutputFile=${step2prefix}_cond \
    --chrom=2 \
    --minMAF=0 \
    --minMAC=20 \
    --LOCO=FALSE \
    --GMMATmodelFile=${step1prefix}.rda \
    --SPAcutoff=2 \
    --varianceRatioFile=${step1prefix}.varianceRatio.txt \
    --rangestoIncludeFile=${regionFile} \
    --markers_per_chunk=10000 \
    --condition=2:300096:2:1,2:609979:1:2
```

**Note**: Conditioning markers must be specified as **chr:pos:ref:alt** and in the same order as stored in the dosage file.

## Input Files

### 1. Dosage/Genotype Files (Required)

#### PLINK Format
```bash
--bedFile=./input/n.indep_100_n.cell_1.bed
--bimFile=./input/n.indep_100_n.cell_1.bim
--famFile=./input/n.indep_100_n.cell_1.fam
```

#### BGEN Format
```bash
# Files needed:
./input/genotype_100markers.bgen
./input/genotype_100markers.bgen.bgi
```

#### VCF Format (Genotypes)

##### View and Index VCF Files

```bash
cd ./input
# Create index file using tabix
tabix --csi -p vcf genotype_10markers.vcf.gz
# View file content
zcat genotype_10markers.vcf.gz | less -S
# Index file: genotype_10markers.vcf.gz.csi
```


#### VCF Format (Dosages)

##### Index and View Dosage VCF

```bash
# Create index file using tabix
tabix --csi -p vcf dosage_10markers.vcf.gz
# View file content
zcat dosage_10markers.vcf.gz | less -S
# Index file: dosage_10markers.vcf.gz.csi


#### SAV Format
```bash
# Files needed:
dosage_10markers.sav
dosage_10markers.sav.s1r
```

### 2. Sample File (Optional - for BGEN without sample IDs)

#### View Sample File

```bash
# Simple format (no header)
less -S ./input/samplelist.txt

# BGEN .sample format
less -S ./input/genotype_100markers_2chr.sample
```


### 3. Step 1 Output Files (Required)

- **Model file**: `./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.rda`
- **Variance ratio file**: `./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1.varianceRatio.txt`

## Key Parameters Explained

| Parameter | Description | Example Value |
|-----------|-------------|---------------|
| `--SAIGEOutputFile` | Output file prefix | `./output/results_cis` |
| `--chrom` | Chromosome to analyze | `2` |
| `--minMAF` | Minimum minor allele frequency | `0` |
| `--minMAC` | Minimum minor allele count | `20` |
| `--LOCO` | Leave-one-chromosome-out | `FALSE` |
| `--SPAcutoff` | SPA threshold (χ² statistic) | `2` |
| `--markers_per_chunk` | Markers per output chunk | `10000` |
| `--condition` | Conditioning markers | `chr:pos:ref:alt,chr:pos:ref:alt` |
| `--rangestoIncludeFile` | Region file for testing | `./input/region.txt` |
| `--vcfField` | VCF field to use | `DS` (dosages) or `GT` (genotypes) |

## Output Files

### 1. Association Results File

#### View Results

```bash
less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis
```


### 2. Index File

Tracks analysis progress for resuming interrupted jobs:

```bash
less -S ./output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis.index
```

## Output Format Explanation

### Standard Columns

#### View Header

```bash
head -n 1 output/nindep_100_ncell_100_lambda_2_tauIntraSample_0.5_gene_1_cis
```


### Column Descriptions

| Column | Description |
|--------|-------------|
| `CHR` | Chromosome |
| `POS` | Genome position |
| `SNPID` | Variant ID |
| `Allele1` | Reference allele |
| `Allele2` | Alternative allele (effect allele) |
| `AC_Allele2` | Allele count of Allele2 |
| `AF_Allele2` | Allele frequency of Allele2 |
| `MissingRate` | Missing rate (or `imputationInfo` if imputed) |
| `BETA` | Effect size of Allele2 |
| `SE` | Standard error of BETA |
| `Tstat` | Score statistic of Allele2 |
| `var` | Estimated variance of score statistic |
| `p.value` | P-value (with SPA if applicable) |
| `p.value.NA` | P-value without SPA (binary traits only) |
| `Is.SPA` | Whether SPA converged |

### Conditional Analysis Columns

When using `--condition=`, additional columns are output:
- `BETA_c`, `SE_c`, `Tstat_c`, `var_c`, `p.value_c`, `p.value.NA_c`

**Note**: Association results are reported with regard to **Allele2** (the alternative allele).

## Best Practices

### Performance Optimization
- Use appropriate `--markers_per_chunk` values (≥1000, typically 10,000)
- Consider chromosomal parallelization for large datasets
- Use `--is_overwrite_output=FALSE` to resume interrupted analyses

### Quality Control
- Set reasonable `--minMAC` thresholds (typically ≥20)
- Monitor SPA convergence in results
- Check for appropriate effect size distributions

### File Management
- Ensure proper indexing for VCF/BCF files (.csi)
- Verify sample ID consistency across files
- Use absolute paths for containerized environments

### Region-Based Testing
- Define appropriate cis-regions (typically ±500kb from gene)
- Use single-line range files for VCF/SAV input
- Consider multiple ranges for trans-QTL analyses

## Troubleshooting

### Common Issues

1. **Index File Errors**
   - Ensure .csi files exist and are properly named
   - Recreate index files if corrupted
   - Check file permissions and accessibility

2. **Memory Issues**
   - Reduce `--markers_per_chunk` for large datasets
   - Monitor memory usage during processing
   - Use appropriate compute resources

3. **Sample Mismatch**
   - Verify sample IDs match between genotype and phenotype files
   - Check sample file format for BGEN inputs
   - Ensure proper sample ordering

4. **Conditional Analysis**
   - Verify conditioning marker format (chr:pos:ref:alt)
   - Ensure markers exist in the dosage file
   - Check marker ordering consistency

5. **File Path Issues**
   - Use absolute paths in containerized environments
   - Verify volume mounting in Docker/Singularity
   - Check file accessibility and permissions

### Performance Tips

- **Parallel Processing**: Run multiple chromosomes simultaneously
- **Resource Allocation**: Monitor CPU and memory usage
- **Storage**: Use fast I/O storage for large datasets
- **Indexing**: Ensure all index files are current and accessible

## Next Steps

After successful completion of Step 2, you can:
- Proceed to Step 3 for gene-level p-value calculations
- Perform additional conditional analyses
- Analyze results for significant associations
- Conduct meta-analyses across multiple datasets

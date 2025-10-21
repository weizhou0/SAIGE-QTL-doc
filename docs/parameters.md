---
layout: default
title: Parameters & Options
nav_order: 8
description: "Comprehensive guide to all parameters in SAIGE-QTL scripts."
---

# Parameters & Options

Comprehensive reference for all command-line parameters in SAIGE-QTL.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }

- [createSparseGRM.R](#create-sparse-grm)
- [step1_fitNULLGLMM_qtl.R](#step1-fit-null-glmm)
- [step2_tests_qtl.R](#step2-association-tests)
- [step3_gene_pvalue_qtl.R](#step3-gene-pvalue)
- [makeGroupFile.R](#make-group-file)

</details>

---

## createSparseGRM.R
{: #create-sparse-grm }

Creates a sparse genetic relationship matrix (GRM). Optional preprocessing step for computational efficiency.

### Input Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--plinkFile` | character | "" | PLINK file prefix (without extension) |
| `--bedFile` | character | "" | PLINK bed file (if not using `--plinkFile`) |
| `--bimFile` | character | "" | PLINK bim file (if not using `--plinkFile`) |
| `--famFile` | character | "" | PLINK fam file (if not using `--plinkFile`) |

### GRM Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--relatednessCutoff` | numeric | 0.125 | Kinship threshold for relatedness (0.125 = second-degree relatives) |
| `--numRandomMarkerforSparseKin` | integer | 2000 | Number of random markers to identify related samples |
| `--isDiagofKinSetAsOne` | logical | FALSE | Set GRM diagonal elements to 1 |
| `--minMAFforGRM` | numeric | 0.01 | Minimum MAF for markers used in GRM |
| `--maxMissingRateforGRM` | numeric | 0.15 | Maximum missing rate for markers used in GRM |

### Computation

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--nThreads` | integer | 16 | Number of CPU threads |
| `--memoryChunk` | numeric | 2 | Memory chunk size (Gb) |
| `--outputPrefix` | character | "~/" | Output path and prefix |

---

## step1_fitNULLGLMM_qtl.R
{: #step1-fit-null-glmm }

Fits the null Poisson mixed model for eQTL analysis.

### Input Files

#### Genotype Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--plinkFile` | character | "" | PLINK file prefix for GRM construction and variance ratio estimation |
| `--bedFile` | character | "" | PLINK bed file |
| `--bimFile` | character | "" | PLINK bim file |
| `--famFile` | character | "" | PLINK fam file |

#### Phenotype Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--phenoFile` | character | "" | **Required.** Tab/space-delimited phenotype file with header |
| `--phenoCol` | character | "" | **Required.** Phenotype column name (e.g., gene ID) |
| `--sampleIDColinphenoFile` | character | "IID" | **Required.** Sample ID column name |
| `--cellIDColinphenoFile` | character | "" | Cell ID column name (for single-cell data) |
| `--SampleIDIncludeFile` | character | "" | File with sample IDs to include |

#### Sparse GRM Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--sparseGRMFile` | character | "" | Pre-calculated sparse GRM file |
| `--sparseGRMSampleIDFile` | character | "" | Sample IDs for sparse GRM (no header) |

#### Additional Matrices

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--VmatFilelist` | character | "" | Comma-separated list of additional covariance matrices |
| `--VmatSampleFilelist` | character | "" | Sample IDs for each V matrix |

### Phenotype and Covariates

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--traitType` | character | "binary" | **Required.** Trait type: "count" (eQTL), "binary", or "quantitative" |
| `--covarColList` | character | "" | Comma-separated covariate columns |
| `--sampleCovarColList` | character | "" | Comma-separated individual-level covariates |
| `--qCovarColList` | character | "" | Comma-separated categorical covariates (must also be in `--covarColList`) |
| `--offsetCol` | character | NULL | Offset column (e.g., log total UMI counts) |
| `--varWeightsCol` | character | NULL | Variance weights column |
| `--isRemoveZerosinPheno` | logical | FALSE | Remove zeros from phenotype |
| `--invNormalize` | logical | FALSE | Inverse normalize quantitative traits |
| `--minCovariateCount` | numeric | -1 | Min count for binary covariates (-1 = no filter) |

### GRM and Relatedness

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--useGRMtoFitNULL` | logical | TRUE | Use GRM in null model fitting |
| `--useSparseGRMtoFitNULL` | logical | FALSE | Use sparse GRM for null model |
| `--useSparseGRMforVarRatio` | logical | FALSE | Use sparse GRM for variance ratio estimation |
| `--relatednessCutoff` | numeric | 0 | Kinship threshold for sparse GRM |
| `--isDiagofKinSetAsOne` | logical | FALSE | Set GRM diagonal to 1 |
| `--minMAFforGRM` | numeric | 0.01 | Min MAF for GRM markers |
| `--maxMissingRateforGRM` | numeric | 0.15 | Max missing rate for GRM markers |

### Model Fitting

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--tol` | numeric | 0.02 | Convergence tolerance |
| `--maxiter` | integer | 20 | Maximum iterations |
| `--tolPCG` | numeric | 1e-5 | PCG convergence tolerance |
| `--maxiterPCG` | integer | 500 | Maximum PCG iterations |
| `--tauInit` | character | "0,0" | Initial tau values (comma-separated) |
| `--isCovariateOffset` | logical | TRUE | Estimate fixed effect coefficients |
| `--isCovariateTransform` | logical | TRUE | Apply QR transformation to covariates |
| `--skipModelFitting` | logical | FALSE | Skip fitting (requires existing model file) |

### Variance Ratio Estimation

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--skipVarianceRatioEstimation` | logical | FALSE | Skip variance ratio estimation |
| `--numRandomMarkerforVarianceRatio` | integer | 30 | Initial number of random markers (auto-increases) |
| `--ratioCVcutoff` | numeric | 0.001 | CV threshold for variance ratio |
| `--traceCVcutoff` | numeric | 0.0025 | CV threshold for trace estimation |
| `--nrun` | numeric | 30 | Number of runs for trace estimation |
| `--includeNonautoMarkersforVarRatio` | logical | FALSE | Include non-autosomal markers |

#### Categorical Variance Ratios

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--isCateVarianceRatio` | logical | FALSE | Estimate variance ratios by MAC category |
| `--cateVarRatioMinMACVecExclude` | character | "10,20.5" | Lower MAC bounds (comma-separated) |
| `--cateVarRatioMaxMACVecInclude` | character | "20.5" | Upper MAC bounds (comma-separated) |

### LOCO (Leave-One-Chromosome-Out)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--LOCO` | logical | TRUE | Apply LOCO approach |
| `--isLowMemLOCO` | logical | FALSE | Output model files by chromosome (reduces memory) |

### Output Control

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--outputPrefix` | character | "~/" | **Required.** Output path and prefix |
| `--outputPrefix_varRatio` | character | "" | Variance ratio file prefix (defaults to `--outputPrefix`) |
| `--IsOverwriteVarianceRatioFile` | logical | FALSE | Overwrite existing variance ratio file |
| `--isStoreSigma` | logical | TRUE | Store inverse Sigma matrix |
| `--isShrinkModelOutput` | logical | TRUE | Remove unnecessary objects (reduces file size) |
| `--isExportResiduals` | logical | FALSE | Export residual vectors |

### Computation

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--nThreads` | integer | 1 | Number of CPU threads |
| `--memoryChunk` | numeric | 2 | Memory chunk size (Gb) |
| `--SPAcutoff` | numeric | 2 | SPA threshold (in SD units) |

### Sex-Specific Analysis

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--FemaleOnly` | logical | FALSE | Run for females only (requires `--sexCol`, `--FemaleCode`) |
| `--MaleOnly` | logical | FALSE | Run for males only (requires `--sexCol`, `--MaleCode`) |
| `--sexCol` | character | "" | Sex column name |
| `--FemaleCode` | character | "1" | Female indicator value |
| `--MaleCode` | character | "0" | Male indicator value |

---

## step2_tests_qtl.R
{: #step2-association-tests }

Performs association tests using the null model from Step 1.

### Genotype Input Files

**Choose one format:**

#### VCF/BCF Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--vcfFile` | character | "" | VCF/BCF file path |
| `--vcfFileIndex` | character | "" | VCF index (.csi) |
| `--vcfField` | character | "DS" | Field to extract: "DS" (dosage) or "GT" (genotype) |

#### SAV Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--savFile` | character | "" | SAV file path |
| `--savFileIndex` | character | "" | SAV index (.s1r) |

#### BGEN Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--bgenFile` | character | "" | BGEN file (v1.2, 8-bit compression) |
| `--bgenFileIndex` | character | "" | BGEN index (.bgi) |
| `--sampleFile` | character | "" | Sample file for BGEN (required for v≥0.38) |

#### PLINK Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--bedFile` | character | "" | PLINK bed file |
| `--bimFile` | character | "" | PLINK bim file |
| `--famFile` | character | "" | PLINK fam file |
| `--AlleleOrder` | character | "alt-first" | Allele order: "alt-first" or "ref-first" |

### Variant Selection

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--idstoIncludeFile` | character | "" | File with variant IDs (chr:pos_ref/alt, one per line) |
| `--rangestoIncludeFile` | character | "" | File with regions (chr, start, end; no header) |
| `--chrom` | character | "" | Chromosome (required for LOCO or VCF/SAV) |

### Quality Filters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--minMAF` | numeric | 0 | Minimum minor allele frequency |
| `--minMAC` | numeric | 0.5 | Minimum minor allele count |
| `--minInfo` | numeric | 0 | Minimum imputation INFO (if `--is_imputed_data=TRUE`) |
| `--maxMissing` | numeric | 0.15 | Maximum missing rate |
| `--is_imputed_data` | logical | FALSE | Whether data is imputed |
| `--impute_method` | character | "best_guess" | Missing imputation: "best_guess", "mean", or "minor" |
| `--dosage_zerod_cutoff` | numeric | 0.2 | Dosage zeroing threshold (for imputed data) |
| `--dosage_zerod_MAC_cutoff` | numeric | 10 | MAC threshold for dosage zeroing |

### Step 1 Model Files

**Choose one:**

#### Single Phenotype

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--GMMATmodelFile` | character | "" | Model file (.rda) from Step 1 |
| `--varianceRatioFile` | character | "" | Variance ratio file (.varianceRatio.txt) from Step 1 |

#### Multiple Phenotypes

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--GMMATmodel_varianceRatio_multiTraits_File` | character | "" | 3-column file: phenotype, model file, variance ratio file |

### Single-Variant Tests

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--LOCO` | logical | TRUE | Apply LOCO (requires `--chrom`) |
| `--SPAcutoff` | numeric | 2 | SPA threshold (in SD units) |
| `--is_Firth_beta` | logical | FALSE | Use Firth correction for effect size (binary traits) |
| `--pCutoffforFirth` | numeric | 0.01 | P-value threshold for Firth |

### Set-Based Tests

#### Group File

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--groupFile` | character | "" | Group file with variant sets |

#### Variant Filters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--maxMAF_in_groupTest` | character | "0.0001,0.001,0.01" | Max MAF for masks (comma-separated) |
| `--minMAF_in_groupTest_Exclude` | character | NULL | Min MAF for masks (comma-separated) |
| `--maxMAC_in_groupTest` | character | "0" | Max MAC for masks (comma-separated) |
| `--minMAC_in_groupTest_Exclude` | character | NULL | Min MAC for masks (comma-separated) |
| `--annotation_in_groupTest` | character | "lof,missense;lof,missense;lof;synonymous" | Annotations (`;` combines multiple) |
| `--MACCutoff_to_CollapseUltraRare` | numeric | 10 | MAC threshold for collapsing ultra-rare variants |
| `--minGroupMAC_in_BurdenTest` | numeric | 5 | Min MAC for burden test pseudo-marker |

#### Test Types

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--r.corr` | numeric | 0 | Test type: 0=SKAT-O, 1=Burden only |
| `--is_SKATO` | logical | FALSE | Perform SKAT-O test |
| `--weights.beta` | character | "1;25" | Beta weights (a;b), multiple sets with commas |
| `--is_equal_weight_in_groupTest` | logical | FALSE | Use equal weights |
| `--is_single_in_groupTest` | logical | FALSE | Output single-variant results |
| `--is_output_markerList_in_groupTest` | logical | FALSE | Output variant lists for each mask |

#### Conditional Analysis

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--condition` | character | "" | Conditioning variants (chr:pos_ref/alt, comma-separated) |
| `--weights_for_condition` | character | NULL | Weights for conditioning variants (`;` and `,` separated) |

### Sparse GRM

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--sparseGRMFile` | character | "" | Sparse GRM file from Step 1 |
| `--sparseGRMSampleIDFile` | character | "" | Sample IDs for sparse GRM |
| `--relatednessCutoff` | numeric | 0 | Kinship threshold |
| `--is_sparseGRM` | logical | FALSE | Use sparse GRM |

### Output

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--SAIGEOutputFile` | character | "" | **Required.** Output file path |
| `--markers_per_chunk` | numeric | 10000 | Variants per chunk (single-variant tests) |
| `--groups_per_chunk` | numeric | 100 | Groups per chunk (set-based tests) |
| `--markers_per_chunk_in_groupTest` | numeric | 100 | Variants per chunk for covariance matrix |
| `--is_output_moreDetails` | logical | FALSE | Output genotype counts |
| `--is_overwrite_output` | logical | TRUE | Overwrite existing output |

### Variance Ratio Categories

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--cateVarRatioMinMACVecExclude` | character | "10,20.5" | Lower MAC bounds (must match Step 1) |
| `--cateVarRatioMaxMACVecInclude` | character | "20.5" | Upper MAC bounds (must match Step 1) |

### Advanced Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--is_fastTest` | logical | FALSE | Use fast test mode |
| `--pval_cutoff_for_fastTest` | numeric | 0.05 | P-value cutoff for fast mode |
| `--is_noadjCov` | logical | TRUE | Regress covariates from genotypes |
| `--max_MAC_for_ER` | numeric | 4 | Max MAC for efficient resampling |
| `--is_EmpSPA` | logical | FALSE | Use empirical SPA |

---

## step3_gene_pvalue_qtl.R
{: #step3-gene-pvalue }

Calculates gene-level p-values using ACAT (Aggregated Cauchy Association Test).

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--assocFile` | character | "" | **Required.** Association file from Step 2 |
| `--weightFile` | character | "" | Weight file (2 columns: MarkerID, weight) |
| `--geneName` | character | "" | Gene name for output (default: "gene") |
| `--genePval_outputFile` | character | "" | **Required.** Output file path |

**Note:** Weight file format requires header with columns "MarkerID" and "weight".

---

## makeGroupFile.R
{: #make-group-file }

Creates group files for set-based association tests.

### Genotype Input Files

**Choose one format:**

#### VCF Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--vcfFile` | character | "" | VCF file path |
| `--vcfFileIndex` | character | "" | VCF index (.csi) |
| `--vcfField` | character | "DS" | Field: "DS" or "GT" |

#### SAV Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--savFile` | character | "" | SAV file path |
| `--savFileIndex` | character | "" | SAV index (.s1r) |

#### BGEN Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--bgenFile` | character | "" | BGEN file (v1.2, 8-bit) |
| `--bgenFileIndex` | character | "" | BGEN index (.bgi) |
| `--sampleFile` | character | "" | Sample file for BGEN |

#### PLINK Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--bedFile` | character | "" | PLINK bed file |
| `--bimFile` | character | "" | PLINK bim file |
| `--famFile` | character | "" | PLINK fam file |

### Region and Output

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--regionFile` | character | "" | **Required.** Regions file (chr, start, end; no header) |
| `--AlleleOrder` | character | "alt-first" | Allele order: "alt-first" or "ref-first" |
| `--outputPrefix` | character | "~/" | **Required.** Output path and prefix |

---

**Required parameters** are marked in **bold** in the Description column. These must be specified for the script to run.

---

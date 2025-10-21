---
layout: default
title: Parameter Explanation
nav_order: 8
description: "Comprehensive guide to all parameters in SAIGE-QTL scripts."
---

# Parameter Explanation
{: .no_toc }

This page provides a comprehensive reference for all command-line parameters available in SAIGE-QTL scripts.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## createSparseGRM.R

Creates a sparse genetic relationship matrix (GRM) for related samples. This is an optional preprocessing step that can improve computational efficiency when sample relatedness exists.

### Input Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--plinkFile` | character | "" | Path to PLINK file prefix (without .bed/.bim/.fam extension). If specified, will automatically use .bed, .bim, and .fam files. |
| `--bedFile` | character | "" | Path to PLINK bed file. Use this if not specifying `--plinkFile`. |
| `--bimFile` | character | "" | Path to PLINK bim file. Use this if not specifying `--plinkFile`. |
| `--famFile` | character | "" | Path to PLINK fam file. Use this if not specifying `--plinkFile`. |

### Output Control

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--outputPrefix` | character | "~/" | Path and prefix for output files. The sparse GRM will be saved with this prefix. |

### Computation Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--nThreads` | integer | 16 | Number of CPU threads to use for parallel computation. |
| `--memoryChunk` | numeric | 2 | Size (in Gb) for each memory chunk during computation. Adjust based on available memory. |
| `--numRandomMarkerforSparseKin` | integer | 2000 | Number of randomly selected markers used to identify related sample pairs for sparse GRM construction. |

### GRM Construction Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--relatednessCutoff` | numeric | 0.125 | Kinship coefficient threshold to determine whether two samples are related. Pairs with kinship below this threshold are treated as unrelated and set to 0 in the sparse GRM. Default 0.125 corresponds to second-degree relatives. |
| `--isDiagofKinSetAsOne` | logical | FALSE | Whether to set diagonal elements of the GRM to 1. If FALSE, diagonal elements represent self-kinship (typically ~0.5 for diploid organisms). |
| `--minMAFforGRM` | numeric | 0.01 | Minimum minor allele frequency for markers used in GRM calculation. Markers with MAF below this threshold are excluded. |
| `--maxMissingRateforGRM` | numeric | 0.15 | Maximum missing rate for markers used in GRM calculation. Markers with missing rate above this threshold are excluded. |

---

## step1_fitNULLGLMM_qtl.R

Fits the null generalized linear mixed model (GLMM) under the null hypothesis of no genetic association. This step estimates variance components and other model parameters used in Step 2 association testing.

### Input Files

#### Genotype Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--plinkFile` | character | "" | Path to PLINK file prefix for creating the genetic relationship matrix (GRM) and estimating variance ratios. Markers can be filtered using `--minMAFforGRM` and `--maxMissingRateforGRM`. |
| `--bedFile` | character | "" | Path to bed file. If `--plinkFile` is specified, this will be automatically set to 'plinkFile.bed'. |
| `--bimFile` | character | "" | Path to bim file. If `--plinkFile` is specified, this will be automatically set to 'plinkFile.bim'. |
| `--famFile` | character | "" | Path to fam file. If `--plinkFile` is specified, this will be automatically set to 'plinkFile.fam'. |

#### Phenotype and Sample Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--phenoFile` | character | "" | **Required**. Path to phenotype file. Must be tab or space delimited with a header. Contains at least a sample ID column and a phenotype column. Can include additional covariate columns. |
| `--sampleIDColinphenoFile` | character | "IID" | **Required**. Column name for sample IDs in the phenotype file. |
| `--cellIDColinphenoFile` | character | "" | Column name for cell IDs in the phenotype file (e.g., barcode). Required for single-cell data with multiple cells per individual. |
| `--SampleIDIncludeFile` | character | "" | Path to file containing sample IDs (one per line) to include in null model fitting. Useful for subsetting samples. |

#### Sparse GRM Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--sparseGRMFile` | character | "" | Path to pre-calculated sparse GRM file (from createSparseGRM.R). If specified with `--useSparseGRMtoFitNULL=TRUE`, this sparse GRM will be used for model fitting. |
| `--sparseGRMSampleIDFile` | character | "" | Path to sample ID file for the sparse GRM (no header, one ID per line). Order must match the sparse GRM file. |

#### Additional Covariance Matrices

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--VmatFilelist` | character | "" | Comma-separated list of additional covariance matrix files to include in the model. Advanced usage for incorporating additional random effects. |
| `--VmatSampleFilelist` | character | "" | Comma-separated list of sample ID files corresponding to each V matrix in `--VmatFilelist`. |

### Phenotype and Covariates

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--phenoCol` | character | "" | **Required**. Column name for the phenotype to test in the phenotype file (e.g., "gene_1" or "ENSG00000123456"). |
| `--traitType` | character | "binary" | **Required**. Type of phenotype: "binary", "quantitative", or "count". For eQTL analysis, use "count" for read count data. |
| `--covarColList` | character | "" | Comma-separated list of covariate column names from the phenotype file. These can be cell-level or individual-level covariates. |
| `--sampleCovarColList` | character | "" | Comma-separated list of individual-level (sample-level) covariates from the phenotype file. These covariates vary across individuals but not across cells within the same individual. |
| `--qCovarColList` | character | "" | Comma-separated list of categorical (qualitative) covariate column names. All categorical covariates must also be included in `--covarColList`. |
| `--longlCol` | character | "" | Column name for longitudinal structure specification (advanced usage). |
| `--offsetCol` | character | NULL | Column name for offset values. For single-cell data, can specify log(total read counts per cell) as an offset term. |
| `--varWeightsCol` | character | NULL | Column name for variance weights (advanced usage for heteroscedastic models). |
| `--isRemoveZerosinPheno` | logical | FALSE | Whether to remove observations with zero phenotype values before analysis. |
| `--invNormalize` | logical | FALSE | Whether to perform inverse normal transformation on quantitative phenotypes. Only applicable when `--traitType=quantitative`. |
| `--minCovariateCount` | numeric | -1 | Minimum count for binary covariates to be retained in the model. Binary covariates with fewer observations in one category than this threshold will be excluded to avoid convergence issues. Default -1 means no exclusion. |

### GRM and Relatedness Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--useGRMtoFitNULL` | logical | TRUE | Whether to use the genetic relationship matrix (GRM) when fitting the null model. Set to FALSE for analyses without sample relatedness. |
| `--useSparseGRMtoFitNULL` | logical | FALSE | Whether to use the sparse GRM (specified by `--sparseGRMFile`) to fit the null model. Sparse GRM can reduce computational burden for large sample sizes. |
| `--useSparseGRMforVarRatio` | logical | FALSE | Whether to use sparse GRM to estimate variance ratios. If TRUE, variance ratios are estimated using full GRM in the numerator and sparse GRM in the denominator. |
| `--minMAFforGRM` | numeric | 0.01 | Minimum minor allele frequency for markers used in GRM construction. |
| `--maxMissingRateforGRM` | numeric | 0.15 | Maximum missing rate for markers used in GRM construction. |
| `--relatednessCutoff` | numeric | 0 | Kinship coefficient threshold for treating two samples as unrelated when using sparse GRM. |
| `--isDiagofKinSetAsOne` | logical | FALSE | Whether to set diagonal elements of the GRM to 1. |

### Model Fitting Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--tol` | numeric | 0.02 | Convergence tolerance for fitting the null GLMM. Smaller values require more stringent convergence. |
| `--maxiter` | integer | 20 | Maximum number of iterations for null GLMM fitting. |
| `--tolPCG` | numeric | 1e-5 | Convergence tolerance for preconditioned conjugate gradient (PCG) algorithm. |
| `--maxiterPCG` | integer | 500 | Maximum number of iterations for PCG algorithm. |
| `--tauInit` | character | "0,0" | Initial values for variance component parameters (tau). Format: comma-separated values. |
| `--isCovariateOffset` | logical | TRUE | Whether to estimate fixed effect coefficients for covariates. If FALSE, covariates are included but their effects are not estimated (treated as offsets). |
| `--isCovariateTransform` | logical | TRUE | Whether to apply QR transformation to covariates. This can improve numerical stability. |
| `--skipModelFitting` | logical | FALSE | Whether to skip model fitting and only estimate variance ratios. If TRUE, requires pre-existing model file (`outputPrefix.rda`). |

### Variance Ratio Estimation

The variance ratio is used to calibrate test statistics in Step 2. It accounts for model misspecification and overdispersion.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--skipVarianceRatioEstimation` | logical | FALSE | Whether to skip variance ratio estimation. If TRUE, variance ratio estimation is not performed. |
| `--numRandomMarkerforVarianceRatio` | integer | 30 | Initial number of randomly selected markers for estimating variance ratio. The number automatically increases by 10 until the coefficient of variation (CV) is below `--ratioCVcutoff`. |
| `--ratioCVcutoff` | numeric | 0.001 | Coefficient of variation (CV) threshold for variance ratio estimation. Marker selection continues until CV is below this threshold. |
| `--traceCVcutoff` | numeric | 0.0025 | CV threshold for trace estimation used in variance ratio calculation. |
| `--nrun` | numeric | 30 | Number of runs for trace estimation in variance ratio calculation. |
| `--isCateVarianceRatio` | logical | FALSE | Whether to estimate variance ratios for different minor allele count (MAC) categories. Useful when variance ratio depends on allele frequency. If TRUE, `--LOCO` must be FALSE. |
| `--cateVarRatioMinMACVecExclude` | character | "10,20.5" | Comma-separated lower bounds for MAC categories when `--isCateVarianceRatio=TRUE`. Length equals number of MAC categories. |
| `--cateVarRatioMaxMACVecInclude` | character | "20.5" | Comma-separated upper bounds for MAC categories when `--isCateVarianceRatio=TRUE`. Length equals number of MAC categories minus 1. |
| `--includeNonautoMarkersforVarRatio` | logical | FALSE | Whether to include non-autosomal markers (e.g., X chromosome) for variance ratio estimation. |

### Leave-One-Chromosome-Out (LOCO)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--LOCO` | logical | TRUE | Whether to apply leave-one-chromosome-out approach. When TRUE, the GRM is constructed excluding markers from the chromosome being tested in Step 2 to avoid proximal contamination. |
| `--isLowMemLOCO` | logical | FALSE | Whether to output model files by chromosome when `--LOCO=TRUE`. If TRUE, reduces memory usage in Steps 1 and 2 by loading only the relevant chromosome's model. |

### Output Control

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--outputPrefix` | character | "~/" | **Required**. Path and prefix for output files. Creates `[prefix].rda` (model file) and `[prefix].varianceRatio.txt` (variance ratio file). |
| `--outputPrefix_varRatio` | character | "" | Alternative path and prefix for variance ratio file. If not specified, uses `--outputPrefix`. |
| `--IsOverwriteVarianceRatioFile` | logical | FALSE | Whether to overwrite existing variance ratio file if it exists. |
| `--isStoreSigma` | logical | TRUE | Whether to store the inverse Sigma matrix in the model output. Recommended to keep TRUE for Step 2. |
| `--isShrinkModelOutput` | logical | TRUE | Whether to remove unnecessary objects from model output to reduce file size. Keeps only objects needed for Step 2. |
| `--isExportResiduals` | logical | FALSE | Whether to export residual vectors for diagnostic purposes. |

### Computation Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--nThreads` | integer | 1 | Number of CPU threads to use for parallel computation. |
| `--memoryChunk` | numeric | 2 | Size (in Gb) for each memory chunk. Adjust based on available memory. |
| `--SPAcutoff` | numeric | 2 | Cutoff for deviation of score test statistic from the mean (in standard deviation units) to perform saddlepoint approximation (SPA) in variance ratio estimation. |

### Sex-Specific Analysis

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--FemaleOnly` | logical | FALSE | Whether to run Step 1 for females only. If TRUE, must specify `--sexCol` and `--FemaleCode`. |
| `--MaleOnly` | logical | FALSE | Whether to run Step 1 for males only. If TRUE, must specify `--sexCol` and `--MaleCode`. |
| `--sexCol` | character | "" | Column name for sex/gender in the phenotype file. |
| `--FemaleCode` | character | "1" | Value in the sex column indicating female samples. |
| `--MaleCode` | character | "0" | Value in the sex column indicating male samples. |

---

## step2_tests_qtl.R

Performs association tests between genetic variants and the phenotype using the null model from Step 1. Supports both single-variant tests and set-based (gene/region) tests.

### Genotype Input Files

SAIGE-QTL supports multiple genotype file formats. Specify one of the following:

#### VCF/BCF Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--vcfFile` | character | "" | Path to VCF or BCF file containing genotype dosages or hard calls. |
| `--vcfFileIndex` | character | "" | Path to VCF index file (`.csi` format created by `tabix -p vcf file.vcf.gz`). |
| `--vcfField` | character | "DS" | Field to extract from VCF: "DS" for dosage or "GT" for genotype. |

#### SAV Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--savFile` | character | "" | Path to SAV file (optimized format for sparse genotype data). |
| `--savFileIndex` | character | "" | Path to `.s1r` file (SAV index file). |

#### BGEN Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--bgenFile` | character | "" | Path to BGEN file. Currently supports version 1.2 with 8-bit compression. |
| `--bgenFileIndex` | character | "" | Path to `.bgi` file (BGEN index file). |
| `--sampleFile` | character | "" | Path to sample file for BGEN (one column with sample IDs). Required for BGEN files in version >= 0.38. |

#### PLINK Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--bedFile` | character | "" | Path to PLINK bed file containing binary genotype data. |
| `--bimFile` | character | "" | Path to PLINK bim file containing variant information. |
| `--famFile` | character | "" | Path to PLINK fam file containing sample information. |

#### Common Genotype Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--AlleleOrder` | character | "alt-first" | Allele order for BGEN or PLINK files: "alt-first" means effect allele is the alternate allele; "ref-first" means effect allele is the reference allele. |

### Variant Selection

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--idstoIncludeFile` | character | "" | Path to file containing variant IDs to include (one per line). Variant IDs must be in format `chr:pos_ref/alt`. No header. |
| `--rangestoIncludeFile` | character | "" | Path to file containing genomic regions to include (three columns: chromosome, start position, end position). No header. For VCF/BCF/SAV files, only the first line is used. |
| `--chrom` | character | "" | Chromosome to analyze. Required if `--LOCO=TRUE` or when using VCF/BCF/SAV input. String must exactly match chromosome naming in the input file (e.g., "1" vs "chr1"). |

### Variant Quality Filters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--minMAF` | numeric | 0 | Minimum minor allele frequency (MAF) for variants to test. The higher threshold between `--minMAF` and `--minMAC` is applied. |
| `--minMAC` | numeric | 0.5 | Minimum minor allele count (MAC) for variants to test. The higher threshold between `--minMAF` and `--minMAC` is applied. |
| `--minInfo` | numeric | 0 | Minimum imputation INFO score for variants to test. Only used if `--is_imputed_data=TRUE`. |
| `--maxMissing` | numeric | 0.15 | Maximum missing rate for variants to test (range 0-1). Variants with missing rate above this threshold are excluded. |
| `--is_imputed_data` | logical | FALSE | Whether genotypes are imputed. If TRUE, INFO scores will be calculated and output, and dosage zeroing filters apply. |
| `--impute_method` | character | "best_guess" | Method for imputing missing dosages: "best_guess" (round to nearest integer based on 2×allele frequency), "mean" (2×allele frequency), or "minor" (minor allele homozygote). |
| `--dosage_zerod_cutoff` | numeric | 0.2 | If `--is_imputed_data=TRUE`, dosages ≤ this value are set to 0 for variants with MAC ≤ `--dosage_zerod_MAC_cutoff`. Reduces noise from uncertain low-frequency genotypes. |
| `--dosage_zerod_MAC_cutoff` | numeric | 10 | MAC threshold for applying dosage zeroing (see `--dosage_zerod_cutoff`). |

### Step 1 Model Files

Specify either a single phenotype or multiple phenotypes:

#### Single Phenotype

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--GMMATmodelFile` | character | "" | Path to model file (`.rda`) output from Step 1. Required for single-phenotype analysis. |
| `--varianceRatioFile` | character | "" | Path to variance ratio file (`.varianceRatio.txt`) output from Step 1. Required for single-phenotype analysis. |

#### Multiple Phenotypes

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--GMMATmodel_varianceRatio_multiTraits_File` | character | "" | Path to file containing information for multiple phenotypes (3 columns: phenotype name, model file path, variance ratio file path). One line per phenotype. Allows testing multiple genes/phenotypes simultaneously across the same variants. |

### Single-Variant Association Test Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--LOCO` | logical | TRUE | Whether to apply leave-one-chromosome-out approach. If TRUE, `--chrom` is required. |
| `--SPAcutoff` | numeric | 2 | Deviation threshold (in standard deviations) for applying saddlepoint approximation (SPA). If test statistic is within this cutoff of the mean, standard score test p-value is returned. |
| `--is_Firth_beta` | logical | FALSE | Whether to use approximate Firth correction to estimate effect sizes (beta). Only for binary traits. More accurate for low-frequency variants. |
| `--pCutoffforFirth` | numeric | 0.01 | P-value threshold for applying Firth correction. Effect sizes are estimated using Firth for variants with p ≤ this threshold. Only used if `--is_Firth_beta=TRUE`. |

### Set-Based (Gene/Region) Test Options

Set-based tests aggregate evidence across multiple variants within a gene or region, increasing power for rare variants.

#### Group File

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--groupFile` | character | "" | Path to group file containing variant sets. Each set requires 2-3 lines: (1) set name and "var" followed by variant IDs (format: `chr:pos_ref/alt`), (2) "anno" followed by annotations, and (3, optional) "weight" followed by weights. Elements are tab or space separated. |

#### Variant Filters for Set Tests

These filters determine which variants within each set are included in different test masks:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--maxMAF_in_groupTest` | character | "0.0001,0.001,0.01" | Comma-separated maximum MAF values for creating different masks. Each value creates a separate test. |
| `--minMAF_in_groupTest_Exclude` | character | NULL | Comma-separated minimum MAF values for creating different masks (lower bound). Must have same length as `--maxMAF_in_groupTest`. |
| `--maxMAC_in_groupTest` | character | "0" | Comma-separated maximum MAC values for masks. Combined with `--maxMAF_in_groupTest`. Default "0" means no MAC filter. |
| `--minMAC_in_groupTest_Exclude` | character | NULL | Comma-separated minimum MAC values for masks (lower bound). Must have same length as `--maxMAC_in_groupTest`. |
| `--annotation_in_groupTest` | character | "lof,missense;lof,missense;lof;synonymous" | Comma-separated list of annotation combinations. Use semicolon (;) to combine multiple annotations in one test. Example: "lof" tests only loss-of-function variants, "lof;missense" tests both together. |
| `--MACCutoff_to_CollapseUltraRare` | numeric | 10 | MAC threshold for collapsing ultra-rare variants. Variants with MAC ≤ this value are collapsed into a single burden component. |
| `--minGroupMAC_in_BurdenTest` | numeric | 5 | Minimum MAC for the pseudo-marker in burden tests. Only applies when running burden-only tests (`--r.corr=1`). |

#### Test Types and Weights

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--r.corr` | numeric | 0 | Test type: 0 = SKAT-O (outputs Burden, SKAT, and SKAT-O results), 1 = Burden test only. |
| `--is_SKATO` | logical | FALSE | Whether to perform SKAT-O test (omnibus test combining Burden and SKAT). |
| `--weights.beta` | character | "1;25" | Parameters for Beta distribution used to weight variants: Beta(MAF, a, b). Format: "a;b". Multiple weight sets can be specified separated by commas (e.g., "1;25,1;1" for both MAF-based and equal weights). |
| `--is_equal_weight_in_groupTest` | logical | FALSE | Whether to use equal weights for all variants in set tests. If FALSE, weights are based on Beta distribution or extracted from group file. |

#### Set Test Output Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--is_single_in_groupTest` | logical | FALSE | Whether to output single-variant results when performing set tests. Primarily useful for burden-only tests (`--r.corr=1`). Single-variant results are always output for SKAT/SKAT-O tests. |
| `--is_output_markerList_in_groupTest` | logical | FALSE | Whether to output the list of markers included in each mask for each set. Useful for tracking which variants contribute to each test. |

#### Conditional Analysis in Set Tests

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--condition` | character | "" | Comma-separated list of variant IDs to condition on (format: `chr:pos_ref/alt`). Example: "chr3:101651171_C/T,chr3:101651186_G/A". |
| `--weights_for_condition` | character | NULL | Weights for conditioning variants in set tests. Format: semicolon-separated values per weight set, comma-separated for multiple weight sets (e.g., "1;2;3,4;5;6"). Length must match number of conditioning variants. If not specified, default Beta weights are used. |

#### Set Test Performance Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--groups_per_chunk` | numeric | 100 | Number of gene/region sets to read and test in each chunk. |
| `--markers_per_chunk_in_groupTest` | numeric | 100 | Number of markers per chunk when calculating variance-covariance matrix in set tests. |

### Sparse GRM Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--sparseGRMFile` | character | "" | Path to sparse GRM file that was used in Step 1. |
| `--sparseGRMSampleIDFile` | character | "" | Path to sample ID file for sparse GRM (no header, order matches sparse GRM). |
| `--relatednessCutoff` | numeric | 0 | Kinship coefficient threshold for treating samples as unrelated when using sparse GRM. |
| `--is_sparseGRM` | logical | FALSE | Whether to use sparse GRM in association tests. |

### Output Control

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--SAIGEOutputFile` | character | "" | **Required**. Path to output file containing association test results. |
| `--markers_per_chunk` | numeric | 10000 | Number of markers to test and output in each chunk for single-variant tests. Larger values use more memory but reduce I/O overhead. |
| `--is_output_moreDetails` | logical | FALSE | Whether to output additional details: homozygous and heterozygous counts in cases and controls (for binary traits). Adds columns: homN_Allele2_cases, hetN_Allele2_cases, homN_Allele2_ctrls, hetN_Allele2_ctrls. |
| `--is_overwrite_output` | logical | TRUE | Whether to overwrite existing output file. If FALSE, continues unfinished analysis instead of starting over. |

### Variance Ratio Categories

For analyses using categorical variance ratios (when Step 1 was run with `--isCateVarianceRatio=TRUE`):

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--cateVarRatioMinMACVecExclude` | character | "10,20.5" | Comma-separated lower bounds for MAC categories. Must match Step 1 settings. |
| `--cateVarRatioMaxMACVecInclude` | character | "20.5" | Comma-separated upper bounds for MAC categories. Must match Step 1 settings. |

### Advanced Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--is_fastTest` | logical | FALSE | Whether to use fast test mode with approximations for computational speed. |
| `--pval_cutoff_for_fastTest` | numeric | 0.05 | P-value cutoff for using sparse variance matrix in fast test mode. |
| `--is_noadjCov` | logical | TRUE | Whether to regress out covariates from genotypes. |
| `--max_MAC_for_ER` | numeric | 4 | Maximum MAC for using efficient resampling (ER) to calculate p-values. Variants with MAC ≤ this value use ER for more accurate p-values. |
| `--is_EmpSPA` | logical | FALSE | Whether to use empirical saddlepoint approximation. |

---

## step3_gene_pvalue_qtl.R

Calculates gene-level p-values from single-variant association results using the Aggregated Cauchy Association Test (ACAT). This is useful for summarizing evidence across all variants in a cis-window for each gene.

### Input Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--assocFile` | character | "" | **Required**. Path to association results file output by Step 2. Must contain columns "MarkerID" and "p.value". |
| `--weightFile` | character | "" | Path to optional weight file (two columns: "MarkerID" and "weight", with header). Allows differential weighting of variants in ACAT. If not specified, equal weights are used. |

### Gene Information

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--geneName` | character | "" | Gene name to include in output. If not specified, "gene" is used as default. |

### Output Control

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--genePval_outputFile` | character | "" | **Required**. Path to output file containing gene-level p-value. Output includes: gene name, ACAT p-value, top variant ID, and top variant p-value. |

---

## makeGroupFile.R

Creates group/set files for set-based association tests. Extracts variant information from genotype files within specified genomic regions and formats them for use with the `--groupFile` parameter in Step 2.

### Genotype Input Files

Specify one of the following genotype file formats:

#### VCF Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--vcfFile` | character | "" | Path to VCF file. |
| `--vcfFileIndex` | character | "" | Path to VCF index file (`.csi` format). |
| `--vcfField` | character | "DS" | Field to extract: "DS" for dosage or "GT" for genotype. |

#### SAV Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--savFile` | character | "" | Path to SAV file. |
| `--savFileIndex` | character | "" | Path to SAV index file (`.s1r`). |

#### BGEN Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--bgenFile` | character | "" | Path to BGEN file (version 1.2 with 8-bit compression). |
| `--bgenFileIndex` | character | "" | Path to BGEN index file (`.bgi`). |

#### PLINK Files

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--bedFile` | character | "" | Path to PLINK bed file. |
| `--bimFile` | character | "" | Path to PLINK bim file. |
| `--famFile` | character | "" | Path to PLINK fam file. |

### Region Specification

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--regionFile` | character | "" | **Required**. Path to file containing genomic regions (three columns: chromosome, start position, end position). No header. One region per line. |
| `--AlleleOrder` | character | "alt-first" | Allele order for BGEN or PLINK files: "alt-first" or "ref-first". |

### Output Control

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `--outputPrefix` | character | "~/" | **Required**. Path and prefix for output group file. The output file will contain variant IDs in the format required for Step 2 set-based tests. |

---

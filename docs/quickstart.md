---
layout: default
title: Quick Start
nav_order: 2.5
description: "Step-by-step guide to get started with SAIGE-QTL in minutes."
---

# Quick Start Guide
{: .no_toc }

Get started with SAIGE-QTL in 5 simple steps. Follow this guide to run your first eQTL analysis.

---

## Step 1: Choose Your Installation Method

Pick the installation method that best fits your environment:

<div style="display: grid; gap: 1rem; margin: 1rem 0;">

<details open markdown="block">
  <summary><strong>🐍 PIXI (Recommended for most users)</strong></summary>

**Best for**: Users without root access, no existing conda setup

Installation method 1: Pixi binary installation (Recommended, using prebuilt binaries)
```bash
# install pixi (if needed)
curl -fsSL https://pixi.sh/install.sh | bash
source ~/.bashrc  # Restart shell or reload environment
# Download SAIGE-QTL repository
git clone https://github.com/weizhou0/qtl.git
cd qtl
# Detect pre-built binary (choose one of the commands below to match your system)
## For Linux users (Linux x86_64):
BINARY_FILE=$(ls binaries/SAIGEQTL_*_linux-x86_64.tgz | head -n1)
## For MacOs users (arm64):
BINARY_FILE=$(ls binaries/SAIGEQTL_*_macos.tgz | head -n1)
# Install from pre-built binary
echo "Installing: $BINARY_FILE"
CONDA_OVERRIDE_GLIBC=2.28 pixi run R -e "
install.packages('${BINARY_FILE}', repos = NULL, type = 'source')
library(SAIGEQTL)
cat('✓ SAIGEQTL', as.character(packageVersion('SAIGEQTL')), 'installed successfully\n')
"
```

Installation method 2: Pixi source installation (using pixi to install from source code with all dependencies)
```bash
# Install Pixi and SAIGE-QTL
curl -fsSL https://pixi.sh/install.sh | sh
export PATH="$HOME/.pixi/bin:$PATH"

# Clone repository
git clone https://github.com/weizhou0/SAIGEQTL
cd SAIGEQTL

# Install dependencies
CONDA_OVERRIDE_GLIBC=2.28 pixi install --manifest-path=$PWD/pixi.toml
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=$PWD/pixi.toml \
    R CMD INSTALL .
```

**Your command prefix**:
```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=/path/to/SAIGEQTL/pixi.toml Rscript
```

📖 [Detailed PIXI Installation Guide](Installation_pixi.html)

</details>

<details markdown="block">
  <summary><strong>🐳 Docker (Best for HPC and reproducibility)</strong></summary>

**Best for**: HPC clusters, reproducible environments

```bash
# Pull pre-built image
docker pull wzhou88/saigeqtl:latest

# Test installation
docker run wzhou88/saigeqtl:latest step1_fitNULLGLMM_qtl.R --help
```

**Your command prefix**:
```bash
docker run -v /your/data:/data wzhou88/saigeqtl:latest
```

📖 [Detailed Docker Installation Guide](Installation_docker.html)

</details>

<details markdown="block">
  <summary><strong>🔬 Singularity (For HPC without Docker)</strong></summary>

**Best for**: HPC clusters without Docker access

```bash
# Pull and convert Docker image
singularity pull docker://wzhou88/saigeqtl:latest

# Test installation
singularity exec saigeqtl_latest.sif step1_fitNULLGLMM_qtl.R --help
```

**Your command prefix**:
```bash
singularity exec --bind /your/data:/data saigeqtl_latest.sif
```

📖 [Detailed Singularity Installation Guide](Installation_docker.html#singularity-installation-and-usage)

</details>

</div>

---

## Step 2: Prepare Your Data

You'll need three types of files:

### Required Files

| File Type | Format | Description | Example |
|-----------|--------|-------------|---------|
| **Phenotype** | Tab/space-delimited | Gene expression + covariates | `phenotypes.txt` |
| **Genotype** | PLINK/VCF/BGEN | Genetic variants | `genotypes.bed/bim/fam` |
| **Regions** (optional) | Tab-delimited | cis-regions for genes | `cis_regions.txt` |

### Example Phenotype File Format

```
individual_id  cell_id         gene_1  gene_2  age  sex  PC1    PC2
IND001        CELL_001        45      120     35   M    0.12   -0.05
IND001        CELL_002        52      98      35   M    0.12   -0.05
IND002        CELL_001        38      156     42   F    -0.08  0.15
```

**Key columns**:
- Individual ID (matches genotype file)
- Cell ID (for single-cell data)
- Gene expression values (one column per gene)
- Covariates (age, sex, PCs, batch effects, etc.)

### Example Region File (for cis-eQTL)

```
1    1000000    2000000
2     500000    1500000
```

Format: `chromosome  start  end` (no header)

---

## Step 3: Fit the Null Model (Step 1)

Run Step 1 for each gene you want to analyze.

### 📝 Basic Command Structure

<details open markdown="block">
  <summary><strong>Show me the command for my installation method</strong></summary>

#### For PIXI Users

```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=/path/to/SAIGEQTL/pixi.toml \
    Rscript /path/to/SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R \
    --phenoFile=phenotypes.txt \
    --phenoCol=gene_1 \
    --covarColList=age,sex,PC1,PC2 \
    --sampleCovarColList=age,sex,PC1,PC2 \
    --sampleIDColinphenoFile=individual_id \
    --cellIDColinphenoFile=cell_id \
    --traitType=count \
    --plinkFile=genotypes \
    --outputPrefix=output/gene_1
```

#### For Docker Users

```bash
docker run -v /data/myproject:/data wzhou88/saigeqtl:latest \
    step1_fitNULLGLMM_qtl.R \
    --phenoFile=/data/phenotypes.txt \
    --phenoCol=gene_1 \
    --covarColList=age,sex,PC1,PC2 \
    --sampleCovarColList=age,sex,PC1,PC2 \
    --sampleIDColinphenoFile=individual_id \
    --cellIDColinphenoFile=cell_id \
    --traitType=count \
    --plinkFile=/data/genotypes \
    --outputPrefix=/data/output/gene_1
```

#### For Singularity Users

```bash
singularity exec --bind /data:/data saigeqtl_latest.sif \
    step1_fitNULLGLMM_qtl.R \
    --phenoFile=/data/phenotypes.txt \
    --phenoCol=gene_1 \
    --covarColList=age,sex,PC1,PC2 \
    --sampleCovarColList=age,sex,PC1,PC2 \
    --sampleIDColinphenoFile=individual_id \
    --cellIDColinphenoFile=cell_id \
    --traitType=count \
    --plinkFile=/data/genotypes \
    --outputPrefix=/data/output/gene_1
```

</details>

### 🔑 Key Parameters Explained

| Parameter | What to put | Example |
|-----------|-------------|---------|
| `--phenoFile` | Your phenotype file path | `phenotypes.txt` |
| `--phenoCol` | Gene/phenotype column name | `gene_1` |
| `--covarColList` | All covariates (comma-separated) | `age,sex,PC1,PC2,batch` |
| `--sampleCovarColList` | Individual-level covariates | `age,sex,PC1,PC2` |
| `--sampleIDColinphenoFile` | Individual ID column name | `individual_id` |
| `--cellIDColinphenoFile` | Cell ID column name | `cell_id` |
| `--traitType` | Always use `count` for eQTL | `count` |
| `--plinkFile` | Genotype file prefix (no extension) | `genotypes` |
| `--outputPrefix` | Where to save results | `output/gene_1` |
| `--library` | Custom library path (if needed) | `/path/to/custom/library` |

> **📝 Note on `--library` parameter**: If you installed SAIGEQTL to a custom library location (e.g., using `R CMD INSTALL --library=custom/path`), use this parameter to specify the path. This avoids manually editing wrapper scripts. Not needed for standard installations.

### ✅ Step 1 Output

You'll get two files:
- `output/gene_1.rda` - Model file (needed for Step 2)
- `output/gene_1.varianceRatio.txt` - Variance ratio (needed for Step 2)

---

## Step 4: Choose Your Analysis Type

Pick the analysis that fits your research question:

<div style="display: grid; gap: 1rem; margin: 1rem 0;">

<details open markdown="block">
  <summary><strong>🎯 cis-eQTL Analysis (Most Common)</strong></summary>

**Use when**: Testing variants near genes (within ~1Mb)

**Advantages**:
- ✅ Higher statistical power
- ✅ Easier interpretation (local regulation)
- ✅ Faster computation

**Next**: Continue to [Step 5A - cis-eQTL Testing](#step-5a-run-cis-eqtl-tests-step-2)

</details>

<details markdown="block">
  <summary><strong>🌐 Genome-wide eQTL Analysis</strong></summary>

**Use when**: Testing all variants across the genome

**Advantages**:
- ✅ Discover trans-acting effects
- ✅ Comprehensive regulatory networks
- ✅ Identify distant regulators

**Next**: Continue to [Step 5B - Genome-wide Testing](#step-5b-run-genome-wide-tests-step-2)

</details>

</div>

---

## Step 5A: Run cis-eQTL Tests (Step 2)

Test genetic variants in the cis-region (near the gene).

### Create Region File

Create a file with your cis-window (e.g., gene location ± 1Mb):

```bash
# Example: gene on chr1, position 1,500,000
# cis-window: 500,000 - 2,500,000 (±1Mb)
echo -e "1\t500000\t2500000" > gene_1_cis_region.txt
```

### Run Step 2

<details open markdown="block">
  <summary><strong>Show me the command for my installation method</strong></summary>

#### For PIXI Users

```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=/path/to/SAIGEQTL/pixi.toml \
    Rscript /path/to/SAIGEQTL/extdata/step2_tests_qtl.R \
    --vcfFile=chr1.vcf.gz \
    --vcfFileIndex=chr1.vcf.gz.csi \
    --vcfField=DS \
    --chrom=1 \
    --rangestoIncludeFile=gene_1_cis_region.txt \
    --GMMATmodelFile=output/gene_1.rda \
    --varianceRatioFile=output/gene_1.varianceRatio.txt \
    --SAIGEOutputFile=output/gene_1_cis_results.txt \
    --minMAC=20 \
    --LOCO=FALSE
```

#### For Docker Users

```bash
docker run -v /data/myproject:/data wzhou88/saigeqtl:latest \
    step2_tests_qtl.R \
    --vcfFile=/data/chr1.vcf.gz \
    --vcfFileIndex=/data/chr1.vcf.gz.csi \
    --vcfField=DS \
    --chrom=1 \
    --rangestoIncludeFile=/data/gene_1_cis_region.txt \
    --GMMATmodelFile=/data/output/gene_1.rda \
    --varianceRatioFile=/data/output/gene_1.varianceRatio.txt \
    --SAIGEOutputFile=/data/output/gene_1_cis_results.txt \
    --minMAC=20 \
    --LOCO=FALSE
```

#### For Singularity Users

```bash
singularity exec --bind /data:/data saigeqtl_latest.sif \
    step2_tests_qtl.R \
    --vcfFile=/data/chr1.vcf.gz \
    --vcfFileIndex=/data/chr1.vcf.gz.csi \
    --vcfField=DS \
    --chrom=1 \
    --rangestoIncludeFile=/data/gene_1_cis_region.txt \
    --GMMATmodelFile=/data/output/gene_1.rda \
    --varianceRatioFile=/data/output/gene_1.varianceRatio.txt \
    --SAIGEOutputFile=/data/output/gene_1_cis_results.txt \
    --minMAC=20 \
    --LOCO=FALSE
```

</details>

**📊 Continue to [Step 6 - Calculate Gene P-value](#step-6-calculate-gene-level-p-value-step-3)**

---

## Step 5B: Run Genome-wide Tests (Step 2)

Test all genetic variants across the genome.

### Run Step 2

<details markdown="block">
  <summary><strong>Show me the command for my installation method</strong></summary>

#### For PIXI Users

```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=/path/to/SAIGEQTL/pixi.toml \
    Rscript /path/to/SAIGEQTL/extdata/step2_tests_qtl.R \
    --vcfFile=chr1.vcf.gz \
    --vcfFileIndex=chr1.vcf.gz.csi \
    --vcfField=DS \
    --chrom=1 \
    --GMMATmodelFile=output/gene_1.rda \
    --varianceRatioFile=output/gene_1.varianceRatio.txt \
    --SAIGEOutputFile=output/gene_1_chr1_results.txt \
    --minMAF=0.01 \
    --LOCO=TRUE
```

#### For Docker Users

```bash
docker run -v /data/myproject:/data wzhou88/saigeqtl:latest \
    step2_tests_qtl.R \
    --vcfFile=/data/chr1.vcf.gz \
    --vcfFileIndex=/data/chr1.vcf.gz.csi \
    --vcfField=DS \
    --chrom=1 \
    --GMMATmodelFile=/data/output/gene_1.rda \
    --varianceRatioFile=/data/output/gene_1.varianceRatio.txt \
    --SAIGEOutputFile=/data/output/gene_1_chr1_results.txt \
    --minMAF=0.01 \
    --LOCO=TRUE
```

#### For Singularity Users

```bash
singularity exec --bind /data:/data saigeqtl_latest.sif \
    step2_tests_qtl.R \
    --vcfFile=/data/chr1.vcf.gz \
    --vcfFileIndex=/data/chr1.vcf.gz.csi \
    --vcfField=DS \
    --chrom=1 \
    --GMMATmodelFile=/data/output/gene_1.rda \
    --varianceRatioFile=/data/output/gene_1.varianceRatio.txt \
    --SAIGEOutputFile=/data/output/gene_1_chr1_results.txt \
    --minMAF=0.01 \
    --LOCO=TRUE
```

</details>

**💡 Tip**: Repeat for each chromosome (chr1-chr22, chrX)

---

## Step 6: Calculate Gene-Level P-value (Step 3)

Combine results from all variants into a single gene-level p-value using ACAT.

### Run Step 3

<details open markdown="block">
  <summary><strong>Show me the command for my installation method</strong></summary>

#### For PIXI Users

```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=/path/to/SAIGEQTL/pixi.toml \
    Rscript /path/to/SAIGEQTL/extdata/step3_gene_pvalue_qtl.R \
    --assocFile=output/gene_1_cis_results.txt \
    --geneName=gene_1 \
    --genePval_outputFile=output/gene_1_gene_pvalue.txt
```

#### For Docker Users

```bash
docker run -v /data/myproject:/data wzhou88/saigeqtl:latest \
    step3_gene_pvalue_qtl.R \
    --assocFile=/data/output/gene_1_cis_results.txt \
    --geneName=gene_1 \
    --genePval_outputFile=/data/output/gene_1_gene_pvalue.txt
```

#### For Singularity Users

```bash
singularity exec --bind /data:/data saigeqtl_latest.sif \
    step3_gene_pvalue_qtl.R \
    --assocFile=/data/output/gene_1_cis_results.txt \
    --geneName=gene_1 \
    --genePval_outputFile=/data/output/gene_1_gene_pvalue.txt
```

</details>

### 💡 Using Custom Library Locations

For all wrapper scripts (`step1_fitNULLGLMM_qtl.R`, `step2_tests_qtl.R`, `step3_gene_pvalue_qtl.R`), you can now specify a custom library path using the `--library` parameter:

```bash
# Example with custom library location
step1_fitNULLGLMM_qtl.R --library=/path/to/custom/library [other_options]
step2_tests_qtl.R --library=/path/to/custom/library [other_options]  
step3_gene_pvalue_qtl.R --library=/path/to/custom/library [other_options]
```

This is especially useful when you've installed SAIGEQTL to a custom location and eliminates the need to manually edit wrapper scripts with `lib.loc` specifications.

### ✅ Final Output

Your final results file (`gene_1_gene_pvalue.txt`) contains:
- Gene name
- ACAT p-value (combined evidence from all variants)
- Top variant ID
- Top variant p-value

---

## 🎉 Congratulations!

You've completed your first SAIGE-QTL analysis!

### What's Next?

**For more genes**:
1. Repeat Step 3 (fit null model) for each gene
2. Run Step 2 and Step 3 for each gene
3. Consider parallelizing across genes for efficiency

**To learn more**:
- 📖 [Complete cis-eQTL Tutorial](cis-eQTL.html) - Detailed workflow with examples
- 📖 [Genome-wide eQTL Tutorial](genomewide-eQTL.html) - Large-scale analysis
- 📖 [Parameters & Options](parameters.html) - Complete parameter reference
- ❓ [FAQ](FAQ.html) - Common questions and troubleshooting

### Common Next Steps

**Analyze multiple genes**:
```bash
# Create a loop for multiple genes
for gene in gene_1 gene_2 gene_3; do
    # Run Step 1 for each gene
    # Run Step 2 for each gene
    # Run Step 3 for each gene
done
```

**Test rare variants**:
See [Set-based Tests Tutorial](cis-eQTL.html) for testing rare variants using SKAT-O

**Visualize results**:
- Plot Manhattan plots of p-values
- Create QQ plots to check for inflation
- Visualize top eQTLs with effect sizes

---

## 💡 Quick Tips

### For Single-Cell Data
- ✅ Include cell-level covariates (batch, UMI counts)
- ✅ Include individual-level covariates (age, sex, PCs)
- ✅ Use `--traitType=count` for read counts
- ✅ Consider using `--offsetCol` for log(total UMI)

### For Computational Efficiency
- ✅ Run Step 1 in parallel (one job per gene)
- ✅ Use `--LOCO=FALSE` for cis-eQTL to save time
- ✅ Use `--LOCO=TRUE` for genome-wide to avoid bias
- ✅ Filter variants: `--minMAC=20` for common variants

### Data Quality
- ✅ Remove low-quality variants (high missing rate)
- ✅ Filter genes with low expression
- ✅ Include appropriate covariates for your study design
- ✅ Check for population stratification

---

## ❓ Need Help?

**Common issues**:
- File path errors? Make sure to use absolute paths in Docker/Singularity
- Convergence errors? Check your covariates and sample size
- Memory errors? Reduce `--markers_per_chunk` parameter

**Get support**:
- 📖 Check the [FAQ](FAQ.html) for common problems
- 📧 Email: wzhou@broadinstitute.org
- 🐛 Report bugs: [GitHub Issues](https://github.com/weizhou0/SAIGEQTL/issues)

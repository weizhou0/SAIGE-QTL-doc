---
layout: default
title: PIXI
nav_order: 1
description: "This guide provides instructions for installing SAIGE-QTL using pixi package manager. Pixi is a standalone package manager that doesn't require conda or any existing package managers - it manages its own isolated environments and downloads packages directly from conda-forge and bioconda channels."
parent: Installation
---

###  Install SAIGE-QTL using pixi

#### Prerequisites
- Linux or macOS (64-bit)
- Internet connection for downloading packages
- Sufficient disk space (~2-3 GB for all dependencies)
- **No conda, mamba, or other package managers required**
- Git (for downloading the source code)


Note: These steps can be found in the [Dockerfile](https://github.com/weizhou0/qtl/blob/main/docker/Dockerfile).


### 0. Download the SAIGE-QTL package from github

```bash
src_branch=main
repo_src_url=https://github.com/weizhou0/SAIGEQTL
git clone -b $src_branch $repo_src_url

```

### 1. Install pixi and the R packages 
##### Option A: User-space Installation (No root required)

#### For Users Without Root Access (Recommended)
```bash
# Single command - installs in user directory
curl -fsSL https://pixi.sh/install.sh | sh && \
    export PATH="$HOME/.pixi/bin:$PATH" && \
    CONDA_OVERRIDE_GLIBC=2.28 pixi install --manifest-path=$PWD/SAIGEQTL/pixi.toml && \
    rm -rf ~/.cache/pixi && \
    CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=$PWD/SAIGEQTL/pixi.toml Rscript -e 'install.packages("lintools", repos="https://cloud.r-project.org")' && \
    CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=$PWD/SAIGEQTL/pixi.toml Rscript -e 'install.packages("remotes", repos="https://cloud.r-project.org"); remotes::install_github("barkasn/fastSave")'
```

##### Option B: System-wide Installation (Requires root - original approach)

#### For Root Users (System-wide Installation)
```bash
# Single command - Install pixi in user directory (recommended for most users)
curl -fsSL https://pixi.sh/install.sh | sh  && \
    export PATH="$HOME/.pixi/bin:$PATH" && \
    CONDA_OVERRIDE_GLIBC=2.28 pixi install --manifest-path=$PWD/SAIGEQTL/pixi.toml && \
    rm -rf ~/.cache/pixi  && \
    CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=$PWD/SAIGEQTL/pixi.toml Rscript -e 'install.packages("lintools", repos="https://cloud.r-project.org")'  && \
    CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=$PWD/SAIGEQTL/pixi.toml Rscript -e 'install.packages("remotes", repos="https://cloud.r-project.org"); remotes::install_github("barkasn/fastSave")' 
```


### 2. Install the SAIGE-QTL package

```bash

# Set the path to the SAIGEQTL/ source code

PATHTOSAIGEQTL=./

# Set the path to the file pixi.toml, which is in the SAIGEQTL folder 

PATHTOTOML=${PATHTOSAIGEQTL}SAIGEQTL/

```

#### Installation to the default R library paths
```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOTOML}pixi.toml R CMD INSTALL ${PATHTOSAIGEQTL}SAIGEQTL
```

When calling SAIGE-QTL in R, set the library location:
```r
library(SAIGEQTL)
```


#### Alternative installation paths
For custom library installation to the specified R library location, e.g. path_to_final_SAIGEQTL_library

```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOTOML}pixi.toml R CMD INSTALL ${PATHTOSAIGEQTL}SAIGEQTL --library=path_to_final_SAIGEQTL_library
```

When calling SAIGE-QTL in R, set the library location:

```r
library(SAIGEQTL, lib.loc=path_to_final_SAIGEQTL_library)
```

**Change the library(SAIGEQTL) to library(SAIGEQTL, lib.loc=path_to_final_SAIGEQTL_library)** in the wrapper files

```
${PATHTOSAIGEQTL}SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R
${PATHTOSAIGEQTL}SAIGEQTL/extdata/step2_tests_qtl.R
${PATHTOSAIGEQTL}SAIGEQTL/extdata/step3_gene_pvalue_qtl.R
${PATHTOSAIGEQTL}SAIGEQTL/extdata/makeGroupFile.R
```

## What Gets Installed

### SAIGE-QTL Source Code
- **Repository**: https://github.com/weizhou0/SAIGEQTL
- **Branch**: main (latest stable version)
- **Local directory**: `SAIGEQTL/` (created by git clone)

### Core Dependencies (from pixi.toml)
- **Statistical Computing**: R with numerical libraries (OpenBLAS, SuperLU)
- **R Packages**:
  - Statistical genetics: MetaSKAT, SKAT, SPAtest
  - Data manipulation: data.table, dplyr, dbplyr
  - Parallel computing: RcppParallel, furrr
  - Numerical computing: RcppArmadillo, RcppEigen, RcppNumerical
- **System Libraries**: boost-cpp, zlib, zstd, savvy

### Additional R Packages
- **lintools**: Linear programming and optimization tools
- **remotes**: For installing packages from GitHub
- **fastSave**: Fast saving/loading of R objects (from GitHub)

## Verification

After installation, verify the setup:

```bash

# Check if pixi environment is working
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOSAIGEQTL}SAIGEQTL/pixi.toml R --version

# Test SAIGE-QTL installation
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOSAIGEQTL}SAIGEQTL/pixi.toml Rscript -e 'library(SAIGEQTL); packageVersion("SAIGEQTL")'

#if not installed in the default folder for R libraries
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOSAIGEQTL}SAIGEQTL/pixi.toml Rscript -e 'library(SAIGEQTL, lib.loc=path_to_final_SAIGEQTL_library); packageVersion("SAIGEQTL")'

#For example
#CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOSAIGEQTL}SAIGEQTL/pixi.toml Rscript -e 'library(SAIGEQTL, lib.loc="/humgen/atgu1/fin/wzhou/projects/eQTL_method_dev/tool_dev/test_doc/install_test"); packageVersion("SAIGEQTL")'
##output: [1] '0.3.2.1'


# Check key dependencies
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOSAIGEQTL}SAIGEQTL/pixi.toml Rscript -e 'library(SKAT); library(MetaSKAT); library(data.table)'
```

---

## ✅ Next Steps

### Installation Complete!

You've successfully installed SAIGE-QTL with Pixi. Here's how to get started with your first analysis.

### Your Command Prefix

All SAIGE-QTL commands will use this prefix:

```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOTOML}pixi.toml
```

**💡 Tip**: Set an alias to save typing:
```bash
# Add to your ~/.bashrc or ~/.bash_profile
alias saige-pixi='CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=/path/to/SAIGEQTL/pixi.toml'

# Then you can run:
saige-pixi Rscript step1_fitNULLGLMM_qtl.R --help
```

### Quick Start Tutorial

Ready to run your first analysis? Follow this tutorial:

**[📖 cis-eQTL Analysis Tutorial](cis-eQTL.html)** - Learn how to map local genetic effects on gene expression

**What you'll learn:**
- How to prepare your data files
- Running Step 1: Fit the null model
- Running Step 2: Test genetic variants
- Running Step 3: Calculate gene-level p-values

**Example command for Pixi users:**
```bash
# Step 1 example
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOTOML}pixi.toml \
    Rscript ${PATHTOSAIGEQTL}SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R \
    --phenoFile=phenotypes.txt \
    --phenoCol=ENSG00000123456 \
    --traitType=count \
    --outputPrefix=output/gene1
```

### Additional Resources

- **[Parameters & Options](parameters.html)** - Complete reference for all command-line parameters
- **[Running Scripts Guide](calling-saigeqtl.html)** - Detailed information on running SAIGE-QTL scripts
- **[FAQ](FAQ.html)** - Common questions and troubleshooting

### Need Help?

If you encounter issues:
1. Check the [FAQ](FAQ.html) for common problems
2. Verify your installation using the verification commands above
3. Review the [cis-eQTL tutorial](cis-eQTL.html) for step-by-step guidance

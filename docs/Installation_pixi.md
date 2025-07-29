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


0. Download the SAIGE-QTL package from github

```bash
src_branch=main
repo_src_url=https://github.com/weizhou0/SAIGEQTL
git clone -b $src_branch $repo_src_url

```

1. Install pixi and the R packages 
##### Option A: User-space Installation (No root required)

#### For Users Without Root Access (Recommended)
```bash
# Single command - installs in user directory
curl -fsSL https://pixi.sh/install.sh | sh && \
    source ~/.bashrc && \
    pixi install && \
    rm -rf ~/.cache/pixi && \
    pixi run Rscript -e 'install.packages("lintools", repos="https://cloud.r-project.org")' && \
    pixi run Rscript -e 'install.packages("remotes", repos="https://cloud.r-project.org"); remotes::install_github("barkasn/fastSave")' && \
```

##### Option B: System-wide Installation (Requires root - original approach)

#### For Root Users (System-wide Installation)
```bash
# Single command - Install pixi in user directory (recommended for most users)
curl -fsSL https://pixi.sh/install.sh | sh  && \
    source ~/.bashrc   && \
    pixi install  && \
    rm -rf ~/.cache/pixi  && \
    pixi run Rscript -e 'install.packages("lintools", repos="https://cloud.r-project.org")'  && \
    pixi run Rscript -e 'install.packages("remotes", repos="https://cloud.r-project.org"); remotes::install_github("barkasn/fastSave")'  && \
```


### 2. Install the SAIGE-QTL package
#### Installation to the default R library paths
```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=./SAIGEQTL/pixi.toml R CMD INSTALL SAIGEQTL
```

When calling SAIGE-QTL in R, set the library location:
```r
library(SAIGEQTL)
```


#### Alternative installation paths
For custom library installation:
```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=./SAIGEQTL/pixi.toml R CMD INSTALL SAIGEQTL --library=path_to_final_SAIGEQTL_library
```

When calling SAIGE-QTL in R, set the library location:

```r
library(SAIGEQTL, lib.loc=path_to_final_SAIGEQTL_library)
```

**Change the library(SAIGEQTL) to library(SAIGEQTL, lib.loc=path_to_final_SAIGEQTL_library)** in the wrapper files

```
./SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R
./SAIGEQTL/extdata/step2_tests_qtl.R
./SAIGEQTL/extdata/step3_gene_pvalue_qtl.R
./SAIGEQTL/extdata/makeGroupFile.R
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
# Navigate to the SAIGEQTL directory if not already there
cd SAIGEQTL

# Check if pixi environment is working
pixi run R --version

# Test SAIGE-QTL installation
pixi run Rscript -e 'library(SAIGEQTL); packageVersion("SAIGEQTL")'

# Check key dependencies
pixi run Rscript -e 'library(SKAT); library(MetaSKAT); library(data.table)'
```





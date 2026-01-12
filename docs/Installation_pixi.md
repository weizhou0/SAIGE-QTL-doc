---
layout: default
title: Pixi Binary (Linux)
nav_order: 2
description: "Fastest installation method for Linux using pre-built binary packages - no compilation required."
parent: Installation
---

# Pixi Binary Installation Guide (🥈 Fastest for Linux)

## Overview

Pixi Binary installation provides the **fastest installation method for Linux users** with pre-built packages that require no compilation. This method is ideal for users who want a quick setup without dealing with compilers or build dependencies.

## Why Pixi Binary?

**✅ Advantages:**
- **Fastest installation** (no compilation required)
- **No compiler** or build tools needed
- **Avoids compilation errors** completely
- **Pre-built optimized** binaries
- **Consistent environment** management
- **Works on most modern Linux systems**

**⚠️ Requirements:**
- Linux x86_64 only
- R 4.4+ (managed by pixi)
- GLIBC 2.28+ (CentOS 7+, Ubuntu 18.04+, most modern systems)
- Pixi package manager

## Quick Installation

### One-Liner Installation
```bash
# Complete installation in one command
curl -fsSL https://pixi.sh/install.sh | bash && source ~/.bashrc && \
git clone https://github.com/weizhou0/SAIGEQTL.git && cd SAIGEQTL && \
BINARY_FILE=$(ls binaries/SAIGEQTL_*_linux-x86_64.tgz | head -n1) && \
CONDA_OVERRIDE_GLIBC=2.28 pixi run R -e "install.packages('${BINARY_FILE}', repos=NULL, type='source'); library(SAIGEQTL)"
```

### Step-by-Step Installation

#### 1. Install pixi (if needed)
```bash
curl -fsSL https://pixi.sh/install.sh | bash
source ~/.bashrc  # Restart shell or reload environment
```

#### 2. Download SAIGE-QTL repository
```bash
git clone https://github.com/weizhou0/SAIGEQTL.git
cd SAIGEQTL
```

#### 3. Install from pre-built binary
```bash
# Auto-detect latest binary version
BINARY_FILE=$(ls binaries/SAIGEQTL_*_linux-x86_64.tgz | head -n1)
echo "Installing: $BINARY_FILE"

# Install binary package
CONDA_OVERRIDE_GLIBC=2.28 pixi run R -e "
install.packages('${BINARY_FILE}', repos = NULL, type = 'source')
library(SAIGEQTL)
cat('✓ SAIGEQTL', as.character(packageVersion('SAIGEQTL')), 'installed successfully\n')
"
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

**No manual wrapper file editing required!** The wrapper scripts now support the `--library` parameter:

```bash
# Use the --library parameter to specify custom library location
step1_fitNULLGLMM_qtl.R --library=path_to_final_SAIGEQTL_library [other_options]
step2_tests_qtl.R --library=path_to_final_SAIGEQTL_library [other_options]
step3_gene_pvalue_qtl.R --library=path_to_final_SAIGEQTL_library [other_options]
makeGroupFile.R --library=path_to_final_SAIGEQTL_library [other_options]
```

**✅ Simplified Workflow:** No need to manually edit wrapper files - just use the `--library` parameter!

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

# Test SAIGE-QTL installation (default library location)
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOSAIGEQTL}SAIGEQTL/pixi.toml Rscript -e 'library(SAIGEQTL); packageVersion("SAIGEQTL")'

# For custom library installations, use the --library parameter with wrapper scripts:
step1_fitNULLGLMM_qtl.R --library=path_to_final_SAIGEQTL_library --help

# Or test in R session (if you need to use lib.loc for direct R commands):
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=${PATHTOSAIGEQTL}SAIGEQTL/pixi.toml Rscript -e 'library(SAIGEQTL, lib.loc=path_to_final_SAIGEQTL_library); packageVersion("SAIGEQTL")'
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

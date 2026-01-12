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
git clone https://github.com/weizhou0/qtl.git && cd qtl && \
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
git clone https://github.com/weizhou0/qtl.git
cd qtl
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

## What Gets Installed

### Pre-built Binary Package
- **SAIGEQTL Binary**: Pre-compiled Linux x86_64 package (`.tgz` format)
- **Location**: `binaries/SAIGEQTL_*_linux-x86_64.tgz` in the repository
- **No compilation**: Ready-to-install binary eliminates build time and dependencies

### Managed R Environment
- **R Version**: 4.4+ with optimized BLAS/LAPACK libraries
- **Core Dependencies**: Automatically managed by pixi environment
- **Cross-platform Compatibility**: Consistent behavior across Linux distributions

### Built-in Dependencies
The binary package includes all required dependencies:
- **Statistical genetics**: MetaSKAT, SKAT, SPAtest
- **Data manipulation**: data.table, dplyr, dbplyr
- **Parallel computing**: RcppParallel, furrr
- **Numerical computing**: RcppArmadillo, RcppEigen, RcppNumerical
- **System libraries**: boost-cpp, zlib, zstd, savvy

### Repository Access
- **Repository**: https://github.com/weizhou0/qtl
- **Binary Location**: `binaries/` directory in the repository
- **Version**: Latest stable release (automatically detected)

## Verification

After installation, verify the setup:

```bash
# Check if pixi environment is working
CONDA_OVERRIDE_GLIBC=2.28 pixi run R --version

# Test SAIGEQTL installation
CONDA_OVERRIDE_GLIBC=2.28 pixi run R -e 'library(SAIGEQTL); packageVersion("SAIGEQTL")'
# Expected output: [1] '0.3.4' (or latest version)

# Test core functionality
CONDA_OVERRIDE_GLIBC=2.28 pixi run R -e 'library(SAIGEQTL); exists("fitNULLGLMM_multiV")'
# Expected output: [1] TRUE

# Check executable scripts are available
ls qtl/extdata/step*.R
# Expected output: step1_fitNULLGLMM_qtl.R step2_tests_qtl.R step3_gene_pvalue_qtl.R

# Test help information
CONDA_OVERRIDE_GLIBC=2.28 pixi run Rscript qtl/extdata/step1_fitNULLGLMM_qtl.R --help
```

---

## ✅ Next Steps

### Installation Complete!

You've successfully installed SAIGEQTL with Pixi Binary method. The package is now ready to use with the fastest possible setup.

### Your Command Prefix

All SAIGEQTL commands will use this prefix:

```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run
```

**💡 Tip**: Set an alias to save typing:
```bash
# Add to your ~/.bashrc or ~/.bash_profile
alias saige-pixi='CONDA_OVERRIDE_GLIBC=2.28 pixi run'

# Then you can run:
saige-pixi Rscript qtl/extdata/step1_fitNULLGLMM_qtl.R --help
```

### Quick Start Tutorial

Ready to run your first analysis? Follow this tutorial:

**[📖 cis-eQTL Analysis Tutorial](cis-eQTL.html)** - Learn how to map local genetic effects on gene expression

**What you'll learn:**
- How to prepare your data files
- Running Step 1: Fit the null model
- Running Step 2: Test genetic variants
- Running Step 3: Calculate gene-level p-values

**Example command for Pixi Binary users:**
```bash
# Step 1 example
CONDA_OVERRIDE_GLIBC=2.28 pixi run \
    Rscript qtl/extdata/step1_fitNULLGLMM_qtl.R \
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

---
layout: default
title: Advanced Source Installation
nav_order: 1
description: "Manual source code installation methods requiring system configuration."
parent: Source Code Installation
---

# Advanced Source Installation Methods

## Overview

These installation methods are for **advanced users and developers** who need to install SAIGEQTL without using Pixi. They require manual system configuration and dependency management.

**⚠️ Note**: These methods require you to configure your system with the necessary compilers and libraries. If you prefer an easier approach, we strongly recommend the **[Pixi Source Installation](Installation_sourcecode.html)** which handles all dependencies automatically.

## Why Use These Methods?

**✅ Advantages:**
- **Customizable builds** and configurations
- **Integration with existing R** environment

**⚠️ Disadvantages:**
- **Requires C++ compiler** setup on your system
- **Can fail due to missing** system libraries
- **No pre-built package**
- **Dependency version conflicts** possible
- **Slower installation** (compilation time)
- **Complex troubleshooting** on older systems

## System Requirements

- **R** ≥ 3.5.0 (≥ 4.0 recommended)
- **C/C++ compiler** (gcc/clang with C++11 support)
- **BLAS/LAPACK libraries** (automatically detected by R)
- **OpenMP** (optional, for parallel processing)
- **Git** (for downloading source code)
- **GNU make/cmake**

## System Libraries (should be installed before R package requirements)

- **zlib & zstd** (Compression & Zstandard compression)
- **SuperLu** (Sparse linear solver)
- **Boost C++ libraries** (String algorithms)

## Dependencies
These dependencies should be installed in following guide (good to check if they exist if running in to installation issues)
#### R Package Dependencies
- **Rcpp** (≥ 1.0.7)
- **RcppArmadillo** (≥ 0.10.7.5)
- **RcppParallel**, **RcppEigen**, **RcppNumerical**, **Matrix**, **data.table**, **furrr**, **dbplyr**, **BH**
#### Additional LinkingTo Dependencies
- **SPAtest** (3.1.2)
- **SKAT**, **RhpcBLASctl**, **RSQLite**, **dplyr**, **nlme**, **MASS**, **optparse**
#### GitHub-hosted Dependencies (can be installed via remotes)
- **cysouw/qlcMatrix**, **leeshawn/MetaSKAT**, **barkasn/fastSave**


### Platform-Specific Compiler Setup

#### Linux (Ubuntu/Debian)
```bash
sudo apt update && sudo apt install build-essential r-base-dev
```

#### Linux (CentOS/RHEL)
```bash
sudo yum groupinstall "Development Tools"
sudo yum install R-devel
```

#### macOS
```bash
# Install Xcode command line tools
xcode-select --install
```

---

## Installation Methods

### Method 1: R remotes
```r
# Install using remotes package
if (!requireNamespace("remotes", quietly = TRUE)) {
  install.packages("remotes")
}
remotes::install_github("weizhou0/SAIGEQTL", dependencies = TRUE)

# Test installation
library(SAIGEQTL)
packageVersion("SAIGEQTL")
```

### Method 2: Manual Git Clone + Install
```bash
# 1. Download source code
git clone https://github.com/weizhou0/SAIGEQTL.git
cd SAIGEQTL

# 2. Install dependencies (if needed)
Rscript -e 'install.packages(c("Rcpp", "RcppArmadillo", "Matrix", "data.table", "optparse"))'

# 3. Install SAIGEQTL package
R CMD INSTALL .

# 4. Test installation
R -e 'library(SAIGEQTL); packageVersion("SAIGEQTL")'
```

### Method 3: Custom Library Path
```bash
# Install to specific directory
mkdir -p ~/R-packages
R CMD INSTALL --library=~/R-packages qtl/

# Test with custom library
R -e 'library(SAIGEQTL, lib.loc="~/R-packages"); packageVersion("SAIGEQTL")'
```

### Method 4: Development Installation
```bash
# For contributors and developers
git clone https://github.com/weizhou0/SAIGEQTL.git
cd SAIGEQTL
Rscript scripts/install_standard.R --dev
```

---

## Usage

### Standard Source Installation
```r
library(SAIGEQTL)
```

### Custom Library Usage

**🆕 Recommended**: Use the `--library` parameter with wrapper scripts:
```bash
step1_fitNULLGLMM_qtl.R --library=/path/to/custom/library [other_options]
```

**Alternative**: Direct R usage with lib.loc:
```r
library(SAIGEQTL, lib.loc="/path/to/custom/library")
```

### Command-Line Scripts
After installation, executable scripts are available in:

```bash
/path/to/SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R
/path/to/SAIGEQTL/extdata/step2_tests_qtl.R
/path/to/SAIGEQTL/extdata/step3_gene_pvalue_qtl.R
/path/to/SAIGEQTL/extdata/makeGroupFile.R
```

**💡 Tip**: Add to your PATH for convenience:
```bash
# Add to ~/.bashrc or ~/.bash_profile
export PATH="/path/to/SAIGEQTL/extdata:$PATH"

# Then you can run:
step1_fitNULLGLMM_qtl.R --help
```

---

## ✅ Next Steps

### Installation Complete!

You've successfully built SAIGEQTL from source. Here's how to get started with your first analysis.

### Your Command Prefix

```bash
# Scripts located in: /path/to/SAIGEQTL/extdata/
Rscript /path/to/SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R [options]
```

### Quick Start Tutorial

Ready to run your first analysis? Follow this tutorial:

**[📖 cis-eQTL Analysis Tutorial](cis-eQTL.html)** - Learn how to map local genetic effects on gene expression

**What you'll learn:**
- How to prepare your data files
- Running Step 1: Fit the null model
- Running Step 2: Test genetic variants
- Running Step 3: Calculate gene-level p-values

**Example command:**

```bash
Rscript /path/to/SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R \
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
2. Verify R can find the SAIGEQTL library
3. Review the [cis-eQTL tutorial](cis-eQTL.html) for step-by-step guidance

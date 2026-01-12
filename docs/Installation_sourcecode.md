---
layout: default
title: Source Code Installation
nav_order: 4
description: "Advanced installation from source code for developers and custom builds."
parent: Installation
---

# Source Code Installation Guide (🔧 Advanced)

## Overview

Source code installation is intended for **developers, contributors, or users needing the latest development features**. This method requires manual dependency management and compilation.

## Why Source Installation?

**✅ Advantages:**
- Access to **latest development features**
- **Customizable builds** and configurations
- **Integration with existing R** environment
- **No pre-built package** limitations
- **Development contribution** capability

**⚠️ Disadvantages:**
- **Requires C++ compiler** setup on your system
- **Can fail due to missing** system libraries
- **Dependency version conflicts** possible
- **Slower installation** (compilation time)
- **Complex troubleshooting** on older systems

## System Requirements

- **R** ≥ 3.5.0 (≥ 4.0 recommended)
- **C/C++ compiler** (gcc/clang with C++11 support)
- **BLAS/LAPACK libraries** (automatically detected by R)
- **OpenMP** (optional, for parallel processing)
- **Git** (for downloading source code)

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

## Installation Methods

### Method 1: R remotes (Recommended)
```r
# Install using remotes package
if (!requireNamespace("remotes", quietly = TRUE)) {
  install.packages("remotes")
}
remotes::install_github("weizhou0/qtl", dependencies = TRUE)

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

## Usage

### Standard Usage (Default Library)
```r
library(SAIGEQTL)
```

### Custom Library Usage

**🆕 Recommended**: Use the `--library` parameter with wrapper scripts:
```bash
# Wrapper scripts now support --library parameter
step1_fitNULLGLMM_qtl.R --library=/path/to/custom/library [other_options]
step2_tests_qtl.R --library=/path/to/custom/library [other_options]
step3_gene_pvalue_qtl.R --library=/path/to/custom/library [other_options]
makeGroupFile.R --library=/path/to/custom/library [other_options]
```

**Alternative**: Direct R usage with lib.loc:
```r
library(SAIGEQTL, lib.loc="/path/to/custom/library")
```

### Command-Line Scripts
After installation, executable scripts are available in:
```bash
# Scripts location (adjust path as needed)
/path/to/qtl/extdata/step1_fitNULLGLMM_qtl.R
/path/to/qtl/extdata/step2_tests_qtl.R  
/path/to/qtl/extdata/step3_gene_pvalue_qtl.R
/path/to/qtl/extdata/makeGroupFile.R
```

**💡 Tip**: Add to PATH for convenience:
```bash
export PATH="/path/to/qtl/extdata:$PATH"
```

---

## ✅ Next Steps

### Installation Complete!

You've successfully built SAIGE-QTL from source. Here's how to get started with your first analysis.

### Your Command Prefix

Your scripts are located in:
```bash
/path/to/SAIGEQTL/extdata/
```

Run commands like:
```bash
Rscript /path/to/SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R [options]
```

**💡 Tip**: Add to PATH for convenience:
```bash
# Add to your ~/.bashrc or ~/.bash_profile
export PATH="/path/to/SAIGEQTL/extdata:$PATH"

# Then you can run:
step1_fitNULLGLMM_qtl.R --help
```

### Quick Start Tutorial

Ready to run your first analysis? Follow this tutorial:

**[📖 cis-eQTL Analysis Tutorial](cis-eQTL.html)** - Learn how to map local genetic effects on gene expression

**What you'll learn:**
- How to prepare your data files
- Running Step 1: Fit the null model
- Running Step 2: Test genetic variants
- Running Step 3: Calculate gene-level p-values

**Example command for Source installation users:**
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

---
layout: default
title: Conda/Bioconda
nav_order: 3
description: "Cross-platform installation using conda package manager with automatic dependency management."
parent: Installation
---

# Conda/Bioconda Installation Guide (🥉 Cross-Platform)

## Overview

Conda provides a **cross-platform installation method** for SAIGE-QTL with automatic dependency management. This method works well for users who already have conda environments or prefer traditional package managers.

## Why Conda?

**✅ Advantages:**
- **Automatic dependency management**
- **No compilation required**
- **Works on any system** with conda (Linux/macOS/Windows)
- **Cross-platform compatibility**
- **Isolated environment management**
- **Fast installation**

**⚠️ Requirements:**
- Conda or Mamba package manager
- No R installation required (conda provides R)

## Installation Options

### Option 1: Direct Installation (Recommended)
```bash
# Install directly with conda
conda install -c aryarm r-saigeqtl
```

### Option 2: Create New Environment
```bash
# Create new isolated environment with SAIGEQTL
conda create -n saigeqtl -c aryarm r-saigeqtl
conda activate saigeqtl
```

### Option 3: Install Miniconda First (If needed)
```bash
# If you don't have conda, install miniconda first
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh -b
~/miniconda3/bin/conda install -c aryarm r-saigeqtl
```

## Usage

### Activate Environment (if using Option 2)
```bash
conda activate saigeqtl
```

### Test Installation
```bash
# Use R with SAIGEQTL
R -e "library(SAIGEQTL); packageVersion('SAIGEQTL')"

# Use command-line tools
step1_fitNULLGLMM_qtl.R --help
step2_tests_qtl.R --help
step3_gene_pvalue_qtl.R --help
```

## Platform-Specific Notes

### Linux
- Full support with all features
- Pre-built packages available
- Fastest conda installation

### macOS  
- Good support for both Intel and Apple Silicon
- Automatic dependency resolution
- May require compilation for some dependencies

### Windows
- Limited support - use WSL2 + conda for best results
- Docker recommended for native Windows

## Troubleshooting

### Common Issues

1. **Channel Priority**
   ```bash
   # Ensure correct channel priority
   conda config --add channels conda-forge
   conda config --add channels bioconda  
   conda config --add channels aryarm
   ```

2. **Environment Conflicts**
   ```bash
   # Create clean environment
   conda create -n saigeqtl-clean -c aryarm r-saigeqtl
   ```

3. **Package Not Found**
   ```bash
   # Update conda and try again
   conda update conda
   conda install -c aryarm r-saigeqtl
   ```


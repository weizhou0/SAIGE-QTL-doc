---
layout: default
title: Source Code Installation
nav_order: 4
description: "Source code installation using Pixi for developers and custom builds."
parent: Installation
has_children: true
---

# Source Code Installation Guide (🔧 Advanced)

## Overview

Source code installation is intended for **developers, contributors, or users needing the latest development features**.

We recommend the **Pixi Source Installation** method below, which handles all dependencies automatically. For other manual installation methods that require complex system configuration and troubleshooting, see **[Advanced Source Installation](Installation_sourcecode_advanced.html)**.

## Pixi Source Installation (Recommended)

```bash
# Full environment management with pixi - cross-platform
# install pixi
curl -fsSL https://pixi.sh/install.sh | bash
# Restart shell or reload environment
export PATH="$HOME/.pixi/bin:$PATH"
# Download SAIGEQTL package from GitHub
git clone https://github.com/weizhou0/SAIGEQTL.git && cd SAIGEQTL
pixi run install-standard

# Commands use manifest path:
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml Rscript extdata/step1_fitNULLGLMM_qtl.R --help
```

**Why Pixi Source recommended?**
- ✅ **Managed environment**: All dependencies handled automatically
- ✅ **Cross-platform**: Works on Linux and macOS
- ✅ **Reproducible**: Consistent across systems
- ✅ **Development-ready**: Full toolchain included
- ✅ **No system configuration required**: Unlike other source methods

## Usage

### Command Prefix
```bash
# Command prefix for all Pixi source installations
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml

# Example commands:
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml R -e 'library(SAIGEQTL); packageVersion("SAIGEQTL")'
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml Rscript extdata/step1_fitNULLGLMM_qtl.R --help
```

### Command-Line Scripts
After installation, executable scripts are available:

```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml Rscript extdata/step1_fitNULLGLMM_qtl.R
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml Rscript extdata/step2_tests_qtl.R
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml Rscript extdata/step3_gene_pvalue_qtl.R
```

**💡 Tip**: Add an alias for convenience:
```bash
# For Pixi source installation
alias saige-pixi='CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml'
# Then: saige-pixi SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R --help
```

### Custom Library Usage

Use the `--library` parameter with wrapper scripts:
```bash
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml Rscript SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R --library=/path/to/custom/library [other_options]
```

---

## ✅ Next Steps

### Installation Complete!

You've successfully built SAIGEQTL from source using Pixi. Here's how to get started with your first analysis.

### Your Command Prefix

```bash
# Use pixi environment
CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml Rscript SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R [options]
```

**💡 Tip**: Set up a convenient alias:
```bash
# For Pixi source installation - add to ~/.bashrc or ~/.zshrc
alias saige-pixi='CONDA_OVERRIDE_GLIBC=2.28 pixi run --manifest-path=pixi.toml'

# Then you can run:
saige-pixi Rscript extdata/step1_fitNULLGLMM_qtl.R --help
```

### Quick Start Tutorial

Ready to run your first analysis? Follow this tutorial:

**[📖 cis-eQTL Analysis Tutorial](cis-eQTL.html)** - Learn how to map local genetic effects on gene expression

**What you'll learn:**
- How to prepare your data files
- Running Step 1: Fit the null model
- Running Step 2: Test genetic variants
- Running Step 3: Calculate gene-level p-values

### Additional Resources

- **[Parameters & Options](parameters.html)** - Complete reference for all command-line parameters
- **[Running Scripts Guide](calling-saigeqtl.html)** - Detailed information on running SAIGE-QTL scripts
- **[FAQ](FAQ.html)** - Common questions and troubleshooting

### Need Help?

If you encounter issues:
1. Check the [FAQ](FAQ.html) for common problems
2. Verify R can find the SAIGEQTL library
3. Review the [cis-eQTL tutorial](cis-eQTL.html) for step-by-step guidance

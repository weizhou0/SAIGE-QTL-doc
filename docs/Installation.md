---
layout: default
title: Installation
nav_order: 3
description: "Step-by-step installation instructions for SAIGE-QTL, with multiple installation methods to suit different environments and use cases."
has_children: true
has_toc: false
---

# Installing SAIGE-QTL

Choose the installation method that best fits your environment and requirements. We recommend **Pixi** for most users as it provides the most reliable and reproducible installation experience.

## Current Version: 0.3.2
*Updated: July 28, 2025*

## Installation Methods

### 🚀 **Recommended: Pixi Installation**
The fastest and most reliable method with automatic dependency management.

**[→ Install with Pixi](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_pixi.html)**

### 🐳 **Docker Installation**
Pre-built container for immediate use without local installation.

**[→ Install with Docker](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_docker.html)**

### 📦 **Conda/Bioconda Installation**
Traditional package manager for Python/R environments.

**[→ Install with Conda](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_bioconda.html)**

### 🔧 **Source Code Installation**
For advanced users who need custom builds or development versions.

**[→ Install from Source](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_sourcecode.html)**

## Quick Start Comparison

| Method | Best For | Setup Time | Dependencies |
|--------|----------|------------|--------------|
| **Pixi** | Most users, reproducible environments | ~5 minutes | Automatic |
| **Docker** | Containers, HPC clusters | ~2 minutes | Docker only |
| **Conda** | Existing conda environments | ~10 minutes | Manual |
| **Source** | Developers, custom builds | ~30 minutes | Manual |

## System Requirements

### Minimum Requirements
- **OS**: Linux, macOS, or Windows (WSL recommended)
- **RAM**: 8GB minimum, 16GB+ recommended for large datasets
- **Storage**: 2GB for software, additional space for data
- **R**: Version 4.0+ (installed automatically with Pixi/Docker)

### Recommended Configuration
- **RAM**: 32GB+ for large-scale analyses
- **CPU**: Multi-core processor for parallel processing
- **Storage**: SSD storage for improved I/O performance

## What's New in Version 0.3.2

### ✨ **New Features**
- **`--offsetCol` parameter**: Use log of total read counts per cell as an offset in the model
- **Enhanced Pixi support**: Improved installation process and environment management
- **Better error handling**: More informative error messages and debugging information

### 🐛 **Bug Fixes**
- Resolved memory issues with large datasets
- Fixed compatibility issues with newer R versions
- Improved handling of edge cases in variance estimation

### 📋 **Complete Changelog**
For detailed logs of all bug fixes and improvements, see the [Installation Logs](Installation.html#changelog).

## Post-Installation

### Verify Installation
After installation, verify SAIGE-QTL is working correctly:

1. **Check help information**:
   ```bash
   # Replace with your installation method's command prefix
   Rscript step1_fitNULLGLMM_qtl.R --help
   ```

2. **Run example analysis**: Follow the [Step 1 tutorial](https://weizhou0.github.io/SAIGE-QTL-doc/docs/step1.html) with provided example data

### Next Steps
1. **[Review the workflow overview](https://weizhou0.github.io/SAIGE-QTL-doc/docs/overview.html)** - Understand the analysis pipeline
2. **[Learn how to call SAIGE-QTL](https://weizhou0.github.io/SAIGE-QTL-doc/docs/calling-saigeqtl.html)** - Execute scripts in different environments
3. **[Start with Step 1](https://weizhou0.github.io/SAIGE-QTL-doc/docs/step1.html)** - Begin your first analysis

## Getting Help

### Installation Issues
- Check the specific installation guide for your chosen method
- Verify system requirements are met
- Review error messages carefully - they often contain helpful information

### Technical Support
- **Email**: [wzhou@broadinstitute.org](mailto:wzhou@broadinstitute.org)
- **GitHub Issues**: For bug reports and feature requests
- **Documentation**: Browse all available guides in the navigation menu

---

## Changelog

### Version 0.3.2 (July 28, 2025)
- **New**: Added `--offsetCol` option for using log of total read counts per cell as an offset in the model
- **Improved**: Enhanced installation process using pixi
- **Fixed**: Various bug fixes and stability improvements 


---
layout: default
title: Installation
nav_order: 3
description: "Step-by-step installation instructions for SAIGE-QTL, with multiple installation methods to suit different environments and use cases."
has_children: true
has_toc: false
---

# Installing SAIGE-QTL

Choose the installation method that best fits your environment and requirements. We recommend **Docker** for most users as it works on all platforms with zero setup, followed by **Pixi Binary** for Linux and Mac users seeking the fastest installation.

## Current Version: 0.3.5
*Updated: February 2, 2025*

## Installation Methods (Ordered by Ease of Use)

### 🥇 **Easiest: Docker (Recommended)**
Zero setup, works on Linux, macOS, Windows - everything included in the container.

**[→ Install with Docker](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_docker.html)**

### 🥈 **Easy: Pixi Binary **
Fastest installation for Linux and Mac - no compilation required, pre-built packages.

**[→ Install with Pixi Binary](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_pixi.html)**

### 🔧 **Advanced: Source Code**
Requires specific system configurations, unless using the pixi source-code installation approach outlined in this page.

**[→ Install from Source](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_sourcecode.html)**

## Platform Support & Quick Start

| Platform | Support Level | Recommended Methods | Notes |
|----------|---------------|-------------------|--------|
| **Linux** | ✅ **Full Support** | Docker → Pixi Binary → Source | Pre-built binaries available |
| **macOS** | ✅ **Good Support** | Docker → Pixi Binary → Source | No pre-built binaries |
| **Windows** | ⚠️ **Limited Support** | Docker only, or WSL2 + Linux methods | Not natively supported |

## Installation Comparison

| Method | Platform | Setup Time | Dependencies | Best For |
|--------|----------|------------|--------------|----------|
| **Docker** | All platforms | ~2 minutes | Docker only | Zero setup, containers, HPC |
| **Pixi Binary** | Linux, macOS | ~5 minutes | Pixi + GLIBC 2.28+ | Fastest installation from pre-built binaries |
| **Pixi Source** | Linux, macOS | ~15 minutes | Pixi only | Managed dev environment |
| **Source** | Linux, macOS | ~30 minutes | C++ compiler + libraries | Traditional development, requires specific system configuration |

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

### Version 0.3.5 (February 2, 2025)
- **New**: Added `--solverMethod` option allowing step 1 null modeling fitting using either Sherman-Morrison-Woodbury approach ("smw" option) or the original preconditioned conjugate gradient approach ("pcg" option). For data sets with unrelated donors, for which the sparse GRM becomes the identity matrix, SMW further reduces to closed-form block-wise operations with optimal complexity. The default is to automatically detect given phenotype file, if multiple rows of cell data for any individual is provided, it will automatically use the SMW approach.
- **Performance**: Optimized matrix inversion algorithm (SMW) for faster and user-specified model fitting
- **Improved**: Enhanced Docker support with updated documentation and cross-platform compatibility
- **Fixed**: Various stability and performance improvements

### Version 0.3.4 (January 11, 2025)
- **New**: Comprehensive installation documentation with platform-specific guidance and troubleshooting
- **New**: Binary installation method for faster Linux setup via Pixi
- **Performance**: Optimized matrix inversion algorithms for faster model fitting
- **Performance**: Improved memory efficiency for large-scale analyses
- **Improved**: Enhanced Docker support with updated documentation and cross-platform compatibility
- **Fixed**: Various stability and performance improvements

### Version 0.3.2 (July 28, 2025)
- **New**: Added `--offsetCol` option for using log of total read counts per cell as an offset in the model
- **Improved**: Enhanced installation process using pixi
- **Fixed**: Various bug fixes and stability improvements

**July 31, 2025:**
- Export extdata/ in the docker container so users do not need to download the git repo when using the docker image wzhou88/saigeqtl:latest on example data 


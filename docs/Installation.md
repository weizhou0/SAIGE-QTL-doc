---
layout: default
title: Installation
nav_order: 3
description: "Step-by-step installation instructions for SAIGE-QTL, with multiple installation methods to suit different environments and use cases."
has_children: true
has_toc: false
---

# Installing SAIGE-QTL

Choose the installation method that best fits your environment and requirements. We recommend **Docker** for most users as it works on all platforms with zero setup, followed by **Pixi Binary** for Linux users seeking the fastest installation.

## Current Version: 0.3.4
*Updated: January 11, 2025*

## Installation Methods (Ordered by Ease of Use)

### 🥇 **Easiest: Docker (Recommended)**
Zero setup, works on Linux, macOS, Windows - everything included in the container.

**[→ Install with Docker](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_docker.html)**

### 🥈 **Easy: Pixi Binary (Linux only)**
Fastest installation for Linux - no compilation required, pre-built packages.

**[→ Install with Pixi Binary](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_pixi.html)**

### 🥉 **Moderate: Conda/Bioconda**
Cross-platform package manager with automatic dependency management.

**[→ Install with Conda](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_bioconda.html)**

### 🔧 **Advanced: Source Code**
For developers, custom builds, or users needing the latest development features.

**[→ Install from Source](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation_sourcecode.html)**

## Platform Support & Quick Start

| Platform | Support Level | Recommended Methods | Notes |
|----------|---------------|-------------------|--------|
| **Linux** | ✅ **Full Support** | Docker → Pixi Binary → Conda → Source | Pre-built binaries available |
| **macOS** | ✅ **Good Support** | Docker → Conda → Source | No pre-built binaries |
| **Windows** | ⚠️ **Limited Support** | Docker only, or WSL2 + Linux methods | Not natively supported |

## Installation Comparison

| Method | Platform | Setup Time | Dependencies | Best For |
|--------|----------|------------|--------------|----------|
| **Docker** | All platforms | ~2 minutes | Docker only | Zero setup, containers, HPC |
| **Pixi Binary** | Linux only | ~5 minutes | Pixi + GLIBC 2.28+ | Fastest Linux installation |
| **Conda** | Linux, macOS | ~10 minutes | Conda/Mamba | Existing conda environments |
| **Pixi Source** | Linux, macOS | ~15 minutes | Pixi only | Managed dev environment |
| **Source** | Linux, macOS | ~30 minutes | C++ compiler + libraries | Traditional development |

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

## What's New in Version 0.3.4

### ✨ **New Features**
- **Improved installation guidance**: Comprehensive installation documentation with platform-specific recommendations and troubleshooting
- **Enhanced Docker support**: Updated Docker images with better documentation and cross-platform compatibility
- **Binary installation method**: Pre-built packages for faster Linux installation via Pixi

### 🚀 **Performance Improvements**
- **Speed up matrix inversion**: Optimized numerical computations for faster model fitting and reduced computation time
- **Memory efficiency**: Better memory management for large-scale analyses

### 📋 **Previous Features (0.3.2)**
- **`--offsetCol` parameter**: Use log of total read counts per cell as an offset in the model
- **Enhanced Pixi support**: Improved installation process and environment management
- **Better error handling**: More informative error messages and debugging information

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


---
layout: default
title: Calling SAIGE-QTL
nav_order: 4
description: "Complete guide for running SAIGE-QTL scripts across different environments with detailed examples and best practices."
has_children: true
has_toc: false
---

# Running SAIGE-QTL Scripts

This guide shows how to execute SAIGE-QTL analysis scripts in different computing environments. Choose the method that matches your [installation approach](Installation.html).

## Available Scripts

SAIGE-QTL provides four main R scripts for the complete analysis pipeline:

| Script | Purpose | When to Use |
|--------|---------|-------------|
| `step1_fitNULLGLMM_qtl.R` | Fit null Poisson mixed model | Required first step for all analyses |
| `step2_tests_qtl.R` | Perform association tests | Main analysis step for variant testing |
| `step3_gene_pvalue_qtl.R` | Calculate gene-level p-values | Optional, for gene-based rare variant tests |
| `makeGroupFile.R` | Create variant group files | For set-based rare variant analyses |

📁 **Script Location**: All scripts are in the `./SAIGEQTL/extdata` directory

## Quick Reference

| Environment | Command Prefix | Working Directory |
|-------------|----------------|-------------------|
| **Pixi** | `pixi run Rscript extdata/` | `./SAIGEQTL/` |
| **Docker** | `docker run -v $PWD:/extdata -w /extdata wzhou88/saigeqtl:latest` | `./SAIGEQTL/extdata` |
| **Singularity** | `singularity exec --bind $PWD:/extdata saigeqtl_latest.sif` | `./SAIGEQTL/extdata` |


---

## 🚀 Pixi Environment

**Recommended method** - Provides reproducible environments with automatic dependency management.

### Prerequisites
- Navigate to the SAIGEQTL root directory (containing `pixi.toml`)
- Ensure pixi is installed and environment is set up

### Basic Usage

```bash
cd ./SAIGEQTL
pixi run Rscript extdata/step1_fitNULLGLMM_qtl.R [options]
```

### Getting Help

```bash
pixi run Rscript extdata/step1_fitNULLGLMM_qtl.R --help
```

### Remote Directory Usage
If you're outside the SAIGEQTL directory:

```bash
pixi run --manifest-path=/full/path/to/SAIGEQTL/pixi.toml Rscript extdata/step1_fitNULLGLMM_qtl.R [options]
```

### How It Works
- `pixi run` activates the reproducible environment defined in `pixi.toml`
- Scripts are referenced by their relative path: `extdata/script_name.R`
- All dependencies are automatically managed


---

## 🐳 Docker Environment

**Container-based method** - Pre-built environment, no local installation required.

### Prerequisites
- Docker installed and running
- Navigate to the SAIGEQTL/extdata directory

### Setup
```bash
cd ./SAIGEQTL/extdata
```

### Basic Usage

```bash
docker run -v $PWD:/extdata -w /extdata wzhou88/saigeqtl:latest step1_fitNULLGLMM_qtl.R [options]
```

### Getting Help

```bash
docker run -v $PWD:/extdata -w /extdata wzhou88/saigeqtl:latest step1_fitNULLGLMM_qtl.R --help
```

### How It Works
- `-v $PWD:/extdata` mounts your current directory into the container as `/extdata`
- `-w /extdata` sets the container's working directory to `/extdata`
- `$PWD` automatically resolves to your current directory path
- All input/output files must be in the mounted directory

---

## 🧊 Singularity Environment

**HPC-friendly method** - Container solution compatible with high-performance computing clusters.

### Prerequisites
- Singularity/Apptainer installed
- Access to `saigeqtl_latest.sif` image file
- Navigate to the SAIGEQTL/extdata directory

### Setup
```bash
cd ./SAIGEQTL/extdata
```

### Basic Usage

```bash
singularity exec \
--bind $PWD:/extdata \
--cleanenv saigeqtl_latest.sif \
step1_fitNULLGLMM_qtl.R [options]
```

### Getting Help

```bash
singularity exec \
--bind $PWD:/extdata \
--cleanenv saigeqtl_latest.sif \
step1_fitNULLGLMM_qtl.R --help
```

### How It Works
- `--bind $PWD:/extdata` gives the container access to your local directory
- `--cleanenv` ensures a clean environment inside the container
- The `.sif` file path may need to be absolute if not in your PATH
- Ideal for HPC environments where Docker isn't available 

---

## 📝 Important Notes

### Custom Library Support
- **New Feature**: All wrapper scripts now support the `--library` parameter
- Use `--library=/path/to/custom/library` when SAIGEQTL is installed in a non-standard location
- No more manual editing of wrapper scripts with `lib.loc` specifications
- Example: `step1_fitNULLGLMM_qtl.R --library=/custom/path [other_options]`

### File Locations and Access
- **Script location**: All R scripts are in `./SAIGEQTL/extdata`
- **Data access**: Ensure input/output files are in directories accessible to your chosen environment:
  - **Pixi**: Files can be anywhere accessible from your system
  - **Docker/Singularity**: Files must be in bound/mounted directories

### Command Structure
- Replace `[options]` with actual script arguments (e.g., `--phenoFile`, `--genoFile`, `--covarColList`)
- Use `--help` with any script to see all available parameters
- The command patterns above work for all four SAIGE-QTL scripts - just change the script name
- **New**: All wrapper scripts now support the `--library` parameter for custom library locations (eliminates need for manual `lib.loc` edits)

### Universal Script Support
All examples above work with any of these scripts:
```
step1_fitNULLGLMM_qtl.R    # Null model fitting
step2_tests_qtl.R          # Association testing  
step3_gene_pvalue_qtl.R    # Gene-level analysis
makeGroupFile.R            # Group file creation
```

## 🚀 Next Steps

1. **[Start with Step 1](step1.html)** - Fit your null models with detailed examples
2. **[Choose your analysis type](overview.html#analysis-types)**:
   - **[cis-eQTL analysis](cis-eQTL.html)** - Local variant testing
   - **[Genome-wide analysis](genomewide-eQTL.html)** - Comprehensive variant screening

## 💡 Tips for Success

- **Test first**: Always run scripts with `--help` to verify they work in your environment
- **Path management**: Use absolute paths for input/output files when possible
- **Resource planning**: Ensure adequate memory and storage for your dataset size
- **Error checking**: Review log files and error messages carefully for troubleshooting

---
layout: default
title: Docker (Recommended)
nav_order: 1
description: "Zero setup installation using Docker containers - works on all platforms with no local dependencies required."
parent: Installation
---

# Docker Installation Guide (🥇 Easiest Method)

## Overview

Docker provides the **easiest and most reliable** installation method for SAIGE-QTL. It works identically on **Linux, macOS, and Windows** with zero setup required - no compilers, no dependency management, no configuration needed.

## Why Docker?

**✅ Advantages:**
- **Zero setup** - runs directly, no installation required
- **Works on any system** with Docker (Linux/macOS/Windows)
- **Completely isolated** environment - no dependency conflicts
- **Consistent behavior** across all platforms
- **Includes all tools** and test data
- **Immediate access** to new versions

**⚠️ Requirements:**
- Docker installed on your system
- No other requirements

## Quick Start

### Test Docker Installation

```bash
# Quick test - verify Docker image works
docker run --rm weizhou0/saigeqtl:latest step1_fitNULLGLMM_qtl.R --help
```

### Run Analysis with Your Data

```bash
# Run analysis with your data (mount current directory)
docker run --rm -v $(pwd):/data weizhou0/saigeqtl:latest \
  step1_fitNULLGLMM_qtl.R \
  --plinkFile=/data/your_genotypes \
  --phenoFile=/data/your_phenotypes.txt \
  --outputPrefix=/data/results
```

### Interactive R Session

```bash
# Interactive R session with SAIGEQTL loaded
docker run --rm -it -v $(pwd):/data weizhou0/saigeqtl:latest \
  pixi run R
```

## Docker Image Details

- **Image**: `weizhou0/saigeqtl:latest`
- **Base**: Ubuntu 20.04 with pixi environment
- **Size**: ~2GB (includes R, dependencies, and tools)
- **Updates**: Automatically built from latest code
- **Repository**: Built from https://github.com/weizhou0/SAIGEQTL

### Available SAIGE-QTL Functions

The following functions are available in the Docker container:

```
step1_fitNULLGLMM_qtl.R
step2_tests_qtl.R
step3_gene_pvalue_qtl.R
makeGroupFile.R
```

### Running SAIGE-QTL on Local Systems

To run SAIGE-QTL functions locally using Docker:

```bash
# Step 1: Fit NULL GLMM model
docker run wzhou88/saigeqtl:latest step1_fitNULLGLMM_qtl.R --help

# Step 2: Run association tests
docker run wzhou88/saigeqtl:latest step2_tests_qtl.R --help

# Step 3: Calculate gene-level p-values
docker run wzhou88/saigeqtl:latest step3_gene_pvalue_qtl.R --help

# Create group files
docker run wzhou88/saigeqtl:latest makeGroupFile.R --help
```

## Singularity Installation and Usage

### Prerequisites
- Singularity installed on your system (common on HPC clusters)
- Access to pull Docker images

### Pull and Convert Docker Image

```bash
# Load Singularity module (if using module system)
module load singularity

# Pull Docker image and convert to Singularity format
# Navigate to the folder to store the singularity image file saigeqtl_latest.sif

PATHTOSIF=/data/wzhougroup/
cd ${PATHTOSIF}
singularity pull docker://wzhou88/saigeqtl:latest
```

This creates a Singularity image file (e.g., `saigeqtl_latest.sif`).

### Running SAIGE-QTL with Singularity

#### Interactive Shell Access

```bash
singularity exec --bind /data/wzhougroup:/data/wzhougroup \
    --cleanenv /path/to/saigeqtl_latest.sif bash
```

**Note**:
- `--bind`: Mounts directories from the host system into the container
- Replace `/data/wzhougroup` with your actual data directories
- Replace `/path/to/saigeqtl_latest.sif` with the actual path to your Singularity image

#### Running SAIGE-QTL Functions

From within the Singularity container:

```bash
# Step 1: Fit NULL GLMM model
step1_fitNULLGLMM_qtl.R --help

# Step 2: Run association tests
step2_tests_qtl.R --help

# Step 3: Calculate gene-level p-values
step3_gene_pvalue_qtl.R --help

# Create group files
makeGroupFile.R --help
```

#### Direct Execution (Non-interactive)

```bash

singularity exec --bind /data/wzhougroup:/data/wzhougroup --cleanenv saigeqtl_latest.sif step1_fitNULLGLMM_qtl.R --help
```

## SLURM Integration

### Basic SLURM Setup

For SLURM job submission, include these basic steps in your submission script:

```bash
module load singularity
singularity exec --bind /your/data/path:/your/data/path \
    --cleanenv /path/to/saigeqtl_latest.sif \
    [your_command]
```

### Complete SLURM Submission Script Example

```bash
#!/bin/bash
#SBATCH --job-name=saige-qtl-analysis
#SBATCH --time=0:20:00
#SBATCH --partition=normal
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=2G
#SBATCH --output=/path/to/logs/%A_%a.out
#SBATCH --error=/path/to/logs/%A_%a.err
#SBATCH --mail-user=your.email@institution.edu
#SBATCH --mail-type=END
#SBATCH --array=1-42

# Load required modules
module load singularity

# Define job from array
i=${SLURM_ARRAY_TASK_ID}
joblist=/path/to/job_scripts/job_list.txt
declare -a FILES=($(cat $joblist))
eachjob=${FILES[$i]}

# Run job with timing information
/bin/time -o /path/to/logs/run.${SLURM_ARRAY_TASK_ID}.timing.txt -v \
singularity exec --bind /data/path:/data/path \
    --cleanenv /path/to/saigeqtl_latest.sif \
    bash "${eachjob}"
```

### SLURM Script Parameters Explanation

| Parameter | Description |
|-----------|-------------|
| `--job-name` | Name for the job (appears in queue) |
| `--time` | Maximum runtime (HH:MM:SS format) |
| `--partition` | SLURM partition/queue to use |
| `--ntasks` | Number of tasks (typically 1 for single jobs) |
| `--cpus-per-task` | CPU cores per task |
| `--mem` | Memory allocation per job |
| `--array` | Submit array of jobs (1-42 means 42 jobs) |
| `--output` | Standard output file location |
| `--error` | Standard error file location |

### Job Array Management

For large-scale analyses, use job arrays:

1. **Create a job list file** (`job_list.txt`):
   ```
   script1.sh
   script2.sh
   script3.sh
   ...
   ```

2. **Each script contains SAIGE-QTL commands**:
   ```bash
   #!/bin/bash
   step2_tests_qtl.R \
       --vcfFile=/data/genotypes/chr1.vcf.gz \
       --vcfFileIndex=/data/genotypes/chr1.vcf.gz.csi \
       --SAIGEOutputFile=/data/results/chr1_results.txt \
       [other options]
   ```

## Best Practices

### Data Management
- Always use absolute paths for data files
- Ensure proper directory binding with `--bind` option
- Create separate directories for logs, results, and temporary files

### Resource Allocation
- Monitor memory usage and adjust `--mem` accordingly
- For large datasets, consider increasing CPU allocation
- Use appropriate time limits based on data size

### Error Handling
- Always specify separate output and error log files
- Include timing information for performance monitoring
- Use descriptive job names for easier tracking

## Troubleshooting

### Common Issues

1. **Permission Errors**
   - Ensure proper file permissions on mounted directories
   - Check that Singularity can access the image file

2. **Memory Issues**
   - Increase memory allocation in SLURM script
   - Monitor actual memory usage with timing tools

3. **Path Issues**
   - Use absolute paths for all file references
   - Verify that bound directories exist on the host system

4. **Module Loading**
   - Ensure Singularity module is available: `module avail singularity`
   - Check module dependencies

### Getting Help

- Check SLURM documentation: `man sbatch`
- Singularity documentation: `singularity help`
- Institution-specific HPC documentation
- SAIGE-QTL GitHub repository for software-specific issues

## Custom Dockerfile

The Dockerfile can be found in the SAIGE-QTL repository at `./docker/Dockerfile`. You can modify and rebuild the image if needed:

```bash
git clone https://github.com/weizhou0/SAIGEQTL.git
cd SAIGEQTL/docker
docker build -t your-custom-saigeqtl .
```

---

## ✅ Next Steps

### Installation Complete!

You've successfully set up SAIGE-QTL with Docker or Singularity. Here's how to get started with your first analysis.

### Your Command Prefix

All SAIGE-QTL commands will use this format:

```bash
# For Docker
docker run -v /your/data:/data wzhou88/saigeqtl:latest [script_name] [options]

# For Singularity
singularity exec --bind /your/data:/data /path/to/saigeqtl_latest.sif [script_name] [options]
```

**💡 Important**: Replace `/your/data` with your actual data directory paths.

### Quick Start Tutorial

Ready to run your first analysis? Follow this tutorial:

**[📖 cis-eQTL Analysis Tutorial](cis-eQTL.html)** - Learn how to map local genetic effects on gene expression

**What you'll learn:**
- How to prepare your data files
- Running Step 1: Fit the null model
- Running Step 2: Test genetic variants
- Running Step 3: Calculate gene-level p-values

**Example command for Docker users:**
```bash
# Step 1 example
docker run -v /data/myproject:/data wzhou88/saigeqtl:latest \
    step1_fitNULLGLMM_qtl.R \
    --phenoFile=/data/phenotypes.txt \
    --phenoCol=ENSG00000123456 \
    --traitType=count \
    --outputPrefix=/data/output/gene1
```

**Example command for Singularity users:**
```bash
# Step 1 example
singularity exec --bind /data/myproject:/data /path/to/saigeqtl_latest.sif \
    step1_fitNULLGLMM_qtl.R \
    --phenoFile=/data/phenotypes.txt \
    --phenoCol=ENSG00000123456 \
    --traitType=count \
    --outputPrefix=/data/output/gene1
```

### Running on HPC with SLURM

If you're using an HPC cluster, see the SLURM examples above. A typical workflow:

1. Create your analysis scripts (Step 1, Step 2, Step 3)
2. Create a job submission script using the template above
3. Submit: `sbatch your_job_script.sh`
4. Monitor: `squeue -u $USER`

### Additional Resources

- **[Parameters & Options](parameters.html)** - Complete reference for all command-line parameters
- **[Running Scripts Guide](calling-saigeqtl.html)** - More examples for different environments
- **[FAQ](FAQ.html)** - Common questions and troubleshooting

### Container-Specific Tips

✅ **Always use absolute paths** inside containers
✅ **Bind all necessary directories** with `-v` (Docker) or `--bind` (Singularity)
✅ **Check file permissions** if you encounter access errors
✅ **Use `--cleanenv`** with Singularity to avoid environment conflicts

### Need Help?

If you encounter issues:
1. Check the [FAQ](FAQ.html) for container-specific problems
2. Verify your mounts: Ensure data directories are properly bound
3. Review the [cis-eQTL tutorial](cis-eQTL.html) for step-by-step guidance

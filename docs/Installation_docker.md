---
layout: default
title: Docker and Singularity (with Slurm script)
nav_order: 2
description: "Steps to use Docker or Singularity for running SAIGE-QTL jobs."
parent: Installation
---

# Docker and Singularity Guide for SAIGE-QTL

## Overview

This guide provides instructions for running SAIGE-QTL using containerized environments with Docker and Singularity, including integration with SLURM job scheduling systems.

## Docker Installation and Usage

### Prerequisites
- Docker installed on your system
- Access to pull images from Docker Hub

### Pull the SAIGE-QTL Docker Image

The pre-built Docker image can be pulled directly from Docker Hub:

```bash
docker pull wzhou88/saigeqtl:0.3.2
```

**Note**: Thanks to Juha Karjalainen, Bram Gorissen, and Masa Kanai for sharing and updating the Dockerfile.

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
docker run wzhou88/saigeqtl:0.3.2 step1_fitNULLGLMM_qtl.R --help

# Step 2: Run association tests
docker run wzhou88/saigeqtl:0.3.2 step2_tests_qtl.R --help

# Step 3: Calculate gene-level p-values
docker run wzhou88/saigeqtl:0.3.2 step3_gene_pvalue_qtl.R --help

# Create group files
docker run wzhou88/saigeqtl:0.3.2 makeGroupFile.R --help
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
singularity pull docker://wzhou88/saigeqtl:0.3.2
```

This creates a Singularity image file (e.g., `saigeqtl_0.3.2.sif`).

### Running SAIGE-QTL with Singularity

#### Interactive Shell Access

```bash
singularity exec --bind /data/wzhougroup:/data/wzhougroup \
    --cleanenv /path/to/saigeqtl_0.3.2.sif bash
```

**Note**:
- `--bind`: Mounts directories from the host system into the container
- Replace `/data/wzhougroup` with your actual data directories
- Replace `/path/to/saigeqtl_0.3.2.sif` with the actual path to your Singularity image

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

singularity exec --bind /data/wzhougroup:/data/wzhougroup --cleanenv saigeqtl_0.3.2.sif step1_fitNULLGLMM_qtl.R --help
```

## SLURM Integration

### Basic SLURM Setup

For SLURM job submission, include these basic steps in your submission script:

```bash
module load singularity
singularity exec --bind /your/data/path:/your/data/path \
    --cleanenv /path/to/saigeqtl_0.3.2.sif \
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
    --cleanenv /path/to/saigeqtl_0.3.2.sif \
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

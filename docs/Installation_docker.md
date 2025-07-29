---
layout: default
title: Docker and Singularity (with Slurm script)
nav_order: 2
description: "Steps to use Docker or Singularity for running SAIGE-QTLjobs."
parent: Installation
---


### Run SAIGE-QTL using a docker image

Note: Thanks to Juha Karjalainen, Bram Gorissen, and Masa Kanai for sharing and updating the Dockerfile. 

* Dockerfile can be found in the SAIGE-QTL folder: ./docker/Dockerfile

* The docker image can be pulled

    ```
    docker pull wzhou88/saigeqtl:0.3.2
    ```

Functions can be called

```
step1_fitNULLGLMM_qtl.R --help
step2_tests_qtl.R --help
step3_gene_pvalue_qtl.R --help
makeGroupFile.R  --help
```

### If docker version of SAIGE-QTL is run on a local system

```
docker run wzhou88/saigeqtl:0.3.2 step1_fitNULLGLMM_qtl.R --help
docker run wzhou88/saigeqtl:0.3.2 step2_tests_qtl.R --help
docker run wzhou88/saigeqtl:0.3.2 step3_gene_pvalue_qtl.R --help
docker run wzhou88/saigeqtl:0.3.2 makeGroupFile.R  --help
```

NOTE: ```-v``` option is also important for docker run to ```Bind mount a volume```. Check [link](https://docs.docker.com/engine/reference/commandline/run/#:~:text=%2D%2Dvolume%20%2C%20%2Dv,mount%20a%20volume) for more details.


### If singularity is used

```
module load singularity
singularity pull docker://wzhou88/saigeqtl:0.3.2
singularity exec --bind /data/wzhougroup:/data/wzhougroup /data/wzhougroup/cliu/saige_qtl_dynamic/saigeqtl_0.3.2.sif bash

```

 Note: --bind: mount (bind) a directory or file from your host system into the container, so it is  accessible inside the container environment
#/data/wzhougroup/cliu/saige_qtl_dynamic/saigeqtl_0.3.2.sif is path to the Singularity image file

Then functions can be called from the singularity container

```
Rscript /usr/local/bin/step1_fitNULLGLMM_qtl.R --help
Rscript /usr/local/bin/step2_tests_qtl.R --help
Rscript /usr/local/bin/step3_gene_pvalue_qtl.R --help
Rscript /usr/local/bin/makeGroupFile.R --help

```

### To submit a job in SLURM

We just simply put the following steps in the submit script:

```
module load singularity
singularity exec --bind #to bind the folder
```

Here is an example of submitting script for slurm 

```
#!/bin/bash
#SBATCH --job-name=saige-b-IN
#SBATCH --time=0:20:00
#SBATCH --partition=normal
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=2G
#SBATCH --output=/data/wzhougroup/cliu/saige_qtl_dynamic/b_IN/wei_original/log/%A_%a.out
#SBATCH --error=/data/wzhougroup/cliu/saige_qtl_dynamic/b_IN/wei_original/log/%A_%a.err
#SBATCH --mail-user=yliu138@mgh.harvard.edu
#SBATCH --mail-type=END
#SBATCH --array=1-42

#load docker
module load singularity

# Define job from array
i=${SLURM_ARRAY_TASK_ID}
joblist=/data/wzhougroup/cliu/saige_qtl_dynamic/b_IN/wei_original/sub_scripts/b_IN.job
declare -a FILES=($(cat $joblist))
eachjob=${FILES[$i]}

# Run each job script inside Singularity container
/bin/time -o /data/wzhougroup/cliu/saige_qtl_dynamic/b_IN/wei_original/log/run.${SLURM_ARRAY_TASK_ID}.b_IN.job.runinfo.txt -v \
singularity exec --bind /data/wzhougroup:/data/wzhougroup \
/data/wzhougroup/cliu/saige_qtl_dynamic/saigeqtl_0.3.2.sif bash "${eachjob}"
```

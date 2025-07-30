---
layout: default
title: Calling SAIGE-QTL
nav_order: 4
description: "Guidelines for running SAIGE-QTL"
has_children: true
has_toc: false
---

## Calling SAIGE-QTL

This guide describes how to run the SAIGE-QTL scripts 
  ```
  step1_fitNULLGLMM_qtl.R
  step2_tests_qtl.R
  step3_gene_pvalue_qtl.R
  makeGroupFile.R
  ```
using different environments: Pixi, Docker, and Singularity. All R scripts are located in the `./SAIGEQTL/extdata` directory. You can refer to the corresponding section below depending on your setup.


---


###  Pixi

From within the SAIGEQTL root directory (which includes `pixi.toml`):

```bash
cd ./SAIGEQTL
pixi run Rscript extdata/step1_fitNULLGLMM_qtl.R [options]
```

**Example:**

```bash
pixi run Rscript extdata/step1_fitNULLGLMM_qtl.R --help
```

If you are outside the SAIGEQTL directory, you can specify the manifest path:

```bash
pixi run --manifest-path=/full/path/to/SAIGEQTL/pixi.toml Rscript extdata/step1_fitNULLGLMM_qtl.R [options]
```

**Explanation:**
- `pixi run` activates the reproducible environment defined in `pixi.toml`
- Scripts are referenced by their path inside the repo (e.g., `extdata/step1_fitNULLGLMM_qtl.R`)


---

### Docker

Navigate to the `SAIGEQTL/extdata` directory first:

```bash
cd ./SAIGEQTL/extdata
```

Then run:

```bash
docker run -v $PWD:/extdata -w /extdata wzhou88/saigeqtl:0.3.2 step1_fitNULLGLMM_qtl.R [options]
```

**Example:**

```bash
docker run -v $PWD:/extdata -w /extdata wzhou88/saigeqtl:0.3.2 step1_fitNULLGLMM_qtl.R --help
```

**Explanation:**
- `-v $PWD:/extdata` mounts the current directory into the Docker container as `/extdata`
- `-w /extdata` sets the containers working directory to `/extdata`
- `$PWD` automatically resolves to your current directory path
- `[options]` are script-specific arguments (see Notes below)

---

### Singularity

Navigate to the `SAIGEQTL/extdata` directory first:

```bash
cd ./SAIGEQTL/extdata

singularity exec \
--bind $PWD:/extdata \
--cleanenv saigeqtl_0.3.2.sif \
step1_fitNULLGLMM_qtl.R [options]

```


**Explanation:**
- `--bind $PWD:/extdata` gives the container access to your local `extdata` directory
- The `.sif` file is the Singularity image for SAIGE-QTL. Its full path will be needed if it is not in the default path of singularity containers 

---

## Notes

- All R scripts are located in `./SAIGEQTL/extdata`
- Replace `[options]` with the actual arguments required by the script (e.g., `--phenoFile`, `--genoFile`, `--covarColList`, etc.)
- Use `--help` to see the full list of arguments for any script
- Make sure input/output files are located in the bound/mounted directory so the container or environment can access them
- The same format applies to the following SAIGE-QTL scripts:

  ```
  step1_fitNULLGLMM_qtl.R
  step2_tests_qtl.R
  step3_gene_pvalue_qtl.R
  makeGroupFile.R
  ```

  Just replace the script name in the example commands above with the one you wish to run.

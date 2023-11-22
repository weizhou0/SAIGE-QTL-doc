---
layout: default
title: Conda
nav_order: 2
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
parent: Installation
---

###  Install SAIGE-QTL using the conda environment

0. Download and install [miniconda](https://docs.conda.io/en/latest/miniconda.html)


1. Create a conda environment as follows:

     * the conda environment file can be found in the folder: ```./conda_env/environment-RSAIGE.yml```

     * After downloading ```environment-RSAIGE.yml```, run the following command to create a new conda environment:

     ```bash
       conda env create -f environment-RSAIGE.yml
     ```

2. Activate the conda environment RSAIGE, by running:

     ```bash
       conda activate RSAIGE
       FLAGPATH=`which python | sed 's|/bin/python$||'`
       export LDFLAGS="-L${FLAGPATH}/lib"
       export CPPFLAGS="-I${FLAGPATH}/include"
     ```
     
     Please make sure to set up the LDFLAGS and CPPFLAGS using export (the last two command lines), so libraries can be linked correctly when the SAIGE source code is compiled.

    Note: [Here](https://github.com/weizhou0/qtl/blob/main/conda_env/createCondaEnvSAIGE_steps.txt) are the steps to create the conda environment file.


3. Install SAIGE-QTL from the source code, by running:

     ```bash
       src_branch=main
       repo_src_url=https://github.com/weizhou0/qtl
       git clone --depth 1 -b $src_branch $repo_src_url
       Rscript ./qtl/extdata/install_packages.R
       R CMD INSTALL --library=path_to_final_SAIGEQTL_library qtl
     ```

     This will have installed all necessary dependencies as well as SAIGE-QTL itself, including the example data and the R wrappers.
   To use these wrapper Rscripts (e.g., ```step1_fitNULLGLMM_qtl.R```), you will need to specify the location where you just installed SAIGE-QTL at the top of the script,
   so change ```R library(SAIGEQTL``` to the library you just installed the package to, as:

     ```R
       library(SAIGEQTL, lib.loc=path_to_final_SAIGEQTL_library)
     ```

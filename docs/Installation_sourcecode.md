---
layout: default
title: Source code
nav_order: 3
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
parent: Installation
published: false # Set to false to prevent rendering
---

### Intall SAIGE-QTL using source code https://github.com/weizhou0/qtl

1. Install dependcies 

     * R >= 3.6.1, gcc >= 5.4.0, cmake 3.14.1,
    
2. Download SAIGE-QTL from github

     ```
       src_branch=main
       repo_src_url=https://github.com/weizhou0/SAIGEQTL
       git clone --depth 1 -b $src_branch $repo_src_url	
     ```

3. Install dependencies: R packages

     ```
	Rscript ./SAIGEQTL/extdata/install_packages.R
        
     ```

4. Install SAIGE-QTL R package

     * To install SAIGE-QTL to the root directory storing all R libraries
     ```
        R CMD INSTALL SAIGEQTL
     ```

     * **--library=path_to_final_SAIGEQTL_library** can be used for specifying the directory where SAIGE-QTL is installed 
     ```
	R CMD INSTALL --library=path_to_final_SAIGEQTL_library SAIGEQTL
     ```

5. Run SAIGE-QTL
     * If SAIGE-QTL was not installed in the root R lib path, change 

     ```
        library(SAIGEQTL)
     ```
     to      
     ```
        library(SAIGEQTL, lib.loc="path_to_final_SAIGEQTL_library")
     ```

     in the following scripts

    ```
    SAIGEQTL/extdata/makeGroupFile.R
    SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R
    SAIGEQTL/extdata/step2_tests_qtl.R
    SAIGEQTL/extdata/step3_gene_pvalue_qtl.R
    ```

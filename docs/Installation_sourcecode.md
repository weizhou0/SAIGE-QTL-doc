---
layout: default
title: Source code
nav_order: 3
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
parent: Installation
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

---

## ✅ Next Steps

### Installation Complete!

You've successfully built SAIGE-QTL from source. Here's how to get started with your first analysis.

### Your Command Prefix

Your scripts are located in:
```bash
/path/to/SAIGEQTL/extdata/
```

Run commands like:
```bash
Rscript /path/to/SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R [options]
```

**💡 Tip**: Add to PATH for convenience:
```bash
# Add to your ~/.bashrc or ~/.bash_profile
export PATH="/path/to/SAIGEQTL/extdata:$PATH"

# Then you can run:
step1_fitNULLGLMM_qtl.R --help
```

### Quick Start Tutorial

Ready to run your first analysis? Follow this tutorial:

**[📖 cis-eQTL Analysis Tutorial](cis-eQTL.html)** - Learn how to map local genetic effects on gene expression

**What you'll learn:**
- How to prepare your data files
- Running Step 1: Fit the null model
- Running Step 2: Test genetic variants
- Running Step 3: Calculate gene-level p-values

**Example command for Source installation users:**
```bash
Rscript /path/to/SAIGEQTL/extdata/step1_fitNULLGLMM_qtl.R \
    --phenoFile=phenotypes.txt \
    --phenoCol=ENSG00000123456 \
    --traitType=count \
    --outputPrefix=output/gene1
```

### Additional Resources

- **[Parameters & Options](parameters.html)** - Complete reference for all command-line parameters
- **[Running Scripts Guide](calling-saigeqtl.html)** - Detailed information on running SAIGE-QTL scripts
- **[FAQ](FAQ.html)** - Common questions and troubleshooting

### Need Help?

If you encounter issues:
1. Check the [FAQ](FAQ.html) for common problems
2. Verify R can find the SAIGEQTL library
3. Review the [cis-eQTL tutorial](cis-eQTL.html) for step-by-step guidance

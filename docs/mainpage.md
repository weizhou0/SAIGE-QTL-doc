---
layout: default
title: Home
nav_order: 1
description: "Documentation for SAIGE-QTL."
permalink: /
---


SAIGE-QTL is an R package developed with Rcpp for scalable and accurate expression quantitative trait locus (QTL) mapping for single-cell studies.

The method
- Models repeated and complex data structure, due to multiple cells per individual and relatedness between individuals
- Models discrete read counts
- Is fast and scalable for large data, can test 20k genes, tens to hundreds of cell types, millions of cells, millions of genetic variants
- Can test for the effects of rare variation, for which the single-variant test is underpowered (uses set-based tests instead).

The package takes genotype file input in the following formats
- PLINK (bed, bim, fam), BGEN, VCF, BCF, SAV

Please note that accounting for total read counts per cell is critical in single-cell eQTL mapping. The SCTransform in the SCTransform function in the Seurat R package and/or including log(total read counts) and percentage of MT read counts for each cell as covariates in the Step 1 null model can be used.  


## Logs 

Please find logs for bugs fixed
[https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation.html](https://weizhou0.github.io/SAIGE-QTL-doc/docs/Installation.html)


## Citation

### SAIGE-QTL
- preprint coming out soon!

## License
SAIGE-QTL is distributed under an MIT license.


## Contact
If you have any questions about SAIGE-QTL please contact
wzhou@broadinstitute.org.

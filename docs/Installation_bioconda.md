---
layout: default
title: Bioconda (Not activated)
nav_order: 3
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
parent: Installation
---

###  Install SAIGE from bioconda (Not current version)

#### Warning: please do not use this bioconda version for bgen input. We are working on the issue.
![r-saige](https://anaconda.org/bioconda/r-saige/badges/version.svg)
![latest_update](https://anaconda.org/bioconda/r-saige/badges/latest_release_date.svg)

To install saige from conda simply create environment with latest version of R and saige:
```
conda create -n saige -c conda-forge -c bioconda "r-base>=4.0" r-saige
conda activate saige
```

More info on [r-saige conda package](https://anaconda.org/bioconda/r-saige) and available versions can be found in the [issue #272](https://github.com/weizhouUMICH/SAIGE/issues/272).


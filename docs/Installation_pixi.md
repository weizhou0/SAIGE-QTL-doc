---
layout: default
title: Conda
nav_order: 1
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
parent: Installation
---

###  Install SAIGE-QTL using the pixi


Note: These steps can be found in the [Dockerfile](https://github.com/weizhou0/qtl/blob/main/docker/Dockerfile).


0. Download the SAIGE-QTL package from github

```
src_branch=main
repo_src_url=https://github.com/weizhou0/qt
git clone -b $src_branch $repo_src_url

```

1. Install pixi and the R package lintools
```
curl -fsSL https://pixi.sh/install.sh | sh && \
    mv /root/.pixi/bin/pixi /bin && pixi install && \
    rm -rf /root/.cache && \
    pixi run Rscript -e 'install.packages("lintools", repos="https://cloud.r-project.org")' && \
    pixi run Rscript -e 'install.packages("remotes", repos="https://cloud.r-project.org"); remotes::install_github("barkasn/fastSave")'
```

2. Download plink source code for reading pgen files in Step 2 association tests
```
curl -L https://github.com/chrchang/plink-ng/archive/refs/tags/v2.0.0-a.6.16.tar.gz | tar -zx && \
    mv plink-ng-2.0.0-a.6.16 plink-ng && \
    pixi run x86_64-conda-linux-gnu-cc -std=c++14 -fPIC -O3 -I.pixi/envs/default/include -L.pixi/envs/default/lib -o plink2_includes.a plink-ng/2.0/include/*.cc -shared -lz -lzstd -lpthread -lm -ldeflate && \
    mv plink2_includes.a .pixi/envs/default/lib
```

3. Install the SAIGE-QTL package

```
CONDA_OVERRIDE_GLIBC=2.28 pixi run  --manifest-path=./SAIGE/pixi.toml  R CMD INSTALL SAIGE --library=path_to_final_SAIGEQTL_library

```

When call SAIGE-QTL in R, set lib.loc=path_to_final_SAIGEQTL_library


```
    library(SAIGEQTL, lib.loc=path_to_final_SAIGEQTL_library)
```

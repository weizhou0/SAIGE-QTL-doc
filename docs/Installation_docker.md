---
layout: default
title: Docker
nav_order: 3
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
parent: Installation
---


### Run SAIGE-QTL using a docker image

Thanks to Juha Karjalainen for sharing the Dockerfile.

* Dockerfile can be found in the SAIGE-QTL folder: ./docker/Dockerfile

* The docker image can be pulled

    ```
    docker pull wzhou88/saigeqtl:0.1.0
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
docker run wzhou88/saigeqtl:0.1.0 step1_fitNULLGLMM_qtl.R --help
docker run wzhou88/saigeqtl:0.1.0 step2_tests_qtl.R --help
docker run wzhou88/saigeqtl:0.1.0 step3_gene_pvalue_qtl.R --help
docker run wzhou88/saigeqtl:0.1.0 makeGroupFile.R  --help
```

NOTE: ```-v``` option is also important for docker run to ```Bind mount a volume```. Check [link](https://docs.docker.com/engine/reference/commandline/run/#:~:text=%2D%2Dvolume%20%2C%20%2Dv,mount%20a%20volume) for more details.

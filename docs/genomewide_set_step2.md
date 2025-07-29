---
layout: default
title: Step 2 - genome-wide (rare variants)
nav_order: 2
description: "Step 2 - genome-wide (rare variants)"
parent: genomewide-eQTL test
---


## Step 2: performing genome-wide set-based association tests. Multiple genes are tested simultaneously for computation speed-up

* Same as the genome-wide single-variant tests, --GMMATmodel_varianceRatio_multiTraits_File can be used  
* Additional input --groupFile= is needed with the following parameters to run set-based tests:

```
	--maxMAF_in_groupTest=0.1   \
        --minMAF_in_groupTest_Exclude=0       \
        --groupFile=${groupFile}        \
        --annotation_in_groupTest=null  \
        --MACCutoff_to_CollapseUltraRare=10     \
        --is_single_in_groupTest=TRUE  \
        --is_equal_weight_in_groupTest=TRUE    \
        --LOCO=FALSE    \
```

## Output files

The program will generate one output file for each of the tested genes. 

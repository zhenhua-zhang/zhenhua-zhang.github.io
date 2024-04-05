---
title: PLINK clump
date: 2022-02-15 09:32:21
tags: [GWAS,genetics]
categories: [GWAS]
---

## GWAS
GWAS全称为全基因组关联分析。简单点讲就是在基因组范围内找与某个疾病或性状相关的标记。这里的标记一般是SNP或者INDEL，即单核苷酸多态和短插入、删除。

## How to do clump by PLINK?

``` {bash}
sumstat=~/blog/GWAS/example_sumary_statistic.csv
refgeno=~/blog/GWAS/EUR

plink \
  --bfile $refgeno \
  --clump $sumstat \
  --clump-p1 5e-8 \
  --clump-p2 5e-2 \
  --clump-r2 0.5 \
  --clump-field p-value \
  --clump-snp-field rsid
```

Details about the parameters used in the command above.

- `--bfile [FILE-PREFIX]`, Reference genotype panel used to estimate linkage between each pair of SNPs.
- `--clump [FILE]`, The summary statistics to be clumped.
- `--clump-p1 [0-1.0]`, The maximum p-value of SNP that can be used to as a top/index SNP in a clump.
- `--clump-p2 [0-1.0]`，The maximum p-value of SNPs to be used in the clumping analysis.
- `--clump-r2 [0-1.0]`, If two SNPs has a LD R^2 statistic less than the argument, they are independent.
- `--clump-field [STRING]`, The column to be clumped on, usually it is a column contains p-values of the association.
- `--clump-snp-field [STRING]`, Unique SNP identifiers, e.g., rsid

## When do need to clump the summary statistics?

1. To report the top X single SNP results from a genome-wide scan in terms of a smaller number of clumps of correlated SNPs (i.e. to assess how many independent loci are associated, for example)
2. To provide a quick way to combine sets of results from two or more studies, when the studies might also be genotyped on different marker sets

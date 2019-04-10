# Estimating heritability using GCTA

### Software:

In this exercise we will be using GCTA. You can see the documentation [here](
http://cnsgenomics.com/software/gcta/#Download). We will be also using plink 1.9, you can see the documentation [here](https://www.cog-genomics.org/plink/1.9/).

### Exercise contents:

We will estimate the amount of variance explained by the SNPs in a GWAS dataset. You can find the data here:

```bash
/home/Data/GWAS_heritability
```
Copy the content of the directory to your home.

### Calculating the genetic relationship matrix

We will use plink to calculate the genetic relationship matrix (GRM) since it is faster than gcta. At the shell prompt, type:

```
plink --make-grm-gz --bfile gwa --out gwa
```

 This will save the genetic relationship matrix in the zipped file gwa.grm.gz. Try to read it into R:

```
d <- read.table(gzfile('gwa.grm.gz'))
```

*1) If you exclude the lines where an individual is compared to itself (column 1 is equal to column 2) what is the highest value in the GRM then?*


```
  V1   V2   V3     V4
1 1924 1921 300436 0.2067297
```

### Estimating variance components

We can use gcta to estimate how much of the variance in the phenotype in gwa.phen is explained by the SNPs:

```
gcta64 --grm-gz gwa --pheno gwa.phen --reml --out test
```

*2) How much of the phenotypic variance (Vp) is explained by the genetic variance (V(G))?*

```
Summary result of REML analysis:
Source  Variance        SE
V(G)    0.061298        0.088854
V(e)    0.188517        0.089298
Vp      0.249815        0.008025
V(G)/Vp 0.245373        0.355848
```

*3) Is this number larger or smaller than the narrow-sense heritability (h^2) of the trait?*

Since we used additive effects only on a smaller set of genes to compute V(G), it will be smaller than h^2.

### Estimating variance components for groups of SNPs

The estimation of variance components can be used to answer questions about how much of the heritability is explained by different parts of the genome (for example different chromosomes or different functional annotations).

 *4) How much of the phenotypic variance can be explained by the genetic variants on chromosome 6? (You can use the “--chr” flag in plink to build a GRM only using variants from a particular chromosome)*

Chr 6 explain 8% compared with 25%

*5) Does chromosome 6 contribute more to the heritability than would be expected? How many of the genetic variants in the data set are located on chr 6? (you can use the genetic map in gwa.bim to see the location of the variants).*

Chr 6 has 22289 variants (out of 301676) = 7%. So chr 6 explains a lot compared to others.

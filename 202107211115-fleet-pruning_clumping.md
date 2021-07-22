<https://privefl.github.io/bigsnpr/articles/pruning-vs-clumping.html>

Clumping on MAF versus pruning.

## Examples
* Pruning avoids too much variants in LD region during PCA.
* Clumping keeps one SNP per region of LD.

## Toy genotypes
* 500 individuals 
* 10 SNPs
* Each SNP has a squared correlation > 0.2 with their direct neighbors and only with them. 
* SNPs have an increasing MAF.

Squared correlation:

``` R
##       [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
##  [1,] 1.00 0.23 0.13 0.08 0.05 0.03 0.02 0.01 0.01  0.00
##  [2,] 0.23 1.00 0.21 0.09 0.05 0.05 0.01 0.01 0.01  0.01
##  [3,] 0.13 0.21 1.00 0.25 0.12 0.10 0.06 0.06 0.02  0.01
##  [4,] 0.08 0.09 0.25 1.00 0.31 0.16 0.07 0.07 0.03  0.04
##  [5,] 0.05 0.05 0.12 0.31 1.00 0.29 0.11 0.10 0.07  0.04
##  [6,] 0.03 0.05 0.10 0.16 0.29 1.00 0.25 0.17 0.08  0.06
##  [7,] 0.02 0.01 0.06 0.07 0.11 0.25 1.00 0.23 0.13  0.07
##  [8,] 0.01 0.01 0.06 0.07 0.10 0.17 0.23 1.00 0.28  0.14
##  [9,] 0.01 0.01 0.02 0.03 0.07 0.08 0.13 0.28 1.00  0.31
## [10,] 0.00 0.01 0.01 0.04 0.04 0.06 0.07 0.14 0.31  1.00
```

MAF:
``` R
##  [1] 0.043 0.091 0.154 0.177 0.234 0.283 0.338 0.372 0.436 0.450
```

## Pruning
* Scan for pairs of SNPs
* Keep the one with higher MAF
* SNP 10 is kept because it has highest MAF
* A threshold could be used to keep SNPs with cor below a certain level

## Clumping
* SNPs sorted by a value (e.g. MAF)
* The benefit of the clumping procedure is that the index SNP is always the SNP that is kept (because it has the highest MAF). 

``` R
##   CHR F    SNP    BP     P TOTAL NSIG S05 S01 S001 S0001      SP2
## 1   1 1 snp_10 10000 0.550     1    1   0   0    0     0 snp_9(1)
## 2   1 1  snp_8  8000 0.628     1    1   0   0    0     0 snp_7(1)
## 3   1 1  snp_6  6000 0.717     1    1   0   0    0     0 snp_5(1)
## 4   1 1  snp_4  4000 0.823     1    1   0   0    0     0 snp_3(1)
## 5   1 1  snp_2  2000 0.909     1    1   0   0    0     0 snp_1(1)
```

## Conclusion 
Pruning removes SNPs in a way that may leave regions of the genome with no representative SNP at all.


<https://mrcieu.github.io/gwasglue/articles/finemapping.html>

## Clumping
* An LD reference panel to identify SNPs that are in LD with the top signals from a GWAS. 
* Sequentially chooses the top SNP, removes all SNPs in LD above some threshold within some window, then goes on to the next top hit and repeats the pruning process, until no more SNPs are left above the specified p-value threshold.

<https://onlinelibrary.wiley.com/doi/full/10.1002/mpr.1608>
> Clumping: This is a procedure in which only the most significant SNP (i.e., lowest p value) in each LD block is identified and selected for further analyses. This reduces the correlation between the remaining SNPs, while retaining SNPs with the strongest statistical evidence.

> Pruning: This is a method to select a subset of markers that are in approximate linkage equilibrium. In PLINK, this method uses the strength of LD between SNPs within a specific window (region) of the chromosome and selects only SNPs that are approximately uncorrelated, based on a user-specified threshold of LD. In contrast to clumping, pruning does not take the p value of a SNP into account.

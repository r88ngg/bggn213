# Class 17: Sequence Analysis on the Cloud
Renny Ng (A98061553)

Where are my files/results?

``` r
dir(pattern="*_quant")
```

    [1] "SRR2156848_quant" "SRR2156849_quant" "SRR2156850_quant" "SRR2156851_quant"

``` r
# Directory of my results
folders <- dir(pattern="*_quant")
# Relative path to all the result files
files <- paste(folders, "/abundance.h5", sep="")
# Set the `names` as tximport uses these later
names(files) <- gsub("_quant", "", folders)
```

``` r
files
```

                         SRR2156848                      SRR2156849 
    "SRR2156848_quant/abundance.h5" "SRR2156849_quant/abundance.h5" 
                         SRR2156850                      SRR2156851 
    "SRR2156850_quant/abundance.h5" "SRR2156851_quant/abundance.h5" 

``` r
library(tximport)
library(rhdf5)
txi.kallisto <- tximport(files, type = "kallisto", txOut = TRUE)
```

    1 2 3 4 

``` r
head(txi.kallisto$counts)
```

                    SRR2156848 SRR2156849 SRR2156850 SRR2156851
    ENST00000539570          0          0    0.00000          0
    ENST00000576455          0          0    2.62037          0
    ENST00000510508          0          0    0.00000          0
    ENST00000474471          0          1    1.00000          0
    ENST00000381700          0          0    0.00000          0
    ENST00000445946          0          0    0.00000          0

``` r
colSums(txi.kallisto$counts)
```

    SRR2156848 SRR2156849 SRR2156850 SRR2156851 
       2563611    2600800    2372309    2111474 

``` r
sum(rowSums(txi.kallisto$counts)>0)
```

    [1] 94561

``` r
to.keep <- rowSums(txi.kallisto$counts) > 0
kset.nonzero <- txi.kallisto$counts[to.keep,]
```

``` r
keep2 <- apply(kset.nonzero,1,sd)>0
x <- kset.nonzero[keep2,]
```

``` r
pca <- prcomp(t(x), scale=TRUE)
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3   PC4
    Standard deviation     183.6379 177.3605 171.3020 1e+00
    Proportion of Variance   0.3568   0.3328   0.3104 1e-05
    Cumulative Proportion    0.3568   0.6895   1.0000 1e+00

``` r
plot(pca$x[,1], pca$x[,2],
     col=c("blue","blue","red","red"),
     xlab="PC1", ylab="PC2", pch=16)
```

![](Class-17_files/figure-commonmark/unnamed-chunk-11-1.png)

``` r
library(ggplot2)
library(ggrepel)
```

``` r
colData <- data.frame(condition = factor(rep(c("control", "treatment"), each = 2)))
rownames(colData) <- colnames(txi.kallisto$counts)
```

``` r
y <- as.data.frame(pca$x)
y$Condition <- as.factor(colData$condition)
```

``` r
ggplot(y) +
  aes(PC1, PC2, col=Condition) +
  geom_point() +
  geom_text_repel(label=rownames(y)) +
  theme_bw()
```

![](Class-17_files/figure-commonmark/unnamed-chunk-15-1.png)

``` r
ggplot(y) +
  aes(PC1, PC3, col=Condition) +
  geom_point() +
  geom_text_repel(label=rownames(y)) +
  theme_bw()
```

![](Class-17_files/figure-commonmark/unnamed-chunk-16-1.png)

``` r
ggplot(y) +
  aes(PC2, PC3, col=Condition) +
  geom_point() +
  geom_text_repel(label=rownames(y)) +
  theme_bw()
```

![](Class-17_files/figure-commonmark/unnamed-chunk-17-1.png)

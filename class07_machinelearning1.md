# Class 7: Machine Learning 1
Renny (PID: A98061553)

Today we are going to explore some core machine learning methods. Namely
clustering and dimensionality reduction.

# Kmeans clustering

The main function for k-means in “base” R is called `kmeans()`. Let’s
first make up some data to see how kmeans works and to get at the
results.

``` r
#`rnorm()` function creates a set of data.
# Using a histogram should show a normal distribution of numbers.
# Setting a mean will designate where the data centers around. 
hist(rnorm(50000, mean = 3))
```

![](class07_machinelearning1_files/figure-commonmark/unnamed-chunk-1-1.png)

Make a vector named `tmp` with 60 total points half centered at +3, and
half centered at -3.

``` r
tmp <- c(rnorm(30, mean=3), rnorm(30, mean=-3))
#concatenate with c()
tmp
```

     [1]  1.5924013  3.1102570  3.6334687  3.1483945  5.1112814  4.5216948
     [7]  1.5507065  2.0395685  3.1357334  3.3139560  3.0004932  1.3827426
    [13]  4.1195189  2.1039871  3.3243225  1.1242263  3.1352249  3.9671126
    [19]  2.7501938  2.7207257  0.8034541  3.2455727  2.1083551  2.4697615
    [25]  4.0076692  4.1613176  3.3456515  3.9956341  2.0829581  1.9877611
    [31] -0.9095373 -3.4300142 -4.4823959 -3.3308957 -1.6890957 -2.7062837
    [37] -1.5861190 -4.6182096 -3.7726932 -4.0979296 -2.5517628 -2.2143727
    [43] -1.7664668 -1.8963554 -1.5424718 -3.5614948 -1.8149551 -2.7529046
    [49] -2.1080431 -3.9329829 -1.9683040 -3.8859590 -3.3891089 -2.6518761
    [55] -3.4440078 -2.4613014 -4.2890199 -4.0544579 -2.9772315 -2.5623124

Reverse tmp using the reverse function `rev()`

``` r
x <- cbind(x=tmp, y=rev(tmp))
x
```

                   x          y
     [1,]  1.5924013 -2.5623124
     [2,]  3.1102570 -2.9772315
     [3,]  3.6334687 -4.0544579
     [4,]  3.1483945 -4.2890199
     [5,]  5.1112814 -2.4613014
     [6,]  4.5216948 -3.4440078
     [7,]  1.5507065 -2.6518761
     [8,]  2.0395685 -3.3891089
     [9,]  3.1357334 -3.8859590
    [10,]  3.3139560 -1.9683040
    [11,]  3.0004932 -3.9329829
    [12,]  1.3827426 -2.1080431
    [13,]  4.1195189 -2.7529046
    [14,]  2.1039871 -1.8149551
    [15,]  3.3243225 -3.5614948
    [16,]  1.1242263 -1.5424718
    [17,]  3.1352249 -1.8963554
    [18,]  3.9671126 -1.7664668
    [19,]  2.7501938 -2.2143727
    [20,]  2.7207257 -2.5517628
    [21,]  0.8034541 -4.0979296
    [22,]  3.2455727 -3.7726932
    [23,]  2.1083551 -4.6182096
    [24,]  2.4697615 -1.5861190
    [25,]  4.0076692 -2.7062837
    [26,]  4.1613176 -1.6890957
    [27,]  3.3456515 -3.3308957
    [28,]  3.9956341 -4.4823959
    [29,]  2.0829581 -3.4300142
    [30,]  1.9877611 -0.9095373
    [31,] -0.9095373  1.9877611
    [32,] -3.4300142  2.0829581
    [33,] -4.4823959  3.9956341
    [34,] -3.3308957  3.3456515
    [35,] -1.6890957  4.1613176
    [36,] -2.7062837  4.0076692
    [37,] -1.5861190  2.4697615
    [38,] -4.6182096  2.1083551
    [39,] -3.7726932  3.2455727
    [40,] -4.0979296  0.8034541
    [41,] -2.5517628  2.7207257
    [42,] -2.2143727  2.7501938
    [43,] -1.7664668  3.9671126
    [44,] -1.8963554  3.1352249
    [45,] -1.5424718  1.1242263
    [46,] -3.5614948  3.3243225
    [47,] -1.8149551  2.1039871
    [48,] -2.7529046  4.1195189
    [49,] -2.1080431  1.3827426
    [50,] -3.9329829  3.0004932
    [51,] -1.9683040  3.3139560
    [52,] -3.8859590  3.1357334
    [53,] -3.3891089  2.0395685
    [54,] -2.6518761  1.5507065
    [55,] -3.4440078  4.5216948
    [56,] -2.4613014  5.1112814
    [57,] -4.2890199  3.1483945
    [58,] -4.0544579  3.6334687
    [59,] -2.9772315  3.1102570
    [60,] -2.5623124  1.5924013

plot(x)

``` r
plot(x)
```

![](class07_machinelearning1_files/figure-commonmark/unnamed-chunk-4-1.png)

Try k-means clustering by running `kmeans()` asking for two clusters:

``` r
k <- kmeans(x, centers=2, nstart=20)
k
```

    K-means clustering with 2 clusters of sizes 30, 30

    Cluster means:
              x         y
    1 -2.881619  2.899805
    2  2.899805 -2.881619

    Clustering vector:
     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

    Within cluster sum of squares by cluster:
    [1] 62.08973 62.08973
     (between_SS / total_SS =  89.0 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

What is in this result object? Check with `attributes()`.

``` r
attributes(k)
```

    $names
    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

    $class
    [1] "kmeans"

Can check individual attributes using function\$attribute.

``` r
k$centers
```

              x         y
    1 -2.881619  2.899805
    2  2.899805 -2.881619

What is my clustering result? I.e. what cluster does each point reside
in? Use \$cluster to check.

``` r
k$cluster
```

     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

> Q. Plot your data `x` showing your clustering results and the center
> point for each cluster.

``` r
#Plot by k$cluster, which shows two distinct clusters. 
plot(x, col = k$cluster)
points(k$centers, pch=15, col="green")
```

![](class07_machinelearning1_files/figure-commonmark/unnamed-chunk-9-1.png)

``` r
# `points()` function will add points to an existing plot.
# Don't need to add `+`, not necessary in base R; automatically adds code together. 
```

> Q. Run kmeans and cluster into 3 groups.

``` r
# Can trick you into thinking the data is a certain way. There will always be an output but it may not be correct. 
k3 <- kmeans(x, centers=3, nstart=20)
plot(x, col=k3$cluster)
```

![](class07_machinelearning1_files/figure-commonmark/unnamed-chunk-10-1.png)

``` r
# Sum of squares value gets smaller with increasing number of clusters. 
k$tot.withinss
```

    [1] 124.1795

``` r
k3$tot.withinss
```

    [1] 98.56976

The main limitation of k-means clustering (though it is very often
employed) is that it imposes a structure on data (i.e. a clustering)
that you ask for in the first place, even if that structure is not
there.

# Hierarchical Clustering

The main function in “base” R for this is called `hclust()`. It wants a
distance matrix as input, not the data itself. hclust measures the
dissimilarities as produced by distance.

We can calculate a distance matrix in multiple ways, but we will use the
`dist()` function. Distance is by default measured as Euclidean
distance).

`hclust(dist(x))`

``` r
#`dist()` takes the distance from all points, to build up a table.
d <- dist(x)
hc <- hclust(d)
hc
```


    Call:
    hclust(d = d)

    Cluster method   : complete 
    Distance         : euclidean 
    Number of objects: 60 

``` r
plot(hc)
# Note that there are no relationships between the labels. 
# From this bottom-up approach, the thing that matters is the "crossbar"; it's this part that is important. The crossbar shows the distance where points are joined together. 
abline(h=9, col="red")
```

![](class07_machinelearning1_files/figure-commonmark/unnamed-chunk-13-1.png)

To get the cluster membership vector (equivalent of k\$cluster for
k-means) we need to “cut” the tree at a given height that we choose. The
function is called `cutree()`.

``` r
cutree(hc, h=9)
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

When choosing not to pick a certain height, and want to instead cut at
values when there are certain clusters, then indicate that with `k`.

``` r
cutree(hc, k=4)
```

     [1] 1 2 2 2 2 2 1 1 2 2 2 1 2 1 2 1 2 2 2 2 1 2 1 1 2 2 2 2 1 1 3 3 4 4 4 4 3 3
    [39] 4 3 4 4 4 4 3 4 3 4 3 4 4 4 3 3 4 4 4 4 4 3

``` r
grps <- cutree(hc, k=2)
grps
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

> Q. Plot our data (‘x’) colored by our hclust result.

``` r
plot(x, col=grps)
```

![](class07_machinelearning1_files/figure-commonmark/unnamed-chunk-17-1.png)

# Principal Component Analysis

We will start with PCA of a tiny dataset of food data.

``` r
url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url, row.names=1)
x
```

                        England Wales Scotland N.Ireland
    Cheese                  105   103      103        66
    Carcass_meat            245   227      242       267
    Other_meat              685   803      750       586
    Fish                    147   160      122        93
    Fats_and_oils           193   235      184       209
    Sugars                  156   175      147       139
    Fresh_potatoes          720   874      566      1033
    Fresh_Veg               253   265      171       143
    Other_Veg               488   570      418       355
    Processed_potatoes      198   203      220       187
    Processed_Veg           360   365      337       334
    Fresh_fruit            1102  1137      957       674
    Cereals                1472  1582     1462      1494
    Beverages                57    73       53        47
    Soft_drinks            1374  1256     1572      1506
    Alcoholic_drinks        375   475      458       135
    Confectionery            54    64       62        41

One useful plot in this case (because we only have 4 countries to look
across) is a so-called “pairs plot”.

``` r
pairs(x, col=rainbow(10), pch=16)
```

![](class07_machinelearning1_files/figure-commonmark/unnamed-chunk-19-1.png)

## Enter PCA

The main function to do PCA in “base” R is called `prcomp()`.

It wants our variables as the columns (e.g. the foods in this case) and
the countries as the rows. The data will need to be transposed.

``` r
pca <- prcomp(t(x))
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3       PC4
    Standard deviation     324.1502 212.7478 73.87622 2.921e-14
    Proportion of Variance   0.6744   0.2905  0.03503 0.000e+00
    Cumulative Proportion    0.6744   0.9650  1.00000 1.000e+00

``` r
attributes(pca)
```

    $names
    [1] "sdev"     "rotation" "center"   "scale"    "x"       

    $class
    [1] "prcomp"

``` r
pca$x
```

                     PC1         PC2        PC3           PC4
    England   -144.99315   -2.532999 105.768945 -9.152022e-15
    Wales     -240.52915 -224.646925 -56.475555  5.560040e-13
    Scotland   -91.86934  286.081786 -44.415495 -6.638419e-13
    N.Ireland  477.39164  -58.901862  -4.877895  1.329771e-13

``` r
#Gives the values for the new values
```

``` r
plot(pca$x[,1], pca$x[,2], xlab="PC1 (67.4%)", ylab="PC2(29%)",
     col = c("orange", "red", "blue", "darkgreen"))
abline(h=0, col="gray", lty=2)
abline(v=0, col="gray", lty=2)
```

![](class07_machinelearning1_files/figure-commonmark/unnamed-chunk-23-1.png)

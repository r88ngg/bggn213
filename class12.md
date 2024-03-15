# Class 12
Renny Ng (A98061553)

Reading in Sample Genotype MXL data for SNP rs8067378.

``` r
rs8067378 <- read.csv("SNP Sample.csv")
attributes(rs8067378)
```

    $names
    [1] "Sample..Male.Female.Unknown." "Genotype..forward.strand."   
    [3] "Population.s."                "Father"                      
    [5] "Mother"                      

    $class
    [1] "data.frame"

    $row.names
     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50
    [51] 51 52 53 54 55 56 57 58 59 60 61 62 63 64

``` r
SNPgeno <- list(rs8067378$"Genotype..forward.strand.")
table(SNPgeno)
```

    SNPgeno
    A|A A|G G|A G|G 
     22  21  12   9 

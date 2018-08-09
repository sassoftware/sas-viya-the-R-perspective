Chapter 5 - First Steps with the CASTable Object
================
Yue Qi

``` r
knitr::opts_chunk$set()
```

Getting Started with Caslibs and CAS Tables
-------------------------------------------

Import the SWAT package.

``` r
library("swat")
```

    ## NOTE: The extension module for binary protocol support is not available.

    ##       Only the CAS REST interface can be used.

    ## SWAT 1.3.0

``` r
conn <- CAS('rdcgrdc.unx.sas.com', 39935)
```

    ## NOTE: Connecting to CAS and generating CAS action functions for loaded

    ##       action sets...

    ## NOTE: To generate the functions with signatures (for tab completion), set

    ##       options(cas.gen.function.sig=TRUE).

``` r
cas.sessionProp.setSessOpt(conn, caslib = "casuser")
```

    ## NOTE: 'CASUSER(sasyqi)' is now the active caslib.

    ## list()

``` r
out <- cas.table.loadTable(conn, path='data/iris.csv', caslib='casuser')
```

    ## NOTE: Cloud Analytic Services made the file data/iris.csv available as table DATA.IRIS in caslib CASUSER(sasyqi).

``` r
out
```

    ## $caslib
    ## [1] "CASUSER(sasyqi)"
    ## 
    ## $tableName
    ## [1] "DATA.IRIS"

``` r
out$caslib
```

    ## [1] "CASUSER(sasyqi)"

``` r
out[['tableName']]
```

    ## [1] "DATA.IRIS"

``` r
out['tableName']
```

    ## $tableName
    ## [1] "DATA.IRIS"

Create the CASTable object manually

``` r
newtbl <- defCasTable(conn, 'data.iris')
```

Verify the result

``` r
head(newtbl)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width     Species
    ## 1          5.1         3.5          1.4         0.2 Iris-setosa
    ## 2          4.9         3.0          1.4         0.2 Iris-setosa
    ## 3          4.7         3.2          1.3         0.2 Iris-setosa
    ## 4          4.6         3.1          1.5         0.2 Iris-setosa
    ## 5          5.0         3.6          1.4         0.2 Iris-setosa
    ## 6          5.4         3.9          1.7         0.4 Iris-setosa

``` r
cas.table.columnInfo(conn, table = 'data.iris')
```

    ## $ColumnInfo
    ##         Column ID    Type RawLength FormattedLength NFL NFD
    ## 1 Sepal.Length  1  double         8              12   0   0
    ## 2  Sepal.Width  2  double         8              12   0   0
    ## 3 Petal.Length  3  double         8              12   0   0
    ## 4  Petal.Width  4  double         8              12   0   0
    ## 5      Species  5 varchar        15              15   0   0

``` r
cas.table.fetch(conn, table = 'data.iris', to = 5)
```

    ## $Fetch
    ##   _Index_ Sepal.Length Sepal.Width Petal.Length Petal.Width     Species
    ## 1       1          5.1         3.5          1.4         0.2 Iris-setosa
    ## 2       2          4.9         3.0          1.4         0.2 Iris-setosa
    ## 3       3          4.7         3.2          1.3         0.2 Iris-setosa
    ## 4       4          4.6         3.1          1.5         0.2 Iris-setosa
    ## 5       5          5.0         3.6          1.4         0.2 Iris-setosa

``` r
cas.table.columnInfo(newtbl)
```

    ## $ColumnInfo
    ##         Column ID    Type RawLength FormattedLength NFL NFD
    ## 1 Sepal.Length  1  double         8              12   0   0
    ## 2  Sepal.Width  2  double         8              12   0   0
    ## 3 Petal.Length  3  double         8              12   0   0
    ## 4  Petal.Width  4  double         8              12   0   0
    ## 5      Species  5 varchar        15              15   0   0

``` r
cas.table.fetch(newtbl, to = 5)
```

    ## $Fetch
    ##   _Index_ Sepal.Length Sepal.Width Petal.Length Petal.Width     Species
    ## 1       1          5.1         3.5          1.4         0.2 Iris-setosa
    ## 2       2          4.9         3.0          1.4         0.2 Iris-setosa
    ## 3       3          4.7         3.2          1.3         0.2 Iris-setosa
    ## 4       4          4.6         3.1          1.5         0.2 Iris-setosa
    ## 5       5          5.0         3.6          1.4         0.2 Iris-setosa

``` r
cas.simple.summary(newtbl)
```

    ## $Summary
    ##         Column Min Max   N NMiss     Mean   Sum       Std     StdErr
    ## 1 Sepal.Length 4.3 7.9 150     0 5.843333 876.5 0.8280661 0.06761132
    ## 2  Sepal.Width 2.0 4.4 150     0 3.054000 458.1 0.4335943 0.03540283
    ## 3 Petal.Length 1.0 6.9 150     0 3.758667 563.8 1.7644204 0.14406432
    ## 4  Petal.Width 0.1 2.5 150     0 1.198667 179.8 0.7631607 0.06231181
    ##         Var     USS       CSS       CV   TValue         ProbT   Skewness
    ## 1 0.6856935 5223.85 102.16833 14.17113 86.42537 3.331256e-129  0.3149110
    ## 2 0.1880040 1427.05  28.01260 14.19759 86.26430 4.374977e-129  0.3340527
    ## 3 3.1131794 2583.00 463.86373 46.94272 26.09020  1.994305e-57 -0.2744643
    ## 4 0.5824143  302.30  86.77973 63.66747 19.23659  3.209704e-42 -0.1049966
    ##     Kurtosis
    ## 1 -0.5520640
    ## 2  0.2907811
    ## 3 -1.4019208
    ## 4 -1.3397542

``` r
cas.simple.correlation(newtbl)
```

    ## $CorrSimple
    ##       Variable   N     Mean   Sum    StdDev Minimum Maximum
    ## 1 Sepal.Length 150 5.843333 876.5 0.8280661     4.3     7.9
    ## 2  Sepal.Width 150 3.054000 458.1 0.4335943     2.0     4.4
    ## 3 Petal.Length 150 3.758667 563.8 1.7644204     1.0     6.9
    ## 4  Petal.Width 150 1.198667 179.8 0.7631607     0.1     2.5
    ## 
    ## $Correlation
    ##       Variable Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## 1 Sepal.Length    1.0000000  -0.1093692    0.8717542   0.8179536
    ## 2  Sepal.Width   -0.1093692   1.0000000   -0.4205161  -0.3565441
    ## 3 Petal.Length    0.8717542  -0.4205161    1.0000000   0.9627571
    ## 4  Petal.Width    0.8179536  -0.3565441    0.9627571   1.0000000

``` r
names(newtbl)
```

    ## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
    ## [5] "Species"

``` r
summary(newtbl)
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

    ## Selecting by Frequency

    ## Warning in filter_impl(.data, quo): hybrid evaluation forced for
    ## `row_number`. Please use dplyr::row_number() or library(dplyr) to remove
    ## this warning.

    ##   Sepal.Length    Sepal.Width     Petal.Length    Petal.Width   
    ##  Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100  
    ##  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300  
    ##  Median :5.800   Median :3.000   Median :4.350   Median :1.300  
    ##  Mean   :5.843   Mean   :3.054   Mean   :3.759   Mean   :1.199  
    ##  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800  
    ##  Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500  
    ##             Species  
    ##  Iris-setosa    :50  
    ##  Iris-versicolor:50  
    ##  Iris-virginica :50  
    ##                      
    ##                      
    ## 

Setting CASTable Parameters
===========================

``` r
attributes(newtbl)
```

    ## $conn
    ## CAS(hostname=rdcgrdc.unx.sas.com, port=39935, username=sasyqi, session=820b1d39-d795-104d-8644-21fc929fb1a8, protocol=http)
    ## 
    ## $tname
    ## [1] "data.iris"
    ## 
    ## $caslib
    ## [1] ""
    ## 
    ## $where
    ## [1] ""
    ## 
    ## $orderby
    ## list()
    ## 
    ## $groupby
    ## list()
    ## 
    ## $gbmode
    ## [1] ""
    ## 
    ## $computedOnDemand
    ## [1] FALSE
    ## 
    ## $computedVars
    ## [1] ""
    ## 
    ## $computedVarsProgram
    ## [1] ""
    ## 
    ## $XcomputedVarsProgram
    ## [1] ""
    ## 
    ## $XcomputedVars
    ## [1] ""
    ## 
    ## $names
    ## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
    ## [5] "Species"     
    ## 
    ## $compcomp
    ## [1] FALSE
    ## 
    ## $class
    ## [1] "CASTable"
    ## attr(,"package")
    ## [1] "swat"

``` r
iristbl <- defCasTable(
  conn, 'data.iris', 
  caslib = 'casuser',
  where = '"Sepal.Length"n > 6.8 and Species = "Iris-virginica"'
  )
```

``` r
cas.simple.summary(
  iristbl, 
  casout = list(name = 'summout', caslib = 'casuser', promote = TRUE)
  )
```

    ## $OutputCasTables
    ##            casLib    Name Rows Columns
    ## 1 CASUSER(sasyqi) summout    4      17

``` r
outtbl = defCasTable(conn, caslib = 'casuser', 'SUMMOUT')
```

``` r
cas.table.fetch(outtbl)
```

    ## $Fetch
    ##   _Index_     _Column_ _Min_ _Max_ _NObs_ _NMiss_   _Mean_ _Sum_     _Std_
    ## 1       1 Sepal.Length   6.9   7.9     15       0 7.360000 110.4 0.3376389
    ## 2       2  Sepal.Width   2.6   3.8     15       0 3.126667  46.9 0.3534860
    ## 3       3 Petal.Length   5.1   6.9     15       0 6.120000  91.8 0.5017114
    ## 4       4  Petal.Width   1.6   2.5     15       0 2.086667  31.3 0.2416215
    ##     _StdErr_      _Var_  _USS_     _CSS_      _CV_      _T_        _PRT_
    ## 1 0.08717798 0.11400000 814.14 1.5960000  4.587485 84.42499 2.332581e-20
    ## 2 0.09126970 0.12495238 148.39 1.7493333 11.305524 34.25744 6.662768e-15
    ## 3 0.12954132 0.25171429 565.34 3.5240000  8.197898 47.24361 7.680656e-17
    ## 4 0.06238640 0.05838095  66.13 0.8173333 11.579305 33.44746 9.278838e-15
    ##    _Skewness_ _Kurtosis_
    ## 1  0.01387515 -1.3790042
    ## 2  0.89896300  0.2055943
    ## 3 -0.35930456 -0.2101472
    ## 4 -0.34630294 -0.2688732

``` r
cas.table.dropTable(conn, name = "summout")
```

    ## NOTE: Cloud Analytic Services dropped table summout from caslib CASUSER(sasyqi).

    ## list()

Managing Parameters Using the Attribute Interface
=================================================

``` r
iristbl <- defCasTable(conn, 'data.iris', caslib = 'casuser')
```

``` r
dim(iristbl)
```

    ## [1] 150   5

``` r
iristbl@where = '"Sepal.Length"n > 6.8 and Species = "Iris-virginica"'
```

``` r
dim(iristbl)
```

    ## [1] 15  5

``` r
iristbl@groupby <- list("Species", "Sepal.Length")
```

``` r
iristbl@groupby[[1]] <- "Species"
```

``` r
iristbl@groupby[[2]] <- "Sepal.Length"
```

``` r
attributes(iristbl)
```

    ## $conn
    ## CAS(hostname=rdcgrdc.unx.sas.com, port=39935, username=sasyqi, session=820b1d39-d795-104d-8644-21fc929fb1a8, protocol=http)
    ## 
    ## $tname
    ## [1] "data.iris"
    ## 
    ## $caslib
    ## [1] "casuser"
    ## 
    ## $where
    ## [1] "\"Sepal.Length\"n > 6.8 and Species = \"Iris-virginica\""
    ## 
    ## $orderby
    ## list()
    ## 
    ## $groupby
    ## $groupby[[1]]
    ## [1] "Species"
    ## 
    ## $groupby[[2]]
    ## [1] "Sepal.Length"
    ## 
    ## 
    ## $gbmode
    ## [1] ""
    ## 
    ## $computedOnDemand
    ## [1] FALSE
    ## 
    ## $computedVars
    ## [1] ""
    ## 
    ## $computedVarsProgram
    ## [1] ""
    ## 
    ## $XcomputedVarsProgram
    ## [1] ""
    ## 
    ## $XcomputedVars
    ## [1] ""
    ## 
    ## $names
    ## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
    ## [5] "Species"     
    ## 
    ## $compcomp
    ## [1] FALSE
    ## 
    ## $class
    ## [1] "CASTable"
    ## attr(,"package")
    ## [1] "swat"

``` r
iristbl@groupby
```

    ## [[1]]
    ## [1] "Species"
    ## 
    ## [[2]]
    ## [1] "Sepal.Length"

``` r
iristbl@groupby <- list()
```

``` r
iristbl@where <- ''
```

``` r
attributes(iristbl)
```

    ## $conn
    ## CAS(hostname=rdcgrdc.unx.sas.com, port=39935, username=sasyqi, session=820b1d39-d795-104d-8644-21fc929fb1a8, protocol=http)
    ## 
    ## $tname
    ## [1] "data.iris"
    ## 
    ## $caslib
    ## [1] "casuser"
    ## 
    ## $where
    ## [1] ""
    ## 
    ## $orderby
    ## list()
    ## 
    ## $groupby
    ## list()
    ## 
    ## $gbmode
    ## [1] ""
    ## 
    ## $computedOnDemand
    ## [1] FALSE
    ## 
    ## $computedVars
    ## [1] ""
    ## 
    ## $computedVarsProgram
    ## [1] ""
    ## 
    ## $XcomputedVarsProgram
    ## [1] ""
    ## 
    ## $XcomputedVars
    ## [1] ""
    ## 
    ## $names
    ## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
    ## [5] "Species"     
    ## 
    ## $compcomp
    ## [1] FALSE
    ## 
    ## $class
    ## [1] "CASTable"
    ## attr(,"package")
    ## [1] "swat"

``` r
iristbl@groupby <- c('Species', 'Sepal.Length')
```

``` r
iristbl@computedVars <- c('Length.Factor')
```

``` r
iristbl@computedVarsProgram <- 'Length.Factor = Sepal.Length * Petal.Length'
```

``` r
attributes(iristbl)
```

    ## $conn
    ## CAS(hostname=rdcgrdc.unx.sas.com, port=39935, username=sasyqi, session=820b1d39-d795-104d-8644-21fc929fb1a8, protocol=http)
    ## 
    ## $tname
    ## [1] "data.iris"
    ## 
    ## $caslib
    ## [1] "casuser"
    ## 
    ## $where
    ## [1] ""
    ## 
    ## $orderby
    ## list()
    ## 
    ## $groupby
    ## [1] "Species"      "Sepal.Length"
    ## 
    ## $gbmode
    ## [1] ""
    ## 
    ## $computedOnDemand
    ## [1] FALSE
    ## 
    ## $computedVars
    ## [1] "Length.Factor"
    ## 
    ## $computedVarsProgram
    ## [1] "Length.Factor = Sepal.Length * Petal.Length"
    ## 
    ## $XcomputedVarsProgram
    ## [1] ""
    ## 
    ## $XcomputedVars
    ## [1] ""
    ## 
    ## $names
    ## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
    ## [5] "Species"     
    ## 
    ## $compcomp
    ## [1] FALSE
    ## 
    ## $class
    ## [1] "CASTable"
    ## attr(,"package")
    ## [1] "swat"

``` r
# Use the fetchvars= parameter to only fetch specified columns
cas.table.fetch(iristbl, fetchvars = c('Sepal.Length', 'Petal.Length', 'Length.Factor'))
```

    ## $Fetch
    ##    _Index_ Sepal.Length Petal.Length Length.Factor
    ## 1        1          5.1          1.4          7.14
    ## 2        2          4.9          1.4          6.86
    ## 3        3          4.7          1.3          6.11
    ## 4        4          4.6          1.5          6.90
    ## 5        5          5.0          1.4          7.00
    ## 6        6          5.4          1.7          9.18
    ## 7        7          4.6          1.4          6.44
    ## 8        8          5.0          1.5          7.50
    ## 9        9          4.4          1.4          6.16
    ## 10      10          4.9          1.5          7.35
    ## 11      11          5.4          1.5          8.10
    ## 12      12          4.8          1.6          7.68
    ## 13      13          4.8          1.4          6.72
    ## 14      14          4.3          1.1          4.73
    ## 15      15          5.8          1.2          6.96
    ## 16      16          5.7          1.5          8.55
    ## 17      17          5.4          1.3          7.02
    ## 18      18          5.1          1.4          7.14
    ## 19      19          5.7          1.7          9.69
    ## 20      20          5.1          1.5          7.65

Materializing CASTable Parameters
=================================

``` r
iristbl <- defCasTable(
  conn, 'data.iris', 
  caslib = 'casuser',
  where = '"Sepal.Length"n > 6.8 and Species = "Iris-virginica"'
  )
```

``` r
sub_iris <- cas.table.partition(iristbl)
```

``` r
sub_iris
```

    ## $averageShuffleWaitTime
    ## [1] 9.536743e-07
    ## 
    ## $caslib
    ## [1] "CASUSER(sasyqi)"
    ## 
    ## $maxShuffleWaitTime
    ## [1] 9.536743e-07
    ## 
    ## $minShuffleWaitTime
    ## [1] 9.536743e-07
    ## 
    ## $rowsTransferred
    ## [1] 0
    ## 
    ## $shuffleWaitTime
    ## [1] 4.768372e-06
    ## 
    ## $tableName
    ## [1] "_T_VEIYRXQS_EV4U7FKI_RNALBG6NFW"

``` r
sub_iris <- defCasTable(conn, sub_iris$tableName)
```

``` r
cas.table.fetch(sub_iris)
```

    ## $Fetch
    ##    _Index_ Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## 1        1          7.1         3.0          5.9         2.1
    ## 2        2          7.6         3.0          6.6         2.1
    ## 3        3          7.3         2.9          6.3         1.8
    ## 4        4          7.2         3.6          6.1         2.5
    ## 5        5          7.7         3.8          6.7         2.2
    ## 6        6          7.7         2.6          6.9         2.3
    ## 7        7          6.9         3.2          5.7         2.3
    ## 8        8          7.7         2.8          6.7         2.0
    ## 9        9          7.2         3.2          6.0         1.8
    ## 10      10          7.2         3.0          5.8         1.6
    ## 11      11          7.4         2.8          6.1         1.9
    ## 12      12          7.9         3.8          6.4         2.0
    ## 13      13          7.7         3.0          6.1         2.3
    ## 14      14          6.9         3.1          5.4         2.1
    ## 15      15          6.9         3.1          5.1         2.3
    ##           Species
    ## 1  Iris-virginica
    ## 2  Iris-virginica
    ## 3  Iris-virginica
    ## 4  Iris-virginica
    ## 5  Iris-virginica
    ## 6  Iris-virginica
    ## 7  Iris-virginica
    ## 8  Iris-virginica
    ## 9  Iris-virginica
    ## 10 Iris-virginica
    ## 11 Iris-virginica
    ## 12 Iris-virginica
    ## 13 Iris-virginica
    ## 14 Iris-virginica
    ## 15 Iris-virginica

``` r
cas.simple.numRows(sub_iris)
```

    ## $numrows
    ## [1] 15

Chapter 6 - Working with CAS Tables
================
Yue Qi

``` r
knitr::opts_chunk$set()
```

Import the SWAT package and get connected.

``` r
library("swat")
```

    ## NOTE: The extension module for binary protocol support is not available.

    ##       Only the CAS REST interface can be used.

    ## SWAT 1.3.0

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 3.4.4

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

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

Using CASTable Objects like a Data Frame
========================================

CAS Table Introspection
-----------------------

check the class of iris data to make sure it is a data.frame instance

``` r
class(iris)
```

    ## [1] "data.frame"

Get the column names

``` r
names(iris)
```

    ## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
    ## [5] "Species"

Load iris data.frame into a CAS table

``` r
tbl <- as.casTable(conn, iris)
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table IRIS in caslib CASUSER(sasyqi).

Show the class of tbl object

``` r
class(tbl)
```

    ## [1] "CASTable"
    ## attr(,"package")
    ## [1] "swat"

``` r
names(tbl)
```

    ## [1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width" 
    ## [5] "Species"

``` r
nrow(tbl)
```

    ## [1] 150

``` r
dim(tbl)
```

    ## [1] 150   5

``` r
attributes(tbl)
```

    ## $conn
    ## CAS(hostname=rdcgrdc.unx.sas.com, port=39935, username=sasyqi, session=eecfe36c-23b3-084c-bbb6-a477f078a38d, protocol=http)
    ## 
    ## $tname
    ## [1] "IRIS"
    ## 
    ## $caslib
    ## [1] "CASUSER(sasyqi)"
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
head(tbl, n = 3L)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa

``` r
tail(tbl, n = 3L)
```

    ##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 148          6.5         3.0          5.2         2.0 virginica
    ## 149          6.2         3.4          5.4         2.3 virginica
    ## 150          5.9         3.0          5.1         1.8 virginica

Computing Simple Statistics
---------------------------

Run the summary function on a data.frame

``` r
summ <- summary(iris)
summ
```

    ##   Sepal.Length    Sepal.Width     Petal.Length    Petal.Width   
    ##  Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100  
    ##  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300  
    ##  Median :5.800   Median :3.000   Median :4.350   Median :1.300  
    ##  Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199  
    ##  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800  
    ##  Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500  
    ##        Species  
    ##  setosa    :50  
    ##  versicolor:50  
    ##  virginica :50  
    ##                 
    ##                 
    ## 

``` r
class(summ)
```

    ## [1] "table"

Run the summary function on a CASTable

``` r
cassumm <- summary(tbl)
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

    ## Selecting by Frequency

``` r
cassumm
```

    ##   Sepal.Length    Sepal.Width     Petal.Length    Petal.Width   
    ##  Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100  
    ##  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300  
    ##  Median :5.800   Median :3.000   Median :4.350   Median :1.300  
    ##  Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199  
    ##  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800  
    ##  Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500  
    ##        Species  
    ##  setosa    :50  
    ##  versicolor:50  
    ##  virginica :50  
    ##                 
    ##                 
    ## 

``` r
class(cassumm)
```

    ## [1] "table"

``` r
cas.sessionProp.setSessOpt(conn,caslib="HPS")
```

    ## NOTE: 'HPS' is now the active caslib.

    ## list()

``` r
out  <- cas.table.loadTable(conn, path = 'megacorp5m.sashdat',casOut=list(replace = TRUE) )
```

    ## NOTE: Cloud Analytic Services made the HDFS file megacorp5m.sashdat available as table MEGACORP5M in caslib HPS.

``` r
cas.table.tableInfo(conn, 'MEGACORP5M')
```

    ## $TableInfo
    ##         Name                                      Label     Rows Columns
    ## 1 MEGACORP5M MegaCorp Production Data - 70 Million rows 70732833      46
    ##   IndexedColumns Encoding       CreateTimeFormatted
    ## 1              0   latin1 2018-07-26T00:09:04-04:00
    ##            ModTimeFormatted       AccessTimeFormatted JavaCharSet
    ## 1 2018-07-26T00:09:04-04:00 2018-07-26T00:09:04-04:00   ISO8859_1
    ##   CreateTime    ModTime AccessTime Global Repeated View         SourceName
    ## 1 1848197344 1848197344 1848197344      0        0    0 megacorp5m.sashdat
    ##   SourceCaslib Compressed Creator Modifier    SourceModTimeFormatted
    ## 1          HPS          0  sasyqi          2017-02-02T12:42:42-04:00
    ##   SourceModTime
    ## 1    1801672962

``` r
mega = defCasTable(conn,'MEGACORP5M')
system.time({ result = summary(mega) })
```

    ## Selecting by Frequency

    ##    user  system elapsed 
    ##    0.26    0.02   8.61

``` r
result
```

    ##       Date         DateByYear     DateByMonth      DayOfWeek    
    ##  Min.   : 7305   Min.   : 7305   Min.   : 7305   Min.   :1.000  
    ##  1st Qu.:12460   1st Qu.:12419   1st Qu.:12450   1st Qu.:3.000  
    ##  Median :14605   Median :14245   Median :14579   Median :4.000  
    ##  Mean   :14389   Mean   :14209   Mean   :14375   Mean   :3.986  
    ##  3rd Qu.:16517   3rd Qu.:16437   3rd Qu.:16496   3rd Qu.:5.000  
    ##  Max.   :18992   Max.   :18628   Max.   :18962   Max.   :7.000  
    ##                                                                 
    ##    FacilityID        Facility       FacilityType      FacilityAge   
    ##  Min.   :  2.00   AL00002:8675945   Plant:70732833   Min.   : 0.00  
    ##  1st Qu.: 20.00   TX00020:7869868                    1st Qu.: 6.00  
    ##  Median : 63.00   AL00072:6278971                    Median :11.00  
    ##  Mean   : 63.04   TX00029:6083924                    Mean   :12.12  
    ##  3rd Qu.: 99.00   AR00063:5667287                    3rd Qu.:17.00  
    ##  Max.   :178.00                                      Max.   :31.00  
    ##                                                                     
    ##  EmployeesUsed     FacilityRegion   FacilityState         FacilityCity    
    ##  Min.   :0.01943   South:42019795   TX:18265607   Mobile        :8675945  
    ##  1st Qu.:0.02564   West :14301043   AL:14954916   Corpus Christi:7869868  
    ##  Median :0.03452   North:11716240   IL: 8021919   Birmingham    :6278971  
    ##  Mean   :0.05026   East : 2695755   CA: 7263519   Houston       :6083924  
    ##  3rd Qu.:0.04473                    AR: 5667287   Little Rock   :5667287  
    ##  Max.   :2.68667                                                          
    ##                                                                           
    ##      UnitID                 Unit          UnitDowntime     
    ##  Min.   :  3.00   TOYAF0000003:1093931   Min.   :0.000000  
    ##  1st Qu.: 25.00   TOYAF0000005:1087559   1st Qu.:0.000000  
    ##  Median : 66.00   TOYAF0000009:1087501   Median :0.000000  
    ##  Mean   : 67.52   TOYAF0000007:1085817   Mean   :0.006194  
    ##  3rd Qu.:102.00   TOYAF0000010:1083843   3rd Qu.:0.000000  
    ##  Max.   :184.00                          Max.   :1.000000  
    ##                                                            
    ##     UnitAge       UnitLifespan    UnitLifespanLimit UnitReliability 
    ##  Min.   :0.000   Min.   :0.2500   Min.   :0.1000    Min.   :0.3978  
    ##  1st Qu.:1.000   1st Qu.:0.5833   1st Qu.:0.3000    1st Qu.:0.8236  
    ##  Median :3.000   Median :0.7500   Median :0.3000    Median :0.9169  
    ##  Mean   :3.031   Mean   :0.7474   Mean   :0.2852    Mean   :0.8792  
    ##  3rd Qu.:5.000   3rd Qu.:0.9166   3rd Qu.:0.3000    3rd Qu.:0.9591  
    ##  Max.   :9.000   Max.   :1.0000   Max.   :0.5000    Max.   :0.9892  
    ##                                                                     
    ##    UnitStatus        UnitCapacity   UnitYieldTarget UnitYieldActual
    ##  active :70294713   Min.   : 16.0   Min.   :  0.0   Min.   :  0.0  
    ##  closed :  186511   1st Qu.:160.0   1st Qu.:111.0   1st Qu.: 91.0  
    ##  upkeep :  130926   Median :176.0   Median :160.0   Median :139.0  
    ##  failure:  119769   Mean   :174.4   Mean   :143.4   Mean   :128.9  
    ##  upgrade:     914   3rd Qu.:193.0   3rd Qu.:176.0   3rd Qu.:167.0  
    ##                     Max.   :212.0   Max.   :212.0   Max.   :212.0  
    ##                                                                    
    ##  UnitYieldRate        ProductBrand              ProductLine      
    ##  Min.   :1.250e-01   Toy    :66466277   Action Figure :36337270  
    ##  1st Qu.:8.409e-01   Novelty: 4266556   Game          :28185491  
    ##  Median :9.500e-01                      Promotional   : 4266556  
    ##  Mean   :8.989e-01                      Stuffed Animal: 1943516  
    ##  3rd Qu.:1.000e+00                                               
    ##  Max.   :1.000e+00                                               
    ##  NA's   :4.381e+05                                               
    ##    ProductID               Product            ProductDescription 
    ##  Min.   :     105   Card       :9363717   Male         :7787301  
    ##  1st Qu.:17954356   Board      :9347660   Female       :7745677  
    ##  Median :35716456   Puzzle     :9331527   Spike        :2363043  
    ##  Mean   :35701366   Firefighter:5206023   Slam         :2344791  
    ##  3rd Qu.:53504339   Movie Star :5192004   Playing Cards:2328319  
    ##  Max.   :71242367                                                
    ##  NA's   :  438120                                                
    ##  ProductPriceTarget  ProductPriceActual  ProductMaterialCost
    ##  Min.   :5.000e-01   Min.   :0.000e+00   Min.   :  0.0000   
    ##  1st Qu.:2.702e+00   1st Qu.:2.702e+00   1st Qu.:  0.3089   
    ##  Median :4.086e+00   Median :4.086e+00   Median :  0.4380   
    ##  Mean   :1.631e+01   Mean   :1.623e+01   Mean   :  6.3782   
    ##  3rd Qu.:1.685e+01   3rd Qu.:1.629e+01   3rd Qu.:  7.2271   
    ##  Max.   :2.628e+02   Max.   :2.628e+02   Max.   :157.6819   
    ##  NA's   :4.381e+05   NA's   :4.381e+05                      
    ##  ProductQuality          Profit              Revenue        
    ##  Min.   :     1.00   Min.   :-2.034e+04   Min.   :   0.000  
    ##  1st Qu.:     1.00   1st Qu.: 2.202e-01   1st Qu.:   4.325  
    ##  Median :     1.00   Median : 6.026e+00   Median :  12.157  
    ##  Mean   :     1.05   Mean   : 2.661e+01   Mean   :  42.765  
    ##  3rd Qu.:     1.00   3rd Qu.: 3.200e+01   3rd Qu.:  47.854  
    ##  Max.   :     4.00   Max.   : 1.584e+03   Max.   :1618.164  
    ##  NA's   :438120.00                                          
    ##     Expenses         ExpensesCapital     ExpensesOperational
    ##  Min.   :    1.985   Min.   :    0.000   Min.   :  0.250    
    ##  1st Qu.:    3.317   1st Qu.:    0.000   1st Qu.:  1.270    
    ##  Median :    5.570   Median :    0.000   Median :  1.558    
    ##  Mean   :   16.153   Mean   :    3.667   Mean   :  2.966    
    ##  3rd Qu.:   12.897   3rd Qu.:    0.000   3rd Qu.:  2.176    
    ##  Max.   :20340.509   Max.   :20000.000   Max.   :280.000    
    ##                                                             
    ##  ExpensesMaterial    ExpensesStaffing   RegionLongitude   RegionLatitude 
    ##  Min.   :0.000e+00   Min.   :  0.8808   Min.   :-115.53   Min.   :34.26  
    ##  1st Qu.:3.582e-01   1st Qu.:  1.1763   1st Qu.: -92.32   1st Qu.:34.26  
    ##  Median :6.735e-01   Median :  1.7746   Median : -90.64   Median :34.26  
    ##  Mean   :8.540e+00   Mean   :  3.1424   Mean   : -95.28   Mean   :37.35  
    ##  3rd Qu.:8.688e+00   3rd Qu.:  2.4189   3rd Qu.: -90.64   3rd Qu.:41.06  
    ##  Max.   :1.340e+04   Max.   :249.1667   Max.   : -73.09   Max.   :43.57  
    ##                                                                          
    ##  StateLongitude    StateLatitude   CityLongitude        CityLatitude      
    ##  Min.   :-119.99   Min.   :31.05   Min.   :  -122.42   Min.   :    27.80  
    ##  1st Qu.: -99.68   1st Qu.:31.17   1st Qu.:   -97.40   1st Qu.:    30.69  
    ##  Median : -92.55   Median :32.62   Median :   -92.29   Median :    33.51  
    ##  Mean   : -96.62   Mean   :35.09   Mean   :   -96.00   Mean   :    34.37  
    ##  3rd Qu.: -86.74   3rd Qu.:37.27   3rd Qu.:   -88.04   3rd Qu.:    37.77  
    ##  Max.   : -71.55   Max.   :47.27   Max.   :   -71.46   Max.   :    47.61  
    ##                                    NA's   :612883.00   NA's   :612883.00

``` r
cas.sessionProp.setSessOpt(conn,caslib="casuser")
```

    ## NOTE: 'CASUSER(sasyqi)' is now the active caslib.

    ## list()

``` r
cas.count(tbl)
```

    ##         Column   N
    ## 1 Sepal.Length 150
    ## 2  Sepal.Width 150
    ## 3 Petal.Length 150
    ## 4  Petal.Width 150

``` r
cas.mean(tbl)
```

    ##         Column     Mean
    ## 1 Sepal.Length 5.843333
    ## 2  Sepal.Width 3.057333
    ## 3 Petal.Length 3.758000
    ## 4  Petal.Width 1.199333

``` r
cas.var(tbl)
```

    ##         Column       Var
    ## 1 Sepal.Length 0.6856935
    ## 2  Sepal.Width 0.1899794
    ## 3 Petal.Length 3.1162779
    ## 4  Petal.Width 0.5810063

``` r
plot(tbl$Sepal.Length, tbl$Sepal.Width, pch = 16, type = 'p', 
  col = 'blue', main = "Plot on a CASTable")
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/6_1.png)

``` r
par(mfrow=c(1,3))

tbl_tmp <- tbl[tbl$Species == 'setosa',]
plot(tbl_tmp$Sepal.Length, tbl_tmp$Sepal.Width, pch = 16, type = 'p', 
    col = 'red', main = "setosa")

tbl_tmp <- tbl[tbl$Species == 'versicolor',]
plot(tbl_tmp$Sepal.Length, tbl_tmp$Sepal.Width, pch = 17, type = 'p', 
    col = 'green', main = "versicolor")

tbl_tmp <- tbl[tbl$Species == 'virginica',]
plot(tbl_tmp$Sepal.Length, tbl_tmp$Sepal.Width, pch = 18, type = 'p', 
    col = 'blue', main = "virginica")
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/6_2.png)

Sorting, Data Selection, and Iteration
======================================

Fetching Data with a Sort Order
-------------------------------

``` r
sorttbl <- defCasTable(conn, 'iris', orderby = 'Sepal.Length')
```

``` r
sorttbl <- defCasTable(conn, 'iris')
sorttbl@orderby <- 'Sepal.Length'
```

``` r
head(sorttbl, n = 10L)
```

    ##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1           4.3         3.0          1.1         0.1  setosa
    ## 2           4.4         3.0          1.3         0.2  setosa
    ## 3           4.4         2.9          1.4         0.2  setosa
    ## 4           4.4         3.2          1.3         0.2  setosa
    ## 5           4.5         2.3          1.3         0.3  setosa
    ## 6           4.6         3.1          1.5         0.2  setosa
    ## 7           4.6         3.6          1.0         0.2  setosa
    ## 8           4.6         3.4          1.4         0.3  setosa
    ## 9           4.6         3.2          1.4         0.2  setosa
    ## 10          4.7         3.2          1.6         0.2  setosa

``` r
tail(sorttbl, n = 10L)
```

    ##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 141          7.2         3.2          6.0         1.8 virginica
    ## 142          7.2         3.0          5.8         1.6 virginica
    ## 143          7.3         2.9          6.3         1.8 virginica
    ## 144          7.4         2.8          6.1         1.9 virginica
    ## 145          7.6         3.0          6.6         2.1 virginica
    ## 146          7.7         2.6          6.9         2.3 virginica
    ## 147          7.7         3.0          6.1         2.3 virginica
    ## 148          7.7         3.8          6.7         2.2 virginica
    ## 149          7.7         2.8          6.7         2.0 virginica
    ## 150          7.9         3.8          6.4         2.0 virginica

Iterating through Columns
-------------------------

``` r
for (col in names(sorttbl)){
  print(col)
}
```

    ## [1] "Sepal.Length"
    ## [1] "Sepal.Width"
    ## [1] "Petal.Length"
    ## [1] "Petal.Width"
    ## [1] "Species"

Techniques for Indexing and Selecting Data
------------------------------------------

### Selecting Columns by Label and Position

``` r
sorttbl@orderby <- 'Sepal.Width'
col <- sorttbl['Sepal.Width']
head(col)
```

    ##   Sepal.Width
    ## 1         2.0
    ## 2         2.2
    ## 3         2.2
    ## 4         2.2
    ## 5         2.3
    ## 6         2.3

``` r
col <- sorttbl$Sepal.Width
head(col)
```

    ##   Sepal.Width
    ## 1         2.0
    ## 2         2.2
    ## 3         2.2
    ## 4         2.2
    ## 5         2.3
    ## 6         2.3

``` r
widths <- sorttbl[c('Sepal.Width', 'Petal.Width', 'Species')]
class(widths)
```

    ## [1] "CASTable"
    ## attr(,"package")
    ## [1] "swat"

``` r
head(widths)
```

    ##   Sepal.Width Petal.Width    Species
    ## 1         2.0         1.0 versicolor
    ## 2         2.2         1.0 versicolor
    ## 3         2.2         1.5 versicolor
    ## 4         2.2         1.5  virginica
    ## 5         2.3         1.3 versicolor
    ## 6         2.3         0.3     setosa

``` r
summary(widths)
```

    ## Selecting by Frequency

    ## Warning in if (s3 > 0) {: the condition has length > 1 and only the first
    ## element will be used

    ## Warning in if (s3 > 0) {: the condition has length > 1 and only the first
    ## element will be used

    ##   Sepal.Width     Petal.Width          Species  
    ##  Min.   :2.000   Min.   :0.100   setosa    :50  
    ##  1st Qu.:2.800   1st Qu.:0.300   versicolor:50  
    ##  Median :3.000   Median :1.300   virginica :50  
    ##  Mean   :3.057   Mean   :1.199                  
    ##  3rd Qu.:3.300   3rd Qu.:1.800                  
    ##  Max.   :4.400   Max.   :2.500

``` r
cas.table.columnInfo(widths)
```

    ## $ColumnInfo
    ##        Column ID    Type RawLength FormattedLength NFL NFD
    ## 1 Sepal.Width  2  double         8              12   0   0
    ## 2 Petal.Width  4  double         8              12   0   0
    ## 3     Species  5 varchar        10              10   0   0

``` r
sorttbl@orderby <- 'Sepal.Width'
```

Select the petal\_width column

``` r
head(sorttbl[,"Petal.Width"])
```

    ##   Petal.Width
    ## 1         1.0
    ## 2         1.0
    ## 3         1.5
    ## 4         1.5
    ## 5         1.3
    ## 6         0.3

Select a list of columns

``` r
head(sorttbl[,c("Petal.Width","Sepal.Length")])
```

    ##   Petal.Width Sepal.Length
    ## 1         1.0          5.0
    ## 2         1.0          6.0
    ## 3         1.5          6.2
    ## 4         1.5          6.0
    ## 5         1.3          5.5
    ## 6         0.3          4.5

``` r
head(sorttbl[,3])
```

    ##   Petal.Length
    ## 1          3.5
    ## 2          4.0
    ## 3          4.5
    ## 4          5.0
    ## 5          4.0
    ## 6          1.3

``` r
head(sorttbl[,1:3])
```

    ##   Sepal.Length Sepal.Width Petal.Length
    ## 1          5.0         2.0          3.5
    ## 2          6.0         2.2          4.0
    ## 3          6.2         2.2          4.5
    ## 4          6.0         2.2          5.0
    ## 5          5.5         2.3          4.0
    ## 6          4.5         2.3          1.3

``` r
head(sorttbl[,c(4,2)])
```

    ##   Petal.Width Sepal.Width
    ## 1         1.0         2.0
    ## 2         1.0         2.2
    ## 3         1.5         2.2
    ## 4         1.5         2.2
    ## 5         1.3         2.3
    ## 6         0.3         2.3

### Dynamic Data Selection

``` r
expr <- sorttbl$Petal.Length > 6.5
```

``` r
newtbl <- sorttbl[expr,]
head(newtbl)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 1          7.7         2.6          6.9         2.3 virginica
    ## 2          7.7         2.8          6.7         2.0 virginica
    ## 3          7.6         3.0          6.6         2.1 virginica
    ## 4          7.7         3.8          6.7         2.2 virginica

``` r
newtbl <- sorttbl[sorttbl$Petal.Length > 6.5,]
head(newtbl)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 1          7.7         2.6          6.9         2.3 virginica
    ## 2          7.7         2.8          6.7         2.0 virginica
    ## 3          7.6         3.0          6.6         2.1 virginica
    ## 4          7.7         3.8          6.7         2.2 virginica

``` r
newtbl2 <- newtbl[newtbl$Petal.Width < 2.2,]
head(newtbl2)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 1          7.7         2.8          6.7         2.0 virginica
    ## 2          7.6         3.0          6.6         2.1 virginica

``` r
newtbl3 <- sorttbl[(sorttbl$Petal.Length > 6.5) & (sorttbl$Petal.Width < 2.2),]
head(newtbl3)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 1          7.7         2.8          6.7         2.0 virginica
    ## 2          7.6         3.0          6.6         2.1 virginica

``` r
attributes(newtbl3)
```

    ## $conn
    ## CAS(hostname=rdcgrdc.unx.sas.com, port=39935, username=sasyqi, session=eecfe36c-23b3-084c-bbb6-a477f078a38d, protocol=http)
    ## 
    ## $tname
    ## [1] "iris"
    ## 
    ## $caslib
    ## [1] ""
    ## 
    ## $where
    ## [1] "((\"Petal.Length\"n > 6.5) AND (\"Petal.Width\"n < 2.2))"
    ## 
    ## $orderby
    ## [1] "Sepal.Width"
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
tbl[(tbl$Petal.Length + tbl$Petal.Width) * 2 > 17.5,]
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 1          7.7         3.8          6.7         2.2 virginica
    ## 2          7.7         2.6          6.9         2.3 virginica

``` r
tbl <- defCasTable(conn, 'iris')
virginica <- tbl[tbl$Species == 'virginica',]
dim(virginica)
```

    ## [1] 50  5

``` r
head(virginica)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ## 1          6.3         3.3          6.0         2.5 virginica
    ## 2          5.8         2.7          5.1         1.9 virginica
    ## 3          7.1         3.0          5.9         2.1 virginica
    ## 4          6.3         2.9          5.6         1.8 virginica
    ## 5          6.5         3.0          5.8         2.2 virginica
    ## 6          7.6         3.0          6.6         2.1 virginica

Data Wrangling on the Fly
=========================

Creating Computed Columns
-------------------------

``` r
sorttbl['Sepal.Factor'] <- (sorttbl$Sepal.Length+sorttbl$Sepal.Width)*2
head(sorttbl)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
    ## 1          5.0         2.0          3.5         1.0 versicolor
    ## 2          6.0         2.2          4.0         1.0 versicolor
    ## 3          6.2         2.2          4.5         1.5 versicolor
    ## 4          6.0         2.2          5.0         1.5  virginica
    ## 5          5.5         2.3          4.0         1.3 versicolor
    ## 6          4.5         2.3          1.3         0.3     setosa
    ##   Sepal.Factor
    ## 1         14.0
    ## 2         16.4
    ## 3         16.8
    ## 4         16.4
    ## 5         15.6
    ## 6         13.6

``` r
sorttbl['Total.Factor'] <- sorttbl$Sepal.Factor + sorttbl$Petal.Width + sorttbl$Petal.Length
head(sorttbl)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
    ## 1          5.0         2.0          3.5         1.0 versicolor
    ## 2          6.0         2.2          4.0         1.0 versicolor
    ## 3          6.2         2.2          4.5         1.5 versicolor
    ## 4          6.0         2.2          5.0         1.5  virginica
    ## 5          5.5         2.3          4.0         1.3 versicolor
    ## 6          4.5         2.3          1.3         0.3     setosa
    ##   Sepal.Factor Total.Factor
    ## 1         14.0         18.5
    ## 2         16.4         21.4
    ## 3         16.8         22.8
    ## 4         16.4         22.9
    ## 5         15.6         20.9
    ## 6         13.6         15.2

``` r
sorttbl['names'] <- 'sepal / petal'
head(sorttbl)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
    ## 1          5.0         2.0          3.5         1.0 versicolor
    ## 2          6.0         2.2          4.0         1.0 versicolor
    ## 3          6.2         2.2          4.5         1.5 versicolor
    ## 4          6.0         2.2          5.0         1.5  virginica
    ## 5          5.5         2.3          4.0         1.3 versicolor
    ## 6          4.5         2.3          1.3         0.3     setosa
    ##   Sepal.Factor Total.Factor         names
    ## 1         14.0         18.5 sepal / petal
    ## 2         16.4         21.4 sepal / petal
    ## 3         16.8         22.8 sepal / petal
    ## 4         16.4         22.9 sepal / petal
    ## 5         15.6         20.9 sepal / petal
    ## 6         13.6         15.2 sepal / petal

``` r
tbl = defCasTable(conn, 'iris')
tbl$is.virginica  <- (tbl$Species == 'virginica')
head(tbl) 
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species is.virginica
    ## 1          5.1         3.5          1.4         0.2  setosa            0
    ## 2          4.9         3.0          1.4         0.2  setosa            0
    ## 3          4.7         3.2          1.3         0.2  setosa            0
    ## 4          4.6         3.1          1.5         0.2  setosa            0
    ## 5          5.0         3.6          1.4         0.2  setosa            0
    ## 6          5.4         3.9          1.7         0.4  setosa            0

BY-Group Processing
-------------------

``` r
tbl = defCasTable(conn, 'iris')
```

``` r
grptbl <- tbl
grptbl@groupby <- 'Species'
attributes(grptbl)
```

    ## $conn
    ## CAS(hostname=rdcgrdc.unx.sas.com, port=39935, username=sasyqi, session=eecfe36c-23b3-084c-bbb6-a477f078a38d, protocol=http)
    ## 
    ## $tname
    ## [1] "iris"
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
    ## [1] "Species"
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
cas.simple.summary(grptbl, subset = c("min","max"))
```

    ## $ByGroup1.Summary
    ##   Species       Column Min Max
    ## 1  setosa Sepal.Length 4.3 5.8
    ## 2  setosa  Sepal.Width 2.3 4.4
    ## 3  setosa Petal.Length 1.0 1.9
    ## 4  setosa  Petal.Width 0.1 0.6
    ## 
    ## $ByGroup2.Summary
    ##      Species       Column Min Max
    ## 1 versicolor Sepal.Length 4.9 7.0
    ## 2 versicolor  Sepal.Width 2.0 3.4
    ## 3 versicolor Petal.Length 3.0 5.1
    ## 4 versicolor  Petal.Width 1.0 1.8
    ## 
    ## $ByGroup3.Summary
    ##     Species       Column Min Max
    ## 1 virginica Sepal.Length 4.9 7.9
    ## 2 virginica  Sepal.Width 2.2 3.8
    ## 3 virginica Petal.Length 4.5 6.9
    ## 4 virginica  Petal.Width 1.4 2.5
    ## 
    ## $ByGroupInfo
    ##      Species  Species_f      _key_
    ## 1     setosa     setosa setosa    
    ## 2 versicolor versicolor versicolor
    ## 3  virginica  virginica virginica

``` r
cas.simple.summary(
  conn, 
  table = list(name = 'iris', groupby = 'Species'),
  subset = c("min","max"))
```

    ## $ByGroup1.Summary
    ##   Species       Column Min Max
    ## 1  setosa Sepal.Length 4.3 5.8
    ## 2  setosa  Sepal.Width 2.3 4.4
    ## 3  setosa Petal.Length 1.0 1.9
    ## 4  setosa  Petal.Width 0.1 0.6
    ## 
    ## $ByGroup2.Summary
    ##      Species       Column Min Max
    ## 1 versicolor Sepal.Length 4.9 7.0
    ## 2 versicolor  Sepal.Width 2.0 3.4
    ## 3 versicolor Petal.Length 3.0 5.1
    ## 4 versicolor  Petal.Width 1.0 1.8
    ## 
    ## $ByGroup3.Summary
    ##     Species       Column Min Max
    ## 1 virginica Sepal.Length 4.9 7.9
    ## 2 virginica  Sepal.Width 2.2 3.8
    ## 3 virginica Petal.Length 4.5 6.9
    ## 4 virginica  Petal.Width 1.4 2.5
    ## 
    ## $ByGroupInfo
    ##      Species  Species_f      _key_
    ## 1     setosa     setosa setosa    
    ## 2 versicolor versicolor versicolor
    ## 3  virginica  virginica virginica

### Concatenating BY Groups

``` r
grpsumm <- cas.simple.summary(grptbl, subset = c("min","max"))
```

``` r
rbind.bygroups(grpsumm)
```

    ## $Summary
    ##       Species       Column Min Max
    ## 1      setosa Sepal.Length 4.3 5.8
    ## 2      setosa  Sepal.Width 2.3 4.4
    ## 3      setosa Petal.Length 1.0 1.9
    ## 4      setosa  Petal.Width 0.1 0.6
    ## 5  versicolor Sepal.Length 4.9 7.0
    ## 6  versicolor  Sepal.Width 2.0 3.4
    ## 7  versicolor Petal.Length 3.0 5.1
    ## 8  versicolor  Petal.Width 1.0 1.8
    ## 9   virginica Sepal.Length 4.9 7.9
    ## 10  virginica  Sepal.Width 2.2 3.8
    ## 11  virginica Petal.Length 4.5 6.9
    ## 12  virginica  Petal.Width 1.4 2.5

Selecting Result Keys by Table Name

``` r
grpcorr <- cas.simple.correlation(grptbl)
grpcorr
```

    ## $ByGroup1.CorrSimple
    ##   Species     Variable  N  Mean   Sum    StdDev Minimum Maximum
    ## 1  setosa Sepal.Length 50 5.006 250.3 0.3524897     4.3     5.8
    ## 2  setosa  Sepal.Width 50 3.428 171.4 0.3790644     2.3     4.4
    ## 3  setosa Petal.Length 50 1.462  73.1 0.1736640     1.0     1.9
    ## 4  setosa  Petal.Width 50 0.246  12.3 0.1053856     0.1     0.6
    ## 
    ## $ByGroup1.Correlation
    ##   Species     Variable Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## 1  setosa Sepal.Length    1.0000000   0.7425467    0.2671758   0.2780984
    ## 2  setosa  Sepal.Width    0.7425467   1.0000000    0.1777000   0.2327520
    ## 3  setosa Petal.Length    0.2671758   0.1777000    1.0000000   0.3316300
    ## 4  setosa  Petal.Width    0.2780984   0.2327520    0.3316300   1.0000000
    ## 
    ## $ByGroup2.CorrSimple
    ##      Species     Variable  N  Mean   Sum    StdDev Minimum Maximum
    ## 1 versicolor Sepal.Length 50 5.936 296.8 0.5161711     4.9     7.0
    ## 2 versicolor  Sepal.Width 50 2.770 138.5 0.3137983     2.0     3.4
    ## 3 versicolor Petal.Length 50 4.260 213.0 0.4699110     3.0     5.1
    ## 4 versicolor  Petal.Width 50 1.326  66.3 0.1977527     1.0     1.8
    ## 
    ## $ByGroup2.Correlation
    ##      Species     Variable Sepal.Length Sepal.Width Petal.Length
    ## 1 versicolor Sepal.Length    1.0000000   0.5259107    0.7540490
    ## 2 versicolor  Sepal.Width    0.5259107   1.0000000    0.5605221
    ## 3 versicolor Petal.Length    0.7540490   0.5605221    1.0000000
    ## 4 versicolor  Petal.Width    0.5464611   0.6639987    0.7866681
    ##   Petal.Width
    ## 1   0.5464611
    ## 2   0.6639987
    ## 3   0.7866681
    ## 4   1.0000000
    ## 
    ## $ByGroup3.CorrSimple
    ##     Species     Variable  N  Mean   Sum    StdDev Minimum Maximum
    ## 1 virginica Sepal.Length 50 6.588 329.4 0.6358796     4.9     7.9
    ## 2 virginica  Sepal.Width 50 2.974 148.7 0.3224966     2.2     3.8
    ## 3 virginica Petal.Length 50 5.552 277.6 0.5518947     4.5     6.9
    ## 4 virginica  Petal.Width 50 2.026 101.3 0.2746501     1.4     2.5
    ## 
    ## $ByGroup3.Correlation
    ##     Species     Variable Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## 1 virginica Sepal.Length    1.0000000   0.4572278    0.8642247   0.2811077
    ## 2 virginica  Sepal.Width    0.4572278   1.0000000    0.4010446   0.5377280
    ## 3 virginica Petal.Length    0.8642247   0.4010446    1.0000000   0.3221082
    ## 4 virginica  Petal.Width    0.2811077   0.5377280    0.3221082   1.0000000
    ## 
    ## $ByGroupInfo
    ##      Species  Species_f      _key_
    ## 1     setosa     setosa setosa    
    ## 2 versicolor versicolor versicolor
    ## 3  virginica  virginica virginica

``` r
result <- rbind.bygroups(grpcorr)
result$Correlation
```

    ##       Species     Variable Sepal.Length Sepal.Width Petal.Length
    ## 1      setosa Sepal.Length    1.0000000   0.7425467    0.2671758
    ## 2      setosa  Sepal.Width    0.7425467   1.0000000    0.1777000
    ## 3      setosa Petal.Length    0.2671758   0.1777000    1.0000000
    ## 4      setosa  Petal.Width    0.2780984   0.2327520    0.3316300
    ## 5  versicolor Sepal.Length    1.0000000   0.5259107    0.7540490
    ## 6  versicolor  Sepal.Width    0.5259107   1.0000000    0.5605221
    ## 7  versicolor Petal.Length    0.7540490   0.5605221    1.0000000
    ## 8  versicolor  Petal.Width    0.5464611   0.6639987    0.7866681
    ## 9   virginica Sepal.Length    1.0000000   0.4572278    0.8642247
    ## 10  virginica  Sepal.Width    0.4572278   1.0000000    0.4010446
    ## 11  virginica Petal.Length    0.8642247   0.4010446    1.0000000
    ## 12  virginica  Petal.Width    0.2811077   0.5377280    0.3221082
    ##    Petal.Width
    ## 1    0.2780984
    ## 2    0.2327520
    ## 3    0.3316300
    ## 4    1.0000000
    ## 5    0.5464611
    ## 6    0.6639987
    ## 7    0.7866681
    ## 8    1.0000000
    ## 9    0.2811077
    ## 10   0.5377280
    ## 11   0.3221082
    ## 12   1.0000000

### Handling Multiple Sets of BY Groups

``` r
grpmdsumm <- cas.simple.mdSummary(
  tbl, 
  sets=list(
    list(groupby='Sepal.Length'),
    list(groupby='Petal.Length')
    )
  )
names(grpmdsumm)
```

    ##  [1] "ByGroupSet1.ByGroup1.MDSummary"  "ByGroupSet1.ByGroup10.MDSummary"
    ##  [3] "ByGroupSet1.ByGroup11.MDSummary" "ByGroupSet1.ByGroup12.MDSummary"
    ##  [5] "ByGroupSet1.ByGroup13.MDSummary" "ByGroupSet1.ByGroup14.MDSummary"
    ##  [7] "ByGroupSet1.ByGroup15.MDSummary" "ByGroupSet1.ByGroup16.MDSummary"
    ##  [9] "ByGroupSet1.ByGroup17.MDSummary" "ByGroupSet1.ByGroup18.MDSummary"
    ## [11] "ByGroupSet1.ByGroup19.MDSummary" "ByGroupSet1.ByGroup2.MDSummary" 
    ## [13] "ByGroupSet1.ByGroup20.MDSummary" "ByGroupSet1.ByGroup21.MDSummary"
    ## [15] "ByGroupSet1.ByGroup22.MDSummary" "ByGroupSet1.ByGroup23.MDSummary"
    ## [17] "ByGroupSet1.ByGroup24.MDSummary" "ByGroupSet1.ByGroup25.MDSummary"
    ## [19] "ByGroupSet1.ByGroup26.MDSummary" "ByGroupSet1.ByGroup27.MDSummary"
    ## [21] "ByGroupSet1.ByGroup28.MDSummary" "ByGroupSet1.ByGroup29.MDSummary"
    ## [23] "ByGroupSet1.ByGroup3.MDSummary"  "ByGroupSet1.ByGroup30.MDSummary"
    ## [25] "ByGroupSet1.ByGroup31.MDSummary" "ByGroupSet1.ByGroup32.MDSummary"
    ## [27] "ByGroupSet1.ByGroup33.MDSummary" "ByGroupSet1.ByGroup34.MDSummary"
    ## [29] "ByGroupSet1.ByGroup35.MDSummary" "ByGroupSet1.ByGroup4.MDSummary" 
    ## [31] "ByGroupSet1.ByGroup5.MDSummary"  "ByGroupSet1.ByGroup6.MDSummary" 
    ## [33] "ByGroupSet1.ByGroup7.MDSummary"  "ByGroupSet1.ByGroup8.MDSummary" 
    ## [35] "ByGroupSet1.ByGroup9.MDSummary"  "ByGroupSet1.ByGroupInfo"        
    ## [37] "ByGroupSet2.ByGroup1.MDSummary"  "ByGroupSet2.ByGroup10.MDSummary"
    ## [39] "ByGroupSet2.ByGroup11.MDSummary" "ByGroupSet2.ByGroup12.MDSummary"
    ## [41] "ByGroupSet2.ByGroup13.MDSummary" "ByGroupSet2.ByGroup14.MDSummary"
    ## [43] "ByGroupSet2.ByGroup15.MDSummary" "ByGroupSet2.ByGroup16.MDSummary"
    ## [45] "ByGroupSet2.ByGroup17.MDSummary" "ByGroupSet2.ByGroup18.MDSummary"
    ## [47] "ByGroupSet2.ByGroup19.MDSummary" "ByGroupSet2.ByGroup2.MDSummary" 
    ## [49] "ByGroupSet2.ByGroup20.MDSummary" "ByGroupSet2.ByGroup21.MDSummary"
    ## [51] "ByGroupSet2.ByGroup22.MDSummary" "ByGroupSet2.ByGroup23.MDSummary"
    ## [53] "ByGroupSet2.ByGroup24.MDSummary" "ByGroupSet2.ByGroup25.MDSummary"
    ## [55] "ByGroupSet2.ByGroup26.MDSummary" "ByGroupSet2.ByGroup27.MDSummary"
    ## [57] "ByGroupSet2.ByGroup28.MDSummary" "ByGroupSet2.ByGroup29.MDSummary"
    ## [59] "ByGroupSet2.ByGroup3.MDSummary"  "ByGroupSet2.ByGroup30.MDSummary"
    ## [61] "ByGroupSet2.ByGroup31.MDSummary" "ByGroupSet2.ByGroup32.MDSummary"
    ## [63] "ByGroupSet2.ByGroup33.MDSummary" "ByGroupSet2.ByGroup34.MDSummary"
    ## [65] "ByGroupSet2.ByGroup35.MDSummary" "ByGroupSet2.ByGroup36.MDSummary"
    ## [67] "ByGroupSet2.ByGroup37.MDSummary" "ByGroupSet2.ByGroup38.MDSummary"
    ## [69] "ByGroupSet2.ByGroup39.MDSummary" "ByGroupSet2.ByGroup4.MDSummary" 
    ## [71] "ByGroupSet2.ByGroup40.MDSummary" "ByGroupSet2.ByGroup41.MDSummary"
    ## [73] "ByGroupSet2.ByGroup42.MDSummary" "ByGroupSet2.ByGroup43.MDSummary"
    ## [75] "ByGroupSet2.ByGroup5.MDSummary"  "ByGroupSet2.ByGroup6.MDSummary" 
    ## [77] "ByGroupSet2.ByGroup7.MDSummary"  "ByGroupSet2.ByGroup8.MDSummary" 
    ## [79] "ByGroupSet2.ByGroup9.MDSummary"  "ByGroupSet2.ByGroupInfo"

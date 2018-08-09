Chapter 7 - Data Exploration and Summary Statistics
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

Overview
========

``` r
cas.sessionProp.setSessOpt(conn, caslib = "HPS")
```

    ## NOTE: 'HPS' is now the active caslib.

    ## list()

``` r
out <- cas.table.loadTable(
  conn, 
  path = 'organics_new_vistat.sashdat',
  casOut = list(name = "organics", replace = TRUE))
```

    ## NOTE: Cloud Analytic Services made the HDFS file organics_new_vistat.sashdat available as table ORGANICS in caslib HPS.

``` r
organics <- defCasTable(conn,'organics')
```

Summarizing Continuous Variables
================================

Descriptive Statistics
----------------------

``` r
cas.simple.summary(organics)
```

    ## $Summary
    ##           Column     Min     Max       N  NMiss        Mean        Sum
    ## 1        DemAffl    0.00   34.00 1606488  82460    8.711893   13995552
    ## 2         DemAge   18.00   79.00 1574340 114608   53.797152   84695008
    ## 3       PromTime    0.00   39.00 1667592  21356    6.564670   10947192
    ## 4  purchase_3mon  698.44 1188.06 1688948      0  950.027539 1604547112
    ## 5  purchase_6mon 1668.77 2370.87 1688948      0 2049.979250 3462308354
    ## 6  purchase_9mon 2624.09 3468.72 1688948      0 3070.016785 5185098708
    ## 7 purchase_12mon 3698.92 4684.88 1688948      0 4189.994798 7076683333
    ##          Std      StdErr         Var          USS         CSS        CV
    ## 1   3.421045 0.002699106    11.70355 1.407294e+08    18801597 39.268672
    ## 2  13.205734 0.010524786   174.39140 4.830901e+09   274551188 24.547273
    ## 3   4.657008 0.003606302    21.68772 1.080310e+08    36166252 70.940468
    ## 4  50.067179 0.038525207  2506.72244 1.528598e+12  4233721342  5.270077
    ## 5  70.731010 0.054425412  5002.87581 7.106110e+12  8449592095  3.450328
    ## 6  86.588115 0.066626983  7497.50165 1.593100e+13 12662882920  2.820444
    ## 7 100.009042 0.076953988 10001.80855 2.966816e+13 16892524537  2.386854
    ##      TValue ProbT      Skewness     Kurtosis
    ## 1  3227.695     0  0.8916212792  2.096090794
    ## 2  5111.472     0 -0.0798247122 -0.843977643
    ## 3  1820.333     0  2.2826359634  8.075535911
    ## 4 24659.894     0  0.0020064188 -0.003629311
    ## 5 37665.847     0  0.0017794945 -0.001335628
    ## 6 46077.680     0  0.0004033935  0.002077235
    ## 7 54448.053     0 -0.0025906213  0.002014442

``` r
varlist <- c('DemAge', 'Purchase_12mon', 'Purchase_6mon')
cas.simple.summary(organics, inputs = varlist)
```

    ## $Summary
    ##           Column     Min     Max       N  NMiss       Mean        Sum
    ## 1         DemAge   18.00   79.00 1574340 114608   53.79715   84695008
    ## 2 purchase_12mon 3698.92 4684.88 1688948      0 4189.99480 7076683333
    ## 3  purchase_6mon 1668.77 2370.87 1688948      0 2049.97925 3462308354
    ##         Std     StdErr        Var          USS         CSS        CV
    ## 1  13.20573 0.01052479   174.3914 4.830901e+09   274551188 24.547273
    ## 2 100.00904 0.07695399 10001.8085 2.966816e+13 16892524537  2.386854
    ## 3  70.73101 0.05442541  5002.8758 7.106110e+12  8449592095  3.450328
    ##      TValue ProbT     Skewness     Kurtosis
    ## 1  5111.472     0 -0.079824712 -0.843977643
    ## 2 54448.053     0 -0.002590621  0.002014442
    ## 3 37665.847     0  0.001779494 -0.001335628

``` r
varlist <- c('Purchase_3mon', 'Purchase_6mon', 'Purchase_9mon',  
            'Purchase_12mon')
result <- cas.simple.summary(organics, inputs = varlist)
```

``` r
names(result)
```

    ## [1] "Summary"

``` r
df <- result$Summary
names(df)
```

    ##  [1] "Column"   "Min"      "Max"      "N"        "NMiss"    "Mean"    
    ##  [7] "Sum"      "Std"      "StdErr"   "Var"      "USS"      "CSS"     
    ## [13] "CV"       "TValue"   "ProbT"    "Skewness" "Kurtosis"

``` r
df1 <- df[c('Column','Min','Mean','Max')]
t(df1)
```

    ##        1               2               3               4               
    ## Column "purchase_3mon" "purchase_6mon" "purchase_9mon" "purchase_12mon"
    ## Min    " 698.44"       "1668.77"       "2624.09"       "3698.92"       
    ## Mean   " 950.0275"     "2049.9792"     "3070.0168"     "4189.9948"     
    ## Max    "1188.06"       "2370.87"       "3468.72"       "4684.88"

``` r
barplot(df$Mean, names.arg = df$Column, col = '#1f77b4')
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_1.png)

``` r
organics@groupby <- 'DemGender'
result <- cas.simple.summary(organics, inputs = 'DemAge')
result
```

    ## $ByGroup1.Summary
    ##   DemGender Column Min Max      N NMiss     Mean      Sum      Std
    ## 1         F DemAge  18  79 861460 61864 52.88072 45554628 13.54665
    ##       StdErr      Var        USS       CSS       CV   TValue ProbT
    ## 1 0.01459534 183.5117 2567049492 158087808 25.61737 3623.123     0
    ##      Skewness   Kurtosis
    ## 1 -0.01568211 -0.9278626
    ## 
    ## $ByGroup2.Summary
    ##   DemGender Column Min Max      N NMiss     Mean      Sum      Std
    ## 1         M DemAge  18  79 411768 30172 54.53581 22456100 12.81756
    ##       StdErr      Var        USS      CSS       CV   TValue ProbT
    ## 1 0.01997464 164.2897 1292310612 67649086 23.50301 2730.253     0
    ##     Skewness   Kurtosis
    ## 1 -0.1031066 -0.7631937
    ## 
    ## $ByGroup3.Summary
    ##   DemGender Column Min Max      N NMiss     Mean      Sum     Std
    ## 1         U DemAge  18  79 301112 22572 55.40888 16684280 12.5047
    ##       StdErr      Var       USS      CSS       CV   TValue ProbT
    ## 1 0.02278815 156.3674 971541288 47083946 22.56803 2431.478     0
    ##     Skewness   Kurtosis
    ## 1 -0.1870561 -0.6433051
    ## 
    ## $ByGroupInfo
    ##   DemGender DemGender_f _key_
    ## 1         F           F     F
    ## 2         M           M     M
    ## 3         U           U     U

``` r
result <- cas.simple.summary(
  conn, 
  table = list(name = 'organics', groupby = 'DemGender'),
  inputs = 'DemAge')
```

``` r
names(result)
```

    ## [1] "ByGroup1.Summary" "ByGroup2.Summary" "ByGroup3.Summary"
    ## [4] "ByGroupInfo"

``` r
result['ByGroupInfo']
```

    ## $ByGroupInfo
    ##   DemGender DemGender_f _key_
    ## 1         F           F     F
    ## 2         M           M     M
    ## 3         U           U     U

``` r
result2 <- rbind.bygroups(result)
result2["Summary"]
```

    ## $Summary
    ##   DemGender Column Min Max      N NMiss     Mean      Sum      Std
    ## 1         F DemAge  18  79 861460 61864 52.88072 45554628 13.54665
    ## 2         M DemAge  18  79 411768 30172 54.53581 22456100 12.81756
    ## 3         U DemAge  18  79 301112 22572 55.40888 16684280 12.50470
    ##       StdErr      Var        USS       CSS       CV   TValue ProbT
    ## 1 0.01459534 183.5117 2567049492 158087808 25.61737 3623.123     0
    ## 2 0.01997464 164.2897 1292310612  67649086 23.50301 2730.253     0
    ## 3 0.02278815 156.3674  971541288  47083946 22.56803 2431.478     0
    ##      Skewness   Kurtosis
    ## 1 -0.01568211 -0.9278626
    ## 2 -0.10310664 -0.7631937
    ## 3 -0.18705608 -0.6433051

``` r
result <- cas.simple.summary(
  conn, 
  table = list(name = 'organics',
               groupby = c('DemGender','DemHomeowner')),
  inputs = 'DemAge')

names(result)
```

    ## [1] "ByGroup1.Summary" "ByGroup2.Summary" "ByGroup3.Summary"
    ## [4] "ByGroup4.Summary" "ByGroup5.Summary" "ByGroup6.Summary"
    ## [7] "ByGroupInfo"

``` r
result$ByGroupInfo
```

    ##   DemGender DemGender_f DemHomeowner DemHomeowner_f _key_
    ## 1         F           F           No             No  FNo 
    ## 2         F           F          Yes            Yes  FYes
    ## 3         M           M           No             No  MNo 
    ## 4         M           M          Yes            Yes  MYes
    ## 5         U           U           No             No  UNo 
    ## 6         U           U          Yes            Yes  UYes

``` r
result2 <- rbind.bygroups(result)
result2
```

    ## $Summary
    ##   DemGender DemHomeowner Column Min Max      N NMiss     Mean      Sum
    ## 1         F           No DemAge  18  79 560471 40011 52.88348 29639658
    ## 2         F          Yes DemAge  18  79 300989 21853 52.87559 15914970
    ## 3         M           No DemAge  18  79 267238 19499 54.52526 14571222
    ## 4         M          Yes DemAge  18  79 144530 10673 54.55530  7884878
    ## 5         U           No DemAge  18  79 195894 14747 55.41474 10855416
    ## 6         U          Yes DemAge  18  79 105218  7825 55.39797  5828864
    ##        Std     StdErr      Var        USS       CSS       CV   TValue
    ## 1 13.54306 0.01809007 183.4146 1670246692 102798374 25.60925 2923.342
    ## 2 13.55334 0.02470422 183.6931  896802800  55289422 25.63252 2140.346
    ## 3 12.81461 0.02478885 164.2144  838383850  43884151 23.50216 2199.588
    ## 4 12.82301 0.03372959 164.4296  453926762  23764850 23.50461 1617.432
    ## 5 12.50437 0.02825212 156.3592  632179774  30629668 22.56505 1961.436
    ## 6 12.50536 0.03855238 156.3840  339361514  16454259 22.57368 1436.953
    ##   ProbT    Skewness   Kurtosis
    ## 1     0 -0.01670682 -0.9261177
    ## 2     0 -0.01377669 -0.9310965
    ## 3     0 -0.10119272 -0.7639596
    ## 4     0 -0.10664614 -0.7617396
    ## 5     0 -0.18655416 -0.6434898
    ## 6     0 -0.18799294 -0.6429416

``` r
organics@groupby <- list()
result <- cas.simple.summary(organics, inputs = 'DemAge')
result
```

    ## $Summary
    ##   Column Min Max       N  NMiss     Mean      Sum      Std     StdErr
    ## 1 DemAge  18  79 1574340 114608 53.79715 84695008 13.20573 0.01052479
    ##        Var        USS       CSS       CV   TValue ProbT    Skewness
    ## 1 174.3914 4830901392 274551188 24.54727 5111.472     0 -0.07982471
    ##     Kurtosis
    ## 1 -0.8439776

Histograms
----------

``` r
loadActionSet(conn, 'dataPreprocess')
```

    ## NOTE: Added action set 'dataPreprocess'.

    ## NOTE: Information for action set 'dataPreprocess':

    ## NOTE:    dataPreprocess

    ## NOTE:       rustats - Computes robust univariate statistics, centralized moments, quantiles, and frequency distribution statistics

    ## NOTE:       impute - Performs data matrix (variable) imputation

    ## NOTE:       outlier - Performs outlier detection and treatment

    ## NOTE:       binning - Performs unsupervised variable discretization

    ## NOTE:       discretize - Performs supervised and unsupervised variable discretization

    ## NOTE:       catTrans - Groups and encodes categorical variables using unsupervised and supervised grouping techniques

    ## NOTE:       histogram - Generates histogram bins and simple bin-based statistics for numeric variables

    ## NOTE:       transform - Performs pipelined variable imputation, outlier detection and treatment, functional transformation, binning, and robust univariate statistics to evaluate the quality of the transformation

    ## NOTE:       kde - Computes kernel density estimation

    ## NOTE:       highCardinality - Performs randomized cardinality estimation

``` r
result <- cas.dataPreprocess.histogram(
  organics, 
  reqpacks = list(list(nicebinning = FALSE, nbins = 10)),
  inputs = c('Purchase_3mon')
  )
result['BinDetails']
```

    ## $BinDetails
    ##         Variable BinSetId BinId BinLowerBnd BinUpperBnd BinWidth NInBin
    ## 1  purchase_3mon        1     1     698.440     747.402   48.962     45
    ## 2  purchase_3mon        1     2     747.402     796.364   48.962   1802
    ## 3  purchase_3mon        1     3     796.364     845.326   48.962  28782
    ## 4  purchase_3mon        1     4     845.326     894.288   48.962 193828
    ## 5  purchase_3mon        1     5     894.288     943.250   48.962 529051
    ## 6  purchase_3mon        1     6     943.250     992.212   48.962 597872
    ## 7  purchase_3mon        1     7     992.212    1041.174   48.962 279514
    ## 8  purchase_3mon        1     8    1041.174    1090.136   48.962  53719
    ## 9  purchase_3mon        1     9    1090.136    1139.098   48.962   4214
    ## 10 purchase_3mon        1    10    1139.098    1188.060   48.962    121
    ##         Mean      Std     Min     Max MidPoint      Percent
    ## 1   735.9204 11.62972  698.44  747.30  722.921  0.002664380
    ## 2   783.9022 10.76720  747.72  796.36  771.883  0.106693634
    ## 3   830.0947 12.08115  796.37  845.32  820.845  1.704137724
    ## 4   875.7615 13.15566  845.33  894.28  869.807 11.476256226
    ## 5   921.1567 13.78227  894.29  943.24  918.769 31.324291808
    ## 6   966.3418 13.86998  943.25  992.21  967.731 35.399076822
    ## 7  1011.6947 13.36085  992.22 1041.17 1016.693 16.549591817
    ## 8  1057.3838 12.42920 1041.18 1090.12 1065.655  3.180618941
    ## 9  1103.7459 11.36581 1090.14 1139.07 1114.617  0.249504425
    ## 10 1151.3658 10.67709 1139.32 1188.06 1163.579  0.007164223

``` r
df = result$BinDetails
barplot(df$Percent, names.arg = df$MidPoint, cex.names = 0.5, 
  xlab='Purchase_3mon', ylab='Percent', col = '#1f77b4')
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_2.png)

``` r
result <- cas.dataPreprocess.histogram(
  organics, 
  reqpacks = list(list(nicebinning = TRUE, nbins = 10)),
  inputs = c('Purchase_3mon')
  )
df = result$BinDetails
barplot(df$Percent, names.arg = df$MidPoint, cex.names = 0.5,
        xlab='Purchase_3mon', ylab='Percent', col = '#1f77b4')
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_3.png)

``` r
result <- cas.dataPreprocess.histogram(
  organics, 
  reqpacks = list(list(nicebinning = TRUE, nbins = 25)),
  inputs = c('Purchase_3mon')
  )
df = result$BinDetails
barplot(df$Percent, names.arg = df$MidPoint, cex.names = 0.5,
        xlab='Purchase_3mon', ylab='Percent', col = '#1f77b4') 
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_4.png)

``` r
result <- cas.dataPreprocess.histogram(
  organics, 
  reqpacks = list(list(binwidth = 50)),
  inputs = c('Purchase_3mon')
  )
df = result$BinDetails
barplot(df$Percent, names.arg = df$MidPoint, cex.names = 0.5,
        xlab='Purchase_3mon', ylab='Percent', col = '#1f77b4')
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_5.png)

``` r
organics@groupby <- c('DemGender', 'DemAgeGroup')
result <- cas.dataPreprocess.histogram(
  organics, 
  reqpacks = list(list(nicebinning = TRUE, nbins = 20)),
  inputs = c('DemAffl')
  )
names(result)
```

    ##  [1] "ByGroup1.BinDetails"  "ByGroup10.BinDetails" "ByGroup11.BinDetails"
    ##  [4] "ByGroup12.BinDetails" "ByGroup2.BinDetails"  "ByGroup3.BinDetails" 
    ##  [7] "ByGroup4.BinDetails"  "ByGroup5.BinDetails"  "ByGroup6.BinDetails" 
    ## [10] "ByGroup7.BinDetails"  "ByGroup8.BinDetails"  "ByGroup9.BinDetails" 
    ## [13] "ByGroupInfo"

``` r
result['ByGroupInfo']
```

    ## $ByGroupInfo
    ##    DemGender DemGender_f DemAgeGroup DemAgeGroup_f       _key_
    ## 1          F           F      middle        middle Fmiddle    
    ## 2          F           F      senior        senior Fsenior    
    ## 3          F           F     unknown       unknown Funknown   
    ## 4          F           F       young         young Fyoung     
    ## 5          M           M      middle        middle Mmiddle    
    ## 6          M           M      senior        senior Msenior    
    ## 7          M           M     unknown       unknown Munknown   
    ## 8          M           M       young         young Myoung     
    ## 9          U           U      middle        middle Umiddle    
    ## 10         U           U      senior        senior Usenior    
    ## 11         U           U     unknown       unknown Uunknown   
    ## 12         U           U       young         young Uyoung

``` r
all_df = list()
all_df[['Gender=Female, AgeGroup=Middle']]  = result$ByGroup1.BinDetails
all_df[['Gender=Female, AgeGroup=Senior']]  = result$ByGroup1.BinDetails
all_df[['Gender=Female, AgeGroup=Unknown']] = result$ByGroup1.BinDetails
all_df[['Gender=Female, AgeGroup=Young']]   = result$ByGroup1.BinDetails

par(mfrow=c(2,2))
for (this_title in names(all_df)){
  barplot(all_df[[this_title]]$Percent, 
          names.arg = all_df[[this_title]]$MidPoint, 
          xlab='DemAffl', ylab='Percent', col = '#1f77b4',
          main = this_title)
  }
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_6.png)

Percentiles
-----------

``` r
cas.builtins.loadActionSet(conn, 'percentile')
```

    ## NOTE: Added action set 'percentile'.

    ## NOTE: Information for action set 'percentile':

    ## NOTE:    percentile

    ## NOTE:       percentile - Calculate quantiles and percentiles

    ## NOTE:       boxPlot - Calculate quantiles, high and low whiskers, and outliers

    ## NOTE:       assess - Assess and compare models

    ## $actionset
    ## [1] "percentile"

``` r
organics@groupby <- list()
cas.percentile.percentile(organics, inputs = 'DemAge')
```

    ## $Percentile
    ##   Variable Pctl Value Converged
    ## 1   DemAge   25    44         1
    ## 2   DemAge   50    54         1
    ## 3   DemAge   75    64         1

``` r
result <- cas.percentile.percentile(
  organics, 
  inputs = 'DemAge', 
  values = seq(5,90,5)
  )
result
```

    ## $Percentile
    ##    Variable Pctl Value Converged
    ## 1    DemAge    5    32         1
    ## 2    DemAge   10    36         1
    ## 3    DemAge   15    39         1
    ## 4    DemAge   20    41         1
    ## 5    DemAge   25    44         1
    ## 6    DemAge   30    46         1
    ## 7    DemAge   35    48         1
    ## 8    DemAge   40    50         1
    ## 9    DemAge   45    52         1
    ## 10   DemAge   50    54         1
    ## 11   DemAge   55    56         1
    ## 12   DemAge   60    58         1
    ## 13   DemAge   65    60         1
    ## 14   DemAge   70    62         1
    ## 15   DemAge   75    64         1
    ## 16   DemAge   80    66         1
    ## 17   DemAge   85    69         1
    ## 18   DemAge   90    72         1

``` r
organics@groupby <- 'DemGender'
result <- cas.percentile.percentile(
  organics, 
  inputs = 'DemAge', 
  values = seq(5,90,5))
result2 <- rbind.bygroups(result)$Percentile
```

``` r
library('ggplot2')
```

    ## Warning: package 'ggplot2' was built under R version 3.4.4

``` r
ggplot(result2, aes(Pctl, Value, shape = factor(DemGender))) +
  geom_point(aes(colour = factor(DemGender)))
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_7.png) 

\#\# Correlations

``` r
cas.simple.correlation(organics)
```

    ## $ByGroup1.CorrSimple
    ##   DemGender       Variable      N        Mean        Sum     StdDev
    ## 1         F        DemAffl 878788    9.003286    7911980   3.573302
    ## 2         F         DemAge 861460   52.880723   45554628  13.546649
    ## 3         F       PromTime 911468    6.502877    5927164   4.601256
    ## 4         F  purchase_3mon 923324  950.006638  877163929  50.119713
    ## 5         F  purchase_6mon 923324 2049.989809 1892804790  70.693589
    ## 6         F  purchase_9mon 923324 3070.037322 2834639140  86.552868
    ## 7         F purchase_12mon 923324 4190.004978 3868732156 100.007506
    ##   Minimum Maximum
    ## 1    0.00   34.00
    ## 2   18.00   79.00
    ## 3    0.00   38.00
    ## 4  698.44 1188.06
    ## 5 1668.77 2370.87
    ## 6 2624.09 3468.72
    ## 7 3704.70 4684.88
    ## 
    ## $ByGroup1.Correlation
    ##   DemGender       Variable       DemAffl       DemAge      PromTime
    ## 1         F        DemAffl  1.0000000000 -0.161425990 -0.0291717995
    ## 2         F         DemAge -0.1614259898  1.000000000  0.2174197676
    ## 3         F       PromTime -0.0291717995  0.217419768  1.0000000000
    ## 4         F  purchase_3mon  0.0010223694 -0.002077419  0.0003438590
    ## 5         F  purchase_6mon  0.0006326970 -0.001380446  0.0000994838
    ## 6         F  purchase_9mon  0.0003432946 -0.001161494 -0.0007596085
    ## 7         F purchase_12mon  0.0004438193 -0.001114877 -0.0007972213
    ##   purchase_3mon purchase_6mon purchase_9mon purchase_12mon  Nobs1  Nobs2
    ## 1   0.001022369  0.0006326970  0.0003432946   0.0004438193 878788 819964
    ## 2  -0.002077419 -0.0013804462 -0.0011614943  -0.0011148772 819964 861460
    ## 3   0.000343859  0.0000994838 -0.0007596085  -0.0007972213 867920 850364
    ## 4   1.000000000  0.7079425116  0.5792577291   0.5011586642 878788 861460
    ## 5   0.707942512  1.0000000000  0.8165240714   0.7071683888 878788 861460
    ## 6   0.579257729  0.8165240714  1.0000000000   0.8659821670 878788 861460
    ## 7   0.501158664  0.7071683888  0.8659821670   1.0000000000 878788 861460
    ##    Nobs3  Nobs4  Nobs5  Nobs6  Nobs7
    ## 1 867920 878788 878788 878788 878788
    ## 2 850364 861460 861460 861460 861460
    ## 3 911468 911468 911468 911468 911468
    ## 4 911468 923324 923324 923324 923324
    ## 5 911468 923324 923324 923324 923324
    ## 6 911468 923324 923324 923324 923324
    ## 7 911468 923324 923324 923324 923324
    ## 
    ## $ByGroup2.CorrSimple
    ##   DemGender       Variable      N        Mean        Sum     StdDev
    ## 1         M        DemAffl 418456    8.484562    3550416   3.253580
    ## 2         M         DemAge 411768   54.535807   22456100  12.817555
    ## 3         M       PromTime 436468    6.596378    2879108   4.672799
    ## 4         M  purchase_3mon 441940  950.025551  419854292  50.066496
    ## 5         M  purchase_6mon 441940 2049.919078  905941237  70.878690
    ## 6         M  purchase_9mon 441940 3069.993460 1356752910  86.757800
    ## 7         M purchase_12mon 441940 4190.040326 1851746422 100.080435
    ##   Minimum Maximum
    ## 1    0.00   31.00
    ## 2   18.00   79.00
    ## 3    0.00   33.00
    ## 4  722.88 1177.53
    ## 5 1729.20 2366.89
    ## 6 2686.81 3454.40
    ## 7 3698.92 4641.13
    ## 
    ## $ByGroup2.Correlation
    ##   DemGender       Variable      DemAffl        DemAge     PromTime
    ## 1         M        DemAffl  1.000000000 -1.191692e-01 -0.030649888
    ## 2         M         DemAge -0.119169182  1.000000e+00  0.202137172
    ## 3         M       PromTime -0.030649888  2.021372e-01  1.000000000
    ## 4         M  purchase_3mon  0.002357243 -1.326359e-03 -0.002264573
    ## 5         M  purchase_6mon  0.003257655 -1.326145e-03 -0.001532980
    ## 6         M  purchase_9mon  0.004398557  2.007199e-05 -0.002873924
    ## 7         M purchase_12mon  0.003142993  3.270245e-04 -0.003998941
    ##   purchase_3mon purchase_6mon purchase_9mon purchase_12mon  Nobs1  Nobs2
    ## 1   0.002357243   0.003257655  4.398557e-03   0.0031429931 418456 390488
    ## 2  -0.001326359  -0.001326145  2.007199e-05   0.0003270245 390488 411768
    ## 3  -0.002264573  -0.001532980 -2.873924e-03  -0.0039989415 413364 406828
    ## 4   1.000000000   0.707110917  5.779358e-01   0.5014760375 418456 411768
    ## 5   0.707110917   1.000000000  8.169025e-01   0.7081884115 418456 411768
    ## 6   0.577935845   0.816902537  1.000000e+00   0.8663233979 418456 411768
    ## 7   0.501476038   0.708188412  8.663234e-01   1.0000000000 418456 411768
    ##    Nobs3  Nobs4  Nobs5  Nobs6  Nobs7
    ## 1 413364 418456 418456 418456 418456
    ## 2 406828 411768 411768 411768 411768
    ## 3 436468 436468 436468 436468 436468
    ## 4 436468 441940 441940 441940 441940
    ## 5 436468 441940 441940 441940 441940
    ## 6 436468 441940 441940 441940 441940
    ## 7 436468 441940 441940 441940 441940
    ## 
    ## $ByGroup3.CorrSimple
    ##   DemGender       Variable      N        Mean        Sum    StdDev Minimum
    ## 1         U        DemAffl 309244    8.191448    2533156  3.099639    0.00
    ## 2         U         DemAge 301112   55.408884   16684280 12.504695   18.00
    ## 3         U       PromTime 319656    6.697575    2140920  4.788380    0.00
    ## 4         U  purchase_3mon 323684  950.089875  307528891 49.918059  716.49
    ## 5         U  purchase_6mon 323684 2050.031286  663562327 70.635981 1741.51
    ## 6         U  purchase_9mon 323684 3069.990046  993706658 86.456886 2702.52
    ## 7         U purchase_12mon 323684 4189.903596 1356204756 99.916124 3752.77
    ##   Maximum
    ## 1   26.00
    ## 2   79.00
    ## 3   39.00
    ## 4 1182.43
    ## 5 2357.42
    ## 6 3447.14
    ## 7 4625.35
    ## 
    ## $ByGroup3.Correlation
    ##   DemGender       Variable       DemAffl        DemAge      PromTime
    ## 1         U        DemAffl  1.0000000000 -0.0394224329 -0.0219043614
    ## 2         U         DemAge -0.0394224329  1.0000000000  0.1824792235
    ## 3         U       PromTime -0.0219043614  0.1824792235  1.0000000000
    ## 4         U  purchase_3mon  0.0026801289  0.0015639725  0.0004798869
    ## 5         U  purchase_6mon  0.0027579853  0.0005000127 -0.0010982083
    ## 6         U  purchase_9mon  0.0006427969  0.0006288963 -0.0027989543
    ## 7         U purchase_12mon -0.0005685855 -0.0002226992 -0.0039098750
    ##   purchase_3mon purchase_6mon purchase_9mon purchase_12mon  Nobs1  Nobs2
    ## 1  0.0026801289  0.0027579853  0.0006427969  -0.0005685855 309244 287812
    ## 2  0.0015639725  0.0005000127  0.0006288963  -0.0002226992 287812 301112
    ## 3  0.0004798869 -0.0010982083 -0.0027989543  -0.0039098750 305444 297464
    ## 4  1.0000000000  0.7066079060  0.5764098308   0.4996927414 309244 301112
    ## 5  0.7066079060  1.0000000000  0.8164008417   0.7069142829 309244 301112
    ## 6  0.5764098308  0.8164008417  1.0000000000   0.8659126012 309244 301112
    ## 7  0.4996927414  0.7069142829  0.8659126012   1.0000000000 309244 301112
    ##    Nobs3  Nobs4  Nobs5  Nobs6  Nobs7
    ## 1 305444 309244 309244 309244 309244
    ## 2 297464 301112 301112 301112 301112
    ## 3 319656 319656 319656 319656 319656
    ## 4 319656 323684 323684 323684 323684
    ## 5 319656 323684 323684 323684 323684
    ## 6 319656 323684 323684 323684 323684
    ## 7 319656 323684 323684 323684 323684
    ## 
    ## $ByGroupInfo
    ##   DemGender DemGender_f _key_
    ## 1         F           F     F
    ## 2         M           M     M
    ## 3         U           U     U

``` r
varlist <- c('DemAffl', 'DemAge', 'purchase_3mon')
cas.simple.correlation(organics,inputs=varlist, simple=FALSE)
```

    ## $ByGroup1.Correlation
    ##   DemGender      Variable      DemAffl       DemAge purchase_3mon  Nobs1
    ## 1         F       DemAffl  1.000000000 -0.161425990   0.001022369 878788
    ## 2         F        DemAge -0.161425990  1.000000000  -0.002077419 819964
    ## 3         F purchase_3mon  0.001022369 -0.002077419   1.000000000 878788
    ##    Nobs2  Nobs3
    ## 1 819964 878788
    ## 2 861460 861460
    ## 3 861460 923324
    ## 
    ## $ByGroup2.Correlation
    ##   DemGender      Variable      DemAffl       DemAge purchase_3mon  Nobs1
    ## 1         M       DemAffl  1.000000000 -0.119169182   0.002357243 418456
    ## 2         M        DemAge -0.119169182  1.000000000  -0.001326359 390488
    ## 3         M purchase_3mon  0.002357243 -0.001326359   1.000000000 418456
    ##    Nobs2  Nobs3
    ## 1 390488 418456
    ## 2 411768 411768
    ## 3 411768 441940
    ## 
    ## $ByGroup3.Correlation
    ##   DemGender      Variable      DemAffl       DemAge purchase_3mon  Nobs1
    ## 1         U       DemAffl  1.000000000 -0.039422433   0.002680129 309244
    ## 2         U        DemAge -0.039422433  1.000000000   0.001563973 287812
    ## 3         U purchase_3mon  0.002680129  0.001563973   1.000000000 309244
    ##    Nobs2  Nobs3
    ## 1 287812 309244
    ## 2 301112 301112
    ## 3 301112 323684
    ## 
    ## $ByGroupInfo
    ##   DemGender DemGender_f _key_
    ## 1         F           F     F
    ## 2         M           M     M
    ## 3         U           U     U

Summarizing Categorical Variables
=================================

Distinct Counts
---------------

``` r
organics <- defCasTable(conn,'organics')
cas.simple.distinct(organics)
```

    ## $Distinct
    ##               Column NDistinct  NMiss Trunc
    ## 1                 ID   1688948      0     0
    ## 2            DemAffl        34  82460     0
    ## 3             DemAge        63 114608     0
    ## 4          DemGender         3      0     0
    ## 5       DemHomeowner         2      0     0
    ## 6        DemAgeGroup         4      0     0
    ## 7         DemCluster        56      0     0
    ## 8             DemReg         6      0     0
    ## 9           DemTVReg        14      0     0
    ## 10          DemFlag1         2      0     0
    ## 11          DemFlag2         2      0     0
    ## 12          DemFlag3         2      0     0
    ## 13          DemFlag4         2      0     0
    ## 14          DemFlag5         2      0     0
    ## 15          DemFlag6         2      0     0
    ## 16          DemFlag7         2      0     0
    ## 17          DemFlag8         2      0     0
    ## 18         PromClass         4      0     0
    ## 19          PromTime        40  21356     0
    ## 20         TargetBuy         2      0     0
    ## 21  Bought_Beverages         2      0     0
    ## 22     Bought_Bakery         2      0     0
    ## 23     Bought_Canned         2      0     0
    ## 24      Bought_Dairy         2      0     0
    ## 25     Bought_Baking         2      0     0
    ## 26     Bought_Frozen         2      0     0
    ## 27       Bought_Meat         2      0     0
    ## 28     Bought_Fruits         2      0     0
    ## 29 Bought_Vegetables         2      0     0
    ## 30   Bought_Cleaners         2      0     0
    ## 31 Bought_PaperGoods         2      0     0
    ## 32     Bought_Others         2      0     0
    ## 33     purchase_3mon     32944      0     0
    ## 34     purchase_6mon     44997      0     0
    ## 35     purchase_9mon     54032      0     0
    ## 36    purchase_12mon     61444      0     0

``` r
cas.simple.distinct(organics, maxnvals=500)
```

    ## $Distinct
    ##               Column NDistinct  NMiss Trunc
    ## 1                 ID       500      0     1
    ## 2            DemAffl        34  82460     0
    ## 3             DemAge        63 114608     0
    ## 4          DemGender         3      0     0
    ## 5       DemHomeowner         2      0     0
    ## 6        DemAgeGroup         4      0     0
    ## 7         DemCluster        56      0     0
    ## 8             DemReg         6      0     0
    ## 9           DemTVReg        14      0     0
    ## 10          DemFlag1         2      0     0
    ## 11          DemFlag2         2      0     0
    ## 12          DemFlag3         2      0     0
    ## 13          DemFlag4         2      0     0
    ## 14          DemFlag5         2      0     0
    ## 15          DemFlag6         2      0     0
    ## 16          DemFlag7         2      0     0
    ## 17          DemFlag8         2      0     0
    ## 18         PromClass         4      0     0
    ## 19          PromTime        40  21356     0
    ## 20         TargetBuy         2      0     0
    ## 21  Bought_Beverages         2      0     0
    ## 22     Bought_Bakery         2      0     0
    ## 23     Bought_Canned         2      0     0
    ## 24      Bought_Dairy         2      0     0
    ## 25     Bought_Baking         2      0     0
    ## 26     Bought_Frozen         2      0     0
    ## 27       Bought_Meat         2      0     0
    ## 28     Bought_Fruits         2      0     0
    ## 29 Bought_Vegetables         2      0     0
    ## 30   Bought_Cleaners         2      0     0
    ## 31 Bought_PaperGoods         2      0     0
    ## 32     Bought_Others         2      0     0
    ## 33     purchase_3mon       500      0     1
    ## 34     purchase_6mon       500      0     1
    ## 35     purchase_9mon       500      0     1
    ## 36    purchase_12mon       500      0     1

``` r
cas.simple.distinct(
  organics, 
  maxnvals=500, 
  casout = list(name = 'distinctOutput'))
```

    ## $OutputCasTables
    ##   casLib           Name Rows Columns
    ## 1    HPS distinctOutput   36       4

``` r
result <- defCasTable(conn, 'distinctOutput')
head(result)
```

    ##       _Column_ _NDis_ _NMiss_ _Truncated_
    ## 1           ID      0       0           1
    ## 2      DemAffl     34   82460           0
    ## 3       DemAge     63  114608           0
    ## 4    DemGender      3       0           0
    ## 5 DemHomeowner      2       0           0
    ## 6  DemAgeGroup      4       0           0

``` r
out1 <- cas.table.columnInfo(organics)$ColumnInfo
out2 <- cas.simple.distinct(organics,maxnvals=1000)$Distinct
out3 <- cbind(out1, out2)

varname <- out3$Column
vartype <- out3$Type
varNDistinct <- out3$NDistinct
catList <- c()
contList <- c()
otherList <- c()

for (i in 1:length(varname)){
  if (vartype[i] == 'char' & varNDistinct[i] <= 128)
    catList <- c(catList, varname[i])
  else if (vartype[i] == 'double' & varNDistinct[i] <= 16)
    catList <- c(catList, varname[i])
  else if (vartype[i] == 'double' & varNDistinct[i] > 16)
    contList <- c(contList, varname[i])
  else
    otherList <- c(otherList, varname[i])
}

varlist <- list(cats = catList,conts = contList, others = otherList)
varlist
```

    ## $cats
    ##  [1] "DemGender"         "DemHomeowner"      "DemAgeGroup"      
    ##  [4] "DemCluster"        "DemReg"            "DemTVReg"         
    ##  [7] "DemFlag1"          "DemFlag2"          "DemFlag3"         
    ## [10] "DemFlag4"          "DemFlag5"          "DemFlag6"         
    ## [13] "DemFlag7"          "DemFlag8"          "PromClass"        
    ## [16] "TargetBuy"         "Bought_Beverages"  "Bought_Bakery"    
    ## [19] "Bought_Canned"     "Bought_Dairy"      "Bought_Baking"    
    ## [22] "Bought_Frozen"     "Bought_Meat"       "Bought_Fruits"    
    ## [25] "Bought_Vegetables" "Bought_Cleaners"   "Bought_PaperGoods"
    ## [28] "Bought_Others"    
    ## 
    ## $conts
    ## [1] "DemAffl"        "DemAge"         "PromTime"       "purchase_3mon" 
    ## [5] "purchase_6mon"  "purchase_9mon"  "purchase_12mon"
    ## 
    ## $others
    ## [1] "ID"

Frequency
---------

``` r
cas.simple.freq(organics, inputs = "TargetBuy")
```

    ## $Frequency
    ##      Column    CharVar     FmtVar Level Frequency
    ## 1 TargetBuy Bought     Bought         1    418380
    ## 2 TargetBuy No         No             2   1270568

``` r
cas.simple.freq(organics["TargetBuy"])
```

    ## $Frequency
    ##      Column    CharVar     FmtVar Level Frequency
    ## 1 TargetBuy Bought     Bought         1    418380
    ## 2 TargetBuy No         No             2   1270568

``` r
out <- cas.simple.freq(organics['TargetBuy'])
df <- out$Frequency
barplot(df$Frequency, names.arg = df$FmtVar, 
        xlab='TargetBuy', ylab='Frequency', col = '#1f77b4')
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_8.png)

``` r
cas.simple.freq(organics['TargetBuy','DemAgeGroup','DemHomeowner'])
```

    ## $Frequency
    ##        Column    CharVar     FmtVar Level Frequency
    ## 1 DemAgeGroup middle     middle         1    991800
    ## 2 DemAgeGroup senior     senior         2    532456
    ## 3 DemAgeGroup unknown    unknown        3    114608
    ## 4 DemAgeGroup young      young          4     50084

``` r
out <- cas.simple.freq(organics['DemAge'], includemissing = FALSE)
df <- out$Frequency
barplot(df$Frequency, names.arg = df$NumVar, 
        xlab='Age', ylab='Frequency', col = '#1f77b4')
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_9.png)

``` r
out <- cas.simple.freq(organics['DemAge'], includemissing = TRUE)
df <- out$Frequency
barplot(df$Frequency, names.arg = df$NumVar, 
        xlab='Age', ylab='Frequency', col = '#1f77b4')
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_10.png)

Top K
-----

``` r
cas.simple.topK(organics['purchase_12mon'], topk=5, bottomk=0)
```

    ## $Topk
    ##           Column       FmtVar Rank
    ## 1 purchase_12mon      4684.88    1
    ## 2 purchase_12mon      4653.49    2
    ## 3 purchase_12mon      4646.95    3
    ## 4 purchase_12mon      4641.13    4
    ## 5 purchase_12mon      4631.91    5
    ## 
    ## $TopkMisc
    ##           Column     N TruncatedTopk TruncatedBtmk ScoreOther
    ## 1 purchase_12mon 61444             0             0        NaN

``` r
result <- cas.simple.topK(organics['purchase_12mon'], topk=5, bottomk=0)
for (df in names(result)){
  dfnames <- paste(names(result[[df]]),collapse=' ')
  print(paste(df, ' table has: ', dfnames))
}
```

    ## [1] "Topk  table has:  Column FmtVar Rank"
    ## [1] "TopkMisc  table has:  Column N TruncatedTopk TruncatedBtmk ScoreOther"

``` r
cas.simple.topK(organics[c('purchase_12mon','DemAge')], topk=5, bottomk=5)
```

    ## $Topk
    ##            Column       FmtVar  Rank
    ## 1  purchase_12mon      4684.88     1
    ## 2  purchase_12mon      4653.49     2
    ## 3  purchase_12mon      4646.95     3
    ## 4  purchase_12mon      4641.13     4
    ## 5  purchase_12mon      4631.91     5
    ## 6  purchase_12mon         3826 61444
    ## 7  purchase_12mon         3833 61443
    ## 8  purchase_12mon         3849 61442
    ## 9  purchase_12mon         3850 61441
    ## 10 purchase_12mon         3851 61440
    ## 11         DemAge           79     1
    ## 12         DemAge           78     2
    ## 13         DemAge           77     3
    ## 14         DemAge           76     4
    ## 15         DemAge           75     5
    ## 16         DemAge            .    63
    ## 17         DemAge           18    62
    ## 18         DemAge           19    61
    ## 19         DemAge           20    60
    ## 20         DemAge           21    59
    ## 
    ## $TopkMisc
    ##           Column     N TruncatedTopk TruncatedBtmk ScoreOther
    ## 1 purchase_12mon 61444             0             0        NaN
    ## 2         DemAge    63             0             0        NaN

``` r
cas.simple.topK(organics, inputs = 'DemTVReg', 
                topk=3, bottomk=3, 
                weight='DemAffl', agg='mean')
```

    ## $Topk
    ##     Column       FmtVar Rank    Score
    ## 1 DemTVReg Unknown         1 8.968397
    ## 2 DemTVReg East            2 8.921506
    ## 3 DemTVReg Yorkshire       3 8.752555
    ## 4 DemTVReg Ulster         14 8.493976
    ## 5 DemTVReg N East         13 8.512684
    ## 6 DemTVReg N West         12 8.533601
    ## 
    ## $TopkMisc
    ##     Column  N TruncatedTopk TruncatedBtmk ScoreOther
    ## 1 DemTVReg 14             0             0   8.716037

``` r
cas.simple.topK(organics, inputs = 'DemTVReg', 
                topk=3, bottomk=3, 
                weight='purchase_3mon', agg='sum')
```

    ## $Topk
    ##     Column       FmtVar Rank     Score
    ## 1 DemTVReg London          1 446844304
    ## 2 DemTVReg Midlands        2 225511094
    ## 3 DemTVReg S & S East      3 176541245
    ## 4 DemTVReg Border         14  14653126
    ## 5 DemTVReg Ulster         13  19199923
    ## 6 DemTVReg N Scot         12  23743227
    ## 
    ## $TopkMisc
    ##     Column  N TruncatedTopk TruncatedBtmk ScoreOther
    ## 1 DemTVReg 14             0             0  698054193

Cross Tabulations
-----------------

``` r
result <- cas.simple.crossTab(organics, row='DemAgeGroup', col='DemGender')
result
```

    ## $Crosstab
    ##   DemAgeGroup   Col1   Col2   Col3
    ## 1      middle 550240 258172 183388
    ## 2      senior 278312 143412 110732
    ## 3     unknown  61864  30172  22572
    ## 4       young  32908  10184   6992

``` r
result$Crosstab@names
```

    ## [1] "DemAgeGroup" "Col1"        "Col2"        "Col3"

``` r
result$Crosstab@col.labels
```

    ## [1] ""  "F" "M" "U"

``` r
df <- result$Crosstab@df
names(df)[-1] <- result$Crosstab@col.labels[-1]
df
```

    ##   DemAgeGroup      F      M      U
    ## 1      middle 550240 258172 183388
    ## 2      senior 278312 143412 110732
    ## 3     unknown  61864  30172  22572
    ## 4       young  32908  10184   6992

``` r
cas.simple.crossTab(organics, row='DemAgeGroup', col='DemGender',
                    association=TRUE, chisq=TRUE)
```

    ## $Association
    ##                            Statistic       Value          ASE     LowerCL
    ## 1                              Gamma 0.022513030 1.227359e-03 0.020107452
    ## 2                    Kendall's Tau-B 0.012912614 7.057156e-04 0.011529437
    ## 3                     Stuart's Tau-c 0.011091773 6.061264e-04 0.009903787
    ## 4                                            NaN          NaN         NaN
    ## 5                      Somers' D C|R 0.013437504 7.345550e-04 0.011997802
    ## 6                      Somers' D R|C 0.012408227 6.780319e-04 0.011079309
    ## 7                                            NaN          NaN         NaN
    ## 8              Lambda Asymmetric C|R 0.000000000 0.000000e+00 0.000000000
    ## 9              Lambda Asymmetric R|C 0.000000000 0.000000e+00 0.000000000
    ## 10                  Lambda Symmetric 0.000000000 0.000000e+00 0.000000000
    ## 11                                           NaN          NaN         NaN
    ## 12       Uncertainty Coefficient C|R 0.001286326 3.877649e-05 0.001210325
    ## 13       Uncertainty Coefficient R|C 0.001331922 4.011861e-05 0.001253291
    ## 14 Uncertainty Coefficient Symmetric 0.001308727 3.943370e-05 0.001231439
    ##        UpperCL
    ## 1  0.024918609
    ## 2  0.014295791
    ## 3  0.012279758
    ## 4          NaN
    ## 5  0.014877205
    ## 6  0.013737145
    ## 7          NaN
    ## 8  0.000000000
    ## 9  0.000000000
    ## 10 0.000000000
    ## 11         NaN
    ## 12 0.001362327
    ## 13 0.001410553
    ## 14 0.001386016
    ## 
    ## $ChiSq
    ##                     Statistic DF    Value Prob
    ## 1                  Chi-Square  6 4284.485    0
    ## 2 Likelihood Ratio Chi-Square  6 4334.500    0
    ## 
    ## $Crosstab
    ##   DemAgeGroup   Col1   Col2   Col3
    ## 1      middle 550240 258172 183388
    ## 2      senior 278312 143412 110732
    ## 3     unknown  61864  30172  22572
    ## 4       young  32908  10184   6992

``` r
result <- cas.simple.crossTab(organics, 
                              row='DemAgeGroup', 
                              col='DemGender')
df <- result$Crosstab@df

# use the reshape2 package to transform data from wide format
# to long format
library(reshape2) 
```

    ## Warning: package 'reshape2' was built under R version 3.4.4

``` r
names(df)[-1] <- result$Crosstab@col.labels[-1]
df_melt <- melt(df, id.vars = c('DemAgeGroup'))
names(df_melt)[-1] = c('DemGender', 'Frequency')

library(ggplot2)
ggplot(df_melt, aes(DemGender, Frequency, fill = DemAgeGroup)) + 
  geom_bar(position = 'dodge', stat='identity') + 
  theme(legend.direction = 'vertical') +
  scale_fill_brewer(palette = 'Set1')
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/7_11.png)

``` r
organics <- defCasTable(conn,'organics')
result <- cas.simple.crossTab(
  organics, row='DemAgeGroup', col='DemGender',
  weight='purchase_3mon', aggregators='sum')
df <- result$Crosstab@df
names(df)[-1] <- result$Crosstab@col.labels[-1]
df
```

    ##   DemAgeGroup         F         M         U
    ## 1      middle 522744714 245281125 174232600
    ## 2      senior 264361216 136232205 105216284
    ## 3     unknown  58781801  28665861  21439594
    ## 4       young  31276199   9675101   6640414

``` r
result <- cas.simple.crossTab(
  organics, row='DemAgeGroup', col='purchase_3mon', 
  colnbins=4, chisq=TRUE)
df <- result$Crosstab@df
names(df)[-1] <- result$Crosstab@col.labels[-1]
df
```

    ##   DemAgeGroup ( 500,  700] ( 700,  900] ( 900, 1100] (1100, 1300]
    ## 1      middle            1       157358       833045         1396
    ## 2      senior            0        84901       446820          735
    ## 3     unknown            0        18225        96230          153
    ## 4       young            0         7835        42188           61

``` r
result[['ChiSq']]
```

    ##                     Statistic DF    Value      Prob
    ## 1                  Chi-Square  9 6.223867 0.7173205
    ## 2 Likelihood Ratio Chi-Square  9 6.641251 0.6744137

``` r
organics@groupby = 'DemReg'
result <- cas.simple.crossTab(
  organics, row='DemAgeGroup', col='DemGender', chisq=TRUE)
for (table_name in names(result)){
  df <- result[[table_name]]
  if (grepl('ChiSq',table_name)) 
    print(df[df$Statistic == 'Chi-Square',])
}
```

    ##     DemReg  Statistic DF    Value Prob
    ## 1 Midlands Chi-Square  6 2256.131    0
    ##   DemReg  Statistic DF    Value          Prob
    ## 1  North Chi-Square  6 1096.161 1.412025e-233
    ##     DemReg  Statistic DF    Value        Prob
    ## 1 Scottish Chi-Square  6 146.5636 4.12041e-29
    ##       DemReg  Statistic DF    Value Prob
    ## 1 South East Chi-Square  6 2959.659    0
    ##       DemReg  Statistic DF    Value          Prob
    ## 1 South West Chi-Square  6 1137.124 1.935146e-242
    ##    DemReg  Statistic DF    Value          Prob
    ## 1 Unknown Chi-Square  6 913.5657 4.385563e-194

``` r
organics@groupby = list()
```

Variable Transformation and Dimension Reduction
===============================================

``` r
cas.builtins.loadActionSet(conn, 'dataPreprocess')
```

    ## NOTE: Added action set 'dataPreprocess'.

    ## NOTE: Information for action set 'dataPreprocess':

    ## NOTE:    dataPreprocess

    ## NOTE:       rustats - Computes robust univariate statistics, centralized moments, quantiles, and frequency distribution statistics

    ## NOTE:       impute - Performs data matrix (variable) imputation

    ## NOTE:       outlier - Performs outlier detection and treatment

    ## NOTE:       binning - Performs unsupervised variable discretization

    ## NOTE:       discretize - Performs supervised and unsupervised variable discretization

    ## NOTE:       catTrans - Groups and encodes categorical variables using unsupervised and supervised grouping techniques

    ## NOTE:       histogram - Generates histogram bins and simple bin-based statistics for numeric variables

    ## NOTE:       transform - Performs pipelined variable imputation, outlier detection and treatment, functional transformation, binning, and robust univariate statistics to evaluate the quality of the transformation

    ## NOTE:       kde - Computes kernel density estimation

    ## NOTE:       highCardinality - Performs randomized cardinality estimation

    ## $actionset
    ## [1] "dataPreprocess"

Variable Binning
----------------

``` r
result <- cas.dataPreprocess.binning(
  organics, inputs = 'purchase_3mon', tech='bucket',
  casout = list(name = 'binnedData', replace = TRUE),
  nBinsArray = 10)
out_data <- defCasTable(conn, 'binnedData')
head(out_data, n = 10L)
```

    ##    BIN_purchase_3mon
    ## 1                  5
    ## 2                  7
    ## 3                  7
    ## 4                  8
    ## 5                  4
    ## 6                  6
    ## 7                  5
    ## 8                  7
    ## 9                  6
    ## 10                 6

``` r
result
```

    ## $BinDetails
    ##         Variable BinId BinLowerBnd BinUpperBnd BinWidth NInBin      Mean
    ## 1  purchase_3mon     1     698.440     747.402   48.962     45  735.9204
    ## 2  purchase_3mon     2     747.402     796.364   48.962   1802  783.9022
    ## 3  purchase_3mon     3     796.364     845.326   48.962  28782  830.0947
    ## 4  purchase_3mon     4     845.326     894.288   48.962 193828  875.7615
    ## 5  purchase_3mon     5     894.288     943.250   48.962 529051  921.1567
    ## 6  purchase_3mon     6     943.250     992.212   48.962 597872  966.3418
    ## 7  purchase_3mon     7     992.212    1041.174   48.962 279514 1011.6947
    ## 8  purchase_3mon     8    1041.174    1090.136   48.962  53719 1057.3838
    ## 9  purchase_3mon     9    1090.136    1139.098   48.962   4214 1103.7459
    ## 10 purchase_3mon    10    1139.098    1188.060   48.962    121 1151.3658
    ##         Std     Min     Max
    ## 1  11.62972  698.44  747.30
    ## 2  10.76720  747.72  796.36
    ## 3  12.08115  796.37  845.32
    ## 4  13.15566  845.33  894.28
    ## 5  13.78227  894.29  943.24
    ## 6  13.86998  943.25  992.21
    ## 7  13.36085  992.22 1041.17
    ## 8  12.42920 1041.18 1090.12
    ## 9  11.36581 1090.14 1139.07
    ## 10 10.67709 1139.32 1188.06
    ## 
    ## $OutputCasTables
    ##   casLib       Name    Rows Columns
    ## 1    HPS binnedData 1688948       1
    ## 
    ## $VarTransInfo
    ##        Variable         ResultVar NBins
    ## 1 purchase_3mon BIN_purchase_3mon    10

``` r
result <- cas.dataPreprocess.binning(
  organics, 
  inputs = c('purchase_3mon', 'purchase_6mon', 
             'purchase_9mon', 'purchase_12mon'),
  tech='bucket',
  casout = list(name = 'binnedData', replace = TRUE),
  nBinsArray = c(4, 10, 20, 6))
out_data <- defCasTable(conn, 'binnedData')
head(out_data, n = 10L)
```

    ##    BIN_purchase_12mon BIN_purchase_3mon BIN_purchase_6mon
    ## 1                   4                 2                 5
    ## 2                   4                 3                 7
    ## 3                   4                 3                 6
    ## 4                   4                 3                 7
    ## 5                   3                 2                 6
    ## 6                   4                 3                 7
    ## 7                   3                 2                 6
    ## 8                   3                 3                 6
    ## 9                   3                 3                 5
    ## 10                  4                 3                 6
    ##    BIN_purchase_9mon
    ## 1                 12
    ## 2                 14
    ## 3                 12
    ## 4                 13
    ## 5                 10
    ## 6                 13
    ## 7                  9
    ## 8                 12
    ## 9                  9
    ## 10                12

``` r
result <- cas.dataPreprocess.binning(
  organics, 
  inputs = c('purchase_3mon', 'purchase_6mon', 
             'purchase_9mon', 'purchase_12mon'),
  tech= 'Quantile',
  casout = list(name = 'binnedData2', replace = TRUE),
  copyallvars = TRUE,
  nBinsArray = c(4, 4, 4, 4))
out_data2 <- defCasTable(conn, 'binnedData2')
names(out_data2)
```

    ##  [1] "ID"                 "DemAffl"            "DemAge"            
    ##  [4] "DemGender"          "DemHomeowner"       "DemAgeGroup"       
    ##  [7] "DemCluster"         "DemReg"             "DemTVReg"          
    ## [10] "DemFlag1"           "DemFlag2"           "DemFlag3"          
    ## [13] "DemFlag4"           "DemFlag5"           "DemFlag6"          
    ## [16] "DemFlag7"           "DemFlag8"           "PromClass"         
    ## [19] "PromTime"           "TargetBuy"          "Bought_Beverages"  
    ## [22] "Bought_Bakery"      "Bought_Canned"      "Bought_Dairy"      
    ## [25] "Bought_Baking"      "Bought_Frozen"      "Bought_Meat"       
    ## [28] "Bought_Fruits"      "Bought_Vegetables"  "Bought_Cleaners"   
    ## [31] "Bought_PaperGoods"  "Bought_Others"      "purchase_3mon"     
    ## [34] "purchase_6mon"      "purchase_9mon"      "purchase_12mon"    
    ## [37] "BIN_purchase_12mon" "BIN_purchase_3mon"  "BIN_purchase_6mon" 
    ## [40] "BIN_purchase_9mon"

``` r
cas.simple.crossTab(out_data2, row='bin_purchase_3mon', col='bin_purchase_12mon')
```

    ## $Crosstab
    ##   BIN_purchase_3mon   Col1   Col2   Col3   Col4
    ## 1                 1 203175 117851  70718  30483
    ## 2                 2 117668 124401 109169  70929
    ## 3                 3  71142 108972 124566 117614
    ## 4                 4  30231  70984 117812 203233

``` r
result <- cas.dataPreprocess.binning(
  organics, 
  inputs = 'purchase_3mon',
  tech = 'bucket',
  casout = list(name = 'binnedData2', replace = TRUE),
  code = list(comment = TRUE, tabform = TRUE),
  nBinsArray = 10)
names(result)
```

    ## [1] "BinDetails"      "CodeGen"         "OutputCasTables" "VarTransInfo"

``` r
# score code is saved in this table
# df = result['CodeGen']
df = result$CodeGen 
```

Variable Imputation
-------------------

``` r
df <- cas.simple.distinct(organics)$Distinct
df[df$NMiss > 0,]
```

    ##      Column NDistinct  NMiss Trunc
    ## 2   DemAffl        34  82460     0
    ## 3    DemAge        63 114608     0
    ## 19 PromTime        40  21356     0

``` r
cas.dataPreprocess.impute(organics, inputs = 'PromTime')
```

    ## $ImputeInfo
    ##   Variable ImputeTech    ResultVar       N NMiss ImputedValueContinuous
    ## 1 PromTime       Mean IMP_PromTime 1667592 21356                6.56467

``` r
cas.dataPreprocess.impute(
  organics, 
  methodcontinuous = 'Median',
  copyallvars = TRUE,
  casout = list(name = 'cas.imputedData1', replace=TRUE),
  inputs = 'PromTime')
```

    ## $ImputeInfo
    ##   Variable ImputeTech    ResultVar       N NMiss ImputedValueContinuous
    ## 1 PromTime     Median IMP_PromTime 1667592 21356                      5
    ## 
    ## $OutputCasTables
    ##   casLib             Name    Rows Columns
    ## 1    HPS cas.imputedData1 1688948      37

``` r
cas.dataPreprocess.impute(
  organics, 
  methodcontinuous = 'Value',
  valuescontinuous = 0,
  copyallvars = TRUE,
  casout = list(name = 'cas.imputedData1', replace=TRUE),
  inputs = 'PromTime')
```

    ## $ImputeInfo
    ##   Variable ImputeTech    ResultVar       N NMiss ImputedValueContinuous
    ## 1 PromTime      Value IMP_PromTime 1667592 21356                      0
    ## 
    ## $OutputCasTables
    ##   casLib             Name    Rows Columns
    ## 1    HPS cas.imputedData1 1688948      37

``` r
result <- cas.dataPreprocess.impute(
  organics, 
  methodcontinuous = 'Value',
  valuescontinuous = 0,
  copyallvars = TRUE,
  casout = list(name = 'cas.imputedData1', replace=TRUE),
  code = list(comment = TRUE, tabform = TRUE),
  inputs = 'PromTime') 
result$CodeGen
```

    ##                                                                                    SASCode
    ## 1          /*-----------------------------------------------------------------------------
    ## 2                            SAS Code Generated by Cloud Analytic Server for Impute Action
    ## 3            Date                                                     : 26Jul2018:23:48:05
    ## 4                             Number of variables                                      : 1
    ## 5          -----------------------------------------------------------------------------*/
    ## 6                                                                                         
    ## 7                                                                             _ngbys_ = 1;
    ## 8                                                                              _igby_ = 0;
    ## 9                                                                        _tnn_ntrans_ = 1;
    ## 10                                                                                        
    ## 11                                                           _fuzcmp_ = 2.22044604925e-10;
    ## 12                                                                                        
    ## 13                                                 array _tnn_vnames_{1}   IMP_PromTime ; 
    ## 14                                                                                        
    ## 15                                                      array _vnn_names_{1}   PromTime ; 
    ## 16                                                                                        
    ## 17                                         array _tnn_ntransvars_{1}  _temporary_   (1 ); 
    ## 18                                                                                        
    ## 19                                          array _tv_nn_indices_{1}  _temporary_   (1 ); 
    ## 20                                                                                        
    ## 21                                                                       IMP_PromTime = .;
    ## 22                                                                                        
    ## 23                                        array _tnn_imputetype_{1}   _temporary_   (8 ); 
    ## 24                                                                                        
    ## 25                                                                                        
    ## 26                                  array _tnn_imputeuniquevals_{1}   _temporary_   (0 ); 
    ## 27                                                                                        
    ## 28                                                                                        
    ## 29                                          /*---------Iterate and score----------------*/
    ## 30                                                                                        
    ## 31                                            /*---------Count variables----------------*/
    ## 32                                                                               _ct_ = 0;
    ## 33                                                                          _trimmed_ = 0;
    ## 34                                                                            _impct_ = 0;
    ## 35                                                                                        
    ## 36                                                             do _i_ = 1 to _tnn_ntrans_;
    ## 37                                                    do _j_ = 1 to _tnn_ntransvars_{_i_};
    ## 38                                                                                        
    ## 39                                                    if (_tnn_imputetype_{_i_} ~= 0) then
    ## 40                                                                            _impct_ + 1;
    ## 41                                                                               _ct_ + 1;
    ## 42                                          _numval_ = _vnn_names_{_tv_nn_indices_{_ct_}};
    ## 43                                                                                        
    ## 44                                         /*-------Apply Imputation phase--------------*/
    ## 45                                                               if missing(_numval_) then
    ## 46                                                                                     do;
    ## 47                                                       if _tnn_imputetype_{_i_} = 0 then
    ## 48                                                                    goto _impute_done1_;
    ## 49                            else _numval_ = _tnn_imputeuniquevals_{1 *_igby_ + _impct_};
    ## 50                                                                                    end;
    ## 51                                                                        _impute_done1_:;
    ## 52                                                          _tnn_vnames_{_ct_} = _numval_;
    ## 53                                                                                    end;
    ## 54                                                                                    end;
    ## 55                                                                                        
    ## 56    drop _ngbys_ _igby_ _tnn_ntrans_ _fuzcmp_ _ct_ _trimmed_ _impct_ _i_ _j_ _numval_ ; 
    ## 57

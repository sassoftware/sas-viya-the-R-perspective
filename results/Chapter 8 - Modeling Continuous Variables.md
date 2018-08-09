Chapter 8 - Modeling Continuous Variables
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
cas.sessionProp.setSessOpt(conn, caslib = 'casuser')
```

    ## NOTE: 'CASUSER(sasyqi)' is now the active caslib.

    ## list()

``` r
cars <- cas.read.csv(conn, file = 'https://raw.githubusercontent.com/sassoftware/sas-viya-programming/master/data/cars.csv')
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table CARS in caslib CASUSER(sasyqi).

``` r
cas.table.tableInfo(conn,name = 'cars')
```

    ## $TableInfo
    ##   Name Rows Columns IndexedColumns Encoding       CreateTimeFormatted
    ## 1 CARS  428      15              0    utf-8 2018-07-27T00:18:10-04:00
    ##            ModTimeFormatted       AccessTimeFormatted JavaCharSet
    ## 1 2018-07-27T00:18:10-04:00 2018-07-27T00:18:10-04:00        UTF8
    ##   CreateTime    ModTime AccessTime Global Repeated View SourceName
    ## 1 1848284290 1848284290 1848284290      0        0    0           
    ##   SourceCaslib Compressed Creator Modifier    SourceModTimeFormatted
    ## 1                       0  sasyqi          2018-07-27T00:18:10-04:00
    ##   SourceModTime
    ## 1    1848284290

``` r
cas.table.columnInfo(cars)
```

    ## $ColumnInfo
    ##         Column ID    Type RawLength FormattedLength NFL NFD
    ## 1         Make  1 varchar        13              13   0   0
    ## 2        Model  2 varchar        39              39   0   0
    ## 3         Type  3 varchar         6               6   0   0
    ## 4       Origin  4 varchar         6               6   0   0
    ## 5   DriveTrain  5 varchar         5               5   0   0
    ## 6         MSRP  6  double         8              12   0   0
    ## 7      Invoice  7  double         8              12   0   0
    ## 8   EngineSize  8  double         8              12   0   0
    ## 9    Cylinders  9  double         8              12   0   0
    ## 10  Horsepower 10  double         8              12   0   0
    ## 11    MPG_City 11  double         8              12   0   0
    ## 12 MPG_Highway 12  double         8              12   0   0
    ## 13      Weight 13  double         8              12   0   0
    ## 14   Wheelbase 14  double         8              12   0   0
    ## 15      Length 15  double         8              12   0   0

Linear Regression
=================

``` r
cas.builtins.loadActionSet(conn, 'regression')
```

    ## NOTE: Added action set 'regression'.

    ## NOTE: Information for action set 'regression':

    ## NOTE:    regression

    ## NOTE:       glm - Fits linear regression models using the method of least squares

    ## NOTE:       genmod - Fits generalized linear regression models

    ## NOTE:       logistic - Fits logistic regression models

    ## NOTE:       logisticType3 - computes Type 3 or Joint tests that all parameters for an effect are zero.

    ## NOTE:       logisticCode - writes SAS DATA step code for computing predicted values of the fitted model.

    ## NOTE:       genmodScore - creates a table on the server that contains results from scoring observations by using a fitted model.

    ## NOTE:       logisticScore - creates a table on the server that contains results from scoring observations by using a fitted model.

    ## NOTE:       glmScore - creates a table on the server that contains results from scoring observations by using a fitted model.

    ## NOTE:       logisticAssociation - computes indices of rank correlation between predicted probabilities and observed responses used for assessing the predictive ability of a model.

    ## NOTE:       modelMatrix - creates a table on the server that contains the design matrix associated with a given model statement.

    ## $actionset
    ## [1] "regression"

``` r
cas.regression.glm(cars, target = 'MSRP', inputs = c('MPG_City'))
```

    ## $ANOVA
    ##   RowId          Source  DF           SS          MS   FValue        ProbF
    ## 1 MODEL           Model   1  36380901274 36380901274 124.1344 1.783404e-25
    ## 2 ERROR           Error 426 124850717429   293076801      NaN          NaN
    ## 3 TOTAL Corrected Total 427 161231618703         NaN      NaN          NaN
    ## 
    ## $Dimensions
    ##      RowId          Description Value
    ## 1 NEFFECTS    Number of Effects     2
    ## 2   NPARMS Number of Parameters     2
    ## 
    ## $FitStatistics
    ##       RowId Description        Value
    ## 1      RMSE    Root MSE 1.711949e+04
    ## 2   RSQUARE    R-Square 2.256437e-01
    ## 3    ADJRSQ    Adj R-Sq 2.238260e-01
    ## 4       AIC         AIC 8.776260e+03
    ## 5      AICC        AICC 8.776316e+03
    ## 6       SBC         SBC 8.354378e+03
    ## 7 TRAIN_ASE         ASE 2.917073e+08
    ## 
    ## $ModelInfo
    ##         RowId       Description Value
    ## 1        DATA       Data Source  CARS
    ## 2 RESPONSEVAR Response Variable  MSRP
    ## 
    ## $NObs
    ##   RowId                 Description Value
    ## 1 NREAD Number of Observations Read   428
    ## 2 NUSED Number of Observations Used   428
    ## 
    ## $ParameterEstimates
    ##      Effect Parameter DF  Estimate    StdErr    tValue        Probt
    ## 1 Intercept Intercept  1 68124.607 3278.9191  20.77654 1.006169e-66
    ## 2  MPG_City  MPG_City  1 -1762.135  158.1588 -11.14156 1.783404e-25
    ## 
    ## $Timing
    ##            RowId                 Task         Time    RelTime
    ## 1          SETUP    Setup and Parsing 0.0310709476 0.55745622
    ## 2   LEVELIZATION         Levelization 0.0143210888 0.25694035
    ## 3 INITIALIZATION Model Initialization 0.0009770393 0.01752945
    ## 4           SSCP     SSCP Computation 0.0018441677 0.03308695
    ## 5        FITTING        Model Fitting 0.0017559528 0.03150425
    ## 6        CLEANUP              Cleanup 0.0055539608 0.09964582
    ## 7          TOTAL                Total 0.0557370186 1.00000000

``` r
cas.regression.glm(cars, target = 'MSRP', inputs = c('MPG_City'),
                   display = list(names = "ParameterEstimates"))
```

    ## $ParameterEstimates
    ##      Effect Parameter DF  Estimate    StdErr    tValue        Probt
    ## 1 Intercept Intercept  1 68124.607 3278.9191  20.77654 1.006169e-66
    ## 2  MPG_City  MPG_City  1 -1762.135  158.1588 -11.14156 1.783404e-25

``` r
result1 <- cas.regression.glm(
  cars, 
  target = 'MSRP', 
  inputs = c('MPG_City'),
  display = list(names = 'OutputCasTables'),
  output = list(casOut = list(name='MSRPPrediction', replace = TRUE), 
                copyvars = 'all'))
result1
```

    ## $OutputCasTables
    ##            casLib           Name Label Rows Columns
    ## 1 CASUSER(sasyqi) MSRPPrediction        428      16

``` r
cas.simple.summary(result1['Pred'])
```

    ## list()

``` r
cas.regression.glm(
  cars, target = 'MSRP', inputs = c('MPG_City'),
  display = list(names = 'OutputCasTables'),
  output = list(casOut = list(name='MSRPPrediction2', replace = TRUE), 
                copyvars = 'all',
                pred  = 'Predicted_MSRP',
                resid = 'Residual_MSRP',
                lcl = 'LCL_MSRP',
                ucl = 'UCL_MSRP'
                ))
```

    ## $OutputCasTables
    ##            casLib            Name Label Rows Columns
    ## 1 CASUSER(sasyqi) MSRPPrediction2        428      19

``` r
result2 <- defCasTable(conn, "MSRPPrediction2")
```

``` r
out1 <- cas.table.fetch(result2, to = 1000)$Fetch
library(ggplot2)
```

    ## Warning: package 'ggplot2' was built under R version 3.4.4

``` r
ggplot(out1, aes(Predicted_MSRP, Residual_MSRP, 
                 colour = Origin, shape = Origin)) +
geom_point()
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/8_1.png)

``` r
result2[result2$Predicted_MSRP<0,   
        c('Predicted_MSRP','MSRP','MPG_City','Make','Model')]
```

    ##   Predicted_MSRP  MSRP MPG_City   Make
    ## 1      -12933.62 20140       46  Honda
    ## 2      -37603.51 19110       60  Honda
    ## 3      -35841.38 20510       59 Toyota
    ##                                    Model
    ## 1 Civic Hybrid 4dr manual (gas/electric)
    ## 2             Insight 2dr (gas/electric)
    ## 3               Prius 4dr (gas/electric)

``` r
ggplot(out1, aes(MPG_City, MSRP, 
                 colour = Origin, shape = Origin)) +
  geom_point()
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/8_2.png)


``` r
cars@where <- 'MSRP < 100000 and MPG_City < 40'
cas.regression.glm(
  cars, target = 'MSRP', inputs = c('MPG_City'),
  display = list(names = 'FitStatistics'),
  output = list(casOut = list(name='MSRPPrediction2', replace = TRUE), 
                copyvars = 'all',
                pred  = 'Predicted_MSRP',
                resid = 'Residual_MSRP',
                lcl = 'LCL_MSRP',
                ucl = 'UCL_MSRP'
  ))
```

    ## $FitStatistics
    ##       RowId Description        Value
    ## 1      RMSE    Root MSE 1.308617e+04
    ## 2   RSQUARE    R-Square 3.415360e-01
    ## 3    ADJRSQ    Adj R-Sq 3.399645e-01
    ## 4       AIC         AIC 8.406575e+03
    ## 5      AICC        AICC 8.406633e+03
    ## 6       SBC         SBC 7.991661e+03
    ## 7 TRAIN_ASE         ASE 1.704343e+08

``` r
result2 <- defCasTable(conn, 'MSRPPrediction2')
out2 <- cas.table.fetch(result2, to = 1000)$Fetch
ggplot(out2, aes(Predicted_MSRP, Residual_MSRP, 
                 colour = Origin, shape = Origin)) +
  geom_point()
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/8_3.png)

``` r
nomList <- c('Origin','Type','DriveTrain')
contList <- c('MPG_City','Weight','Length')
cas.regression.glm(
  cars, target = 'MSRP', inputs = c(nomList,contList), nominals = nomList,
  display = list(names = 'FitStatistics'),
  output = list(casOut = list(name='MSRPPrediction2', replace = TRUE), 
                copyvars = 'all',
                pred  = 'Predicted_MSRP',
                resid = 'Residual_MSRP',
                lcl = 'LCL_MSRP',
                ucl = 'UCL_MSRP'
  ))
```

    ## $FitStatistics
    ##       RowId Description        Value
    ## 1      RMSE    Root MSE 8.902096e+03
    ## 2   RSQUARE    R-Square 7.025591e-01
    ## 3    ADJRSQ    Adj R-Sq 6.945595e-01
    ## 4       AIC         AIC 8.092009e+03
    ## 5      AICC        AICC 8.092903e+03
    ## 6       SBC         SBC 7.717521e+03
    ## 7 TRAIN_ASE         ASE 7.698848e+07

``` r
cars@where <- ''
cars@groupby <- 'Origin'
out <- cas.simple.summary(cars,inputs=c('MSRP'))
result <- rbind.bygroups(out)
result$Summary[,c('Origin','Column','Mean','Var','Std')]
```

    ##   Origin Column     Mean       Var      Std
    ## 1   Asia   MSRP 24741.32 128166619 11321.07
    ## 2 Europe   MSRP 48349.80 641031529 25318.60
    ## 3    USA   MSRP 28377.44 137170534 11711.98

``` r
cars@groupby <- 'Origin'
cars@where <- 'MSRP < 100000 and MPG_City < 40'
nomList <- c('Type','DriveTrain')
contList <- c('MPG_City','Weight','Length')
tmp <- cas.regression.glm(
  cars, target = 'MSRP', inputs = c(nomList,contList), nominals = nomList,
  display = list(names = "FitStatistics"),
  output = list(casOut = list(name='MSRPPredictionGroupBy', replace = TRUE), 
                copyvars = 'all',
                pred  = 'Predicted_MSRP',
                resid = 'Residual_MSRP',
                lcl = 'LCL_MSRP',
                ucl = 'UCL_MSRP'
  ))
groupBYResult <- defCasTable(conn, 'MSRPPredictionGroupBy')
out <- cas.table.fetch(groupBYResult, to = 1000)$Fetch
ggplot(out, aes(Predicted_MSRP, Residual_MSRP, 
                 colour = Origin, shape = Origin)) +
  geom_point()
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/8_4.png) 

\# Extensions of Ordinary Linear Regression \#\# Generalized Linear Models

``` r
cars@groupby <- list()
cars@where <- ''
cas.regression.genmod(
  cars,
  model = list(depVars = list(name = 'MSRP'),
               effects = c('MPG_City'),
               dist    = 'gamma',
               link    = 'log'
               )
  )
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

    ## $ConvergenceStatus
    ##                                          Reason Status  MaxGradient
    ## 1 Convergence criterion (GCONV=1E-8) satisfied.      0 1.068358e-09
    ## 
    ## $Dimensions
    ##         RowId                Description Value
    ## 1 NDESIGNCOLS          Columns in Design     2
    ## 2    NEFFECTS          Number of Effects     2
    ## 3   MAXEFCOLS         Max Effect Columns     1
    ## 4  DESIGNRANK             Rank of Design     2
    ## 5     OPTPARM Parameters in Optimization     3
    ## 
    ## $FitStatistics
    ##   RowId              Description    Value
    ## 1  M2LL        -2 Log Likelihood 9270.853
    ## 2   AIC  AIC (smaller is better) 9276.853
    ## 3  AICC AICC (smaller is better) 9276.910
    ## 4   SBC  SBC (smaller is better) 9289.031
    ## 
    ## $ModelInfo
    ##         RowId            Description                       Value
    ## 1        DATA            Data Source                        CARS
    ## 2 RESPONSEVAR      Response Variable                        MSRP
    ## 3        DIST           Distribution                       Gamma
    ## 4        LINK          Link Function                         Log
    ## 5        TECH Optimization Technique Newton-Raphson with Ridging
    ## 
    ## $NObs
    ##   RowId                 Description Value
    ## 1 NREAD Number of Observations Read   428
    ## 2 NUSED Number of Observations Used   428
    ## 
    ## $ParameterEstimates
    ##       Effect  Parameter   ParmName DF   Estimate      StdErr      ChiSq
    ## 1  Intercept  Intercept  Intercept  1 11.3077899 0.059610593 35983.9291
    ## 2   MPG_City   MPG_City   MPG_City  1 -0.0473999 0.002800635   286.4454
    ## 3 Dispersion Dispersion Dispersion  1  5.8865740 0.391526276        NaN
    ##      ProbChiSq
    ## 1 0.000000e+00
    ## 2 2.958655e-64
    ## 3          NaN
    ## 
    ## $Timing
    ##            RowId                 Task         Time      RelTime
    ## 1          SETUP    Setup and Parsing 3.530908e-02 4.789094e-01
    ## 2   LEVELIZATION         Levelization 1.441789e-02 1.955549e-01
    ## 3 INITIALIZATION Model Initialization 7.323027e-03 9.932479e-02
    ## 4           SSCP     SSCP Computation 1.405001e-03 1.905652e-02
    ## 5        FITTING        Model Fitting 1.508904e-02 2.046579e-01
    ## 6        CLEANUP              Cleanup 3.099442e-06 4.203882e-05
    ## 7          TOTAL                Total 7.372808e-02 1.000000e+00

``` r
cas.regression.genmod(
  cars,
  model = list(depVars = list(name = 'Cylinders'),
               effects = c('MPG_City'),
               dist    = 'multinomial',
               link    = 'logit'),
  display = list(names = list('ModelInfo', 'ParameterEstimates'))
)
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

    ## $ModelInfo
    ##         RowId               Description                       Value
    ## 1        DATA               Data Source                        CARS
    ## 2 RESPONSEVAR         Response Variable                   Cylinders
    ## 3     NLEVELS Number of Response Levels                           7
    ## 4        DIST              Distribution                 Multinomial
    ## 5    LINKTYPE                 Link Type                  Cumulative
    ## 6        LINK             Link Function                       Logit
    ## 7        TECH    Optimization Technique Newton-Raphson with Ridging
    ## 
    ## $ParameterEstimates
    ##      Effect Parameter     ParmName Outcome Cylinders DF   Estimate
    ## 1 Intercept Intercept  Intercept_3       3         3  1 -60.329075
    ## 2 Intercept Intercept  Intercept_4       4         4  1 -21.461149
    ## 3 Intercept Intercept  Intercept_5       5         5  1 -21.233691
    ## 4 Intercept Intercept  Intercept_6       6         6  1 -16.632445
    ## 5 Intercept Intercept  Intercept_8       8         8  1 -10.988487
    ## 6 Intercept Intercept Intercept_10      10        10  1 -10.314220
    ## 7  MPG_City  MPG_City     MPG_City               NaN  1   1.013934
    ##       StdErr     ChiSq    ProbChiSq
    ## 1 4.82953335 156.04253 8.286542e-36
    ## 2 1.58488726 183.36194 8.941488e-42
    ## 3 1.57576625 181.57975 2.190306e-41
    ## 4 1.33727470 154.69310 1.634032e-35
    ## 5 1.13947020  92.99719 5.236863e-22
    ## 6 1.18654121  75.56264 3.539969e-18
    ## 7 0.07737144 171.73470 3.092446e-39

``` r
out <- cas.regression.genmod(
  cars,
  model = list(depVars = list(name = 'Cylinders'),
               effects = c('MPG_City'),
               dist    = 'multinomial',
               link    = 'logit'),
  output= list(casout  = list(name = 'CylinderPredicted',
                              replace = TRUE),
               copyVars= 'ALL',
               pred = 'Prob_Cylinders')
)
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

``` r
result <- defCasTable(conn, 'CylinderPredicted')
head(result[[c('Prob_Cylinders','_LEVEL_','Cylinders','MPG_City')]], n = 24)
```

    ##    Prob_Cylinders _LEVEL_ Cylinders MPG_City
    ## 1    1.928842e-19       3         6       17
    ## 2    1.442488e-02       4         6       17
    ## 3    1.804258e-02       5         6       17
    ## 4    6.466697e-01       6         6       17
    ## 5    9.980702e-01       8         6       17
    ## 6    9.990158e-01      10         6       17
    ## 7    2.331945e-16       3         4       24
    ## 8    9.465090e-01       4         4       24
    ## 9    9.569226e-01       5         4       24
    ## 10   9.995483e-01       6         4       24
    ## 11   9.999984e-01       8         4       24
    ## 12   9.999992e-01      10         4       24
    ## 13   3.069208e-17       3         4       22
    ## 14   6.996011e-01       4         4       22
    ## 15   7.451397e-01       5         4       22
    ## 16   9.965780e-01       6         4       22
    ## 17   9.999878e-01       8         4       22
    ## 18   9.999938e-01      10         4       22
    ## 19   4.039564e-18       3         6       20
    ## 20   2.346085e-01       4         6       20
    ## 21   2.778780e-01       5         6       20
    ## 22   9.745742e-01       6         6       20
    ## 23   9.999077e-01       8         6       20
    ## 24   9.999530e-01      10         6       20

Regression Trees
----------------

``` r
loadActionSet(conn, 'decisionTree')
```

    ## NOTE: Added action set 'decisionTree'.

    ## NOTE: Information for action set 'decisionTree':

    ## NOTE:    decisionTree

    ## NOTE:       dtreeTrain - Trains a decision tree

    ## NOTE:       dtreeScore - Scores a table using a decision tree model

    ## NOTE:       dtreeSplit - Splits decision tree nodes

    ## NOTE:       dtreePrune - Prune a decision tree

    ## NOTE:       dtreeMerge - Merges decision tree nodes

    ## NOTE:       dtreeCode - Generates DATA step scoring code from a decision tree model

    ## NOTE:       forestTrain - Trains a forest

    ## NOTE:       forestScore - Scores a table using a forest model

    ## NOTE:       forestCode - Generates DATA step scoring code from a forest model

    ## NOTE:       gbtreeTrain - Trains a gradient boosting tree

    ## NOTE:       gbtreeScore - Scores a table using a gradient boosting tree model

    ## NOTE:       gbtreeCode - Generates DATA step scoring code from a gradient boosting tree model

``` r
result <- cas.decisionTree.dtreeTrain(
  cars,
  target = 'MSRP',
  inputs = c('MPG_City'),
  maxlevel = 2,
  casout = list(name = 'treeModel1', replace = TRUE)
)
output1 <- defCasTable(conn, 'treeModel1')
output1[,c('_NodeID_', '_Parent_','_Mean_','_NodeName_',
           '_PBLower0_','_PBUpper0_')]
```

    ##   _NodeID_ _Parent_   _Mean_ _NodeName_ _PBLower0_ _PBUpper0_
    ## 1        0       -1 32774.86   MPG_City        NaN        NaN
    ## 2        1        0 22875.34       MSRP         20         60
    ## 3        2        0 41623.09       MSRP         10         20

``` r
cas.decisiontree.dtreePrune(
  conn, 
  table = 'your_vadliatoin_data',
  modelTable = 'treeModel1',
  casout = list(name = 'pruned_tree')
  )
```

``` r
cas.decisiontree.dtreeScore(
  conn,
  table = 'your_vadliatoin_data',
  modelTable = 'pruned_tree'
  )
```

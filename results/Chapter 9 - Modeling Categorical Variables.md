Chapter 9 - Modeling Categorical Variables
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

Load data

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

``` r
cas.table.tableInfo(conn)
```

    ## $TableInfo
    ##       Name    Rows Columns IndexedColumns Encoding
    ## 1 ORGANICS 1688948      36              0    utf-8
    ##         CreateTimeFormatted          ModTimeFormatted
    ## 1 2018-07-27T01:12:32-04:00 2018-07-27T01:12:32-04:00
    ##         AccessTimeFormatted JavaCharSet CreateTime    ModTime AccessTime
    ## 1 2018-07-27T01:12:32-04:00        UTF8 1848287552 1848287552 1848287552
    ##   Global Repeated View                  SourceName SourceCaslib Compressed
    ## 1      0        0    0 organics_new_vistat.sashdat          HPS          0
    ##   Creator Modifier    SourceModTimeFormatted SourceModTime
    ## 1  sasyqi          2016-08-26T12:16:52-04:00    1787847412

Logistic Regression
===================

``` r
loadActionSet(conn, 'regression')
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

``` r
cas.regression.logistic(
  organics,
  target = 'TargetBuy',
  inputs = c('DemAge', 'Purchase_3mon', 'Purchase_6mon')
)
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

    ## $ConvergenceStatus
    ##                                          Reason Status  MaxGradient
    ## 1 Convergence criterion (GCONV=1E-8) satisfied.      0 4.419893e-08
    ## 
    ## $Dimensions
    ##         RowId                Description Value
    ## 1 NDESIGNCOLS          Columns in Design     4
    ## 2    NEFFECTS          Number of Effects     4
    ## 3   MAXEFCOLS         Max Effect Columns     1
    ## 4  DESIGNRANK             Rank of Design     4
    ## 5     OPTPARM Parameters in Optimization     4
    ## 
    ## $FitStatistics
    ##   RowId              Description   Value
    ## 1  M2LL        -2 Log Likelihood 1608090
    ## 2   AIC  AIC (smaller is better) 1608098
    ## 3  AICC AICC (smaller is better) 1608098
    ## 4   SBC  SBC (smaller is better) 1608147
    ## 
    ## $GlobalTest
    ##               Test DF    ChiSq ProbChiSq
    ## 1 Likelihood Ratio  3 149251.1         0
    ## 
    ## $ModelInfo
    ##         RowId            Description                       Value
    ## 1        DATA            Data Source                    ORGANICS
    ## 2 RESPONSEVAR      Response Variable                   TargetBuy
    ## 3        DIST           Distribution                      Binary
    ## 4        LINK          Link Function                       Logit
    ## 5        TECH Optimization Technique Newton-Raphson with Ridging
    ## 
    ## $NObs
    ##   RowId                 Description   Value
    ## 1 NREAD Number of Observations Read 1688948
    ## 2 NUSED Number of Observations Used 1574340
    ## 
    ## $ParameterEstimates
    ##          Effect     Parameter      ParmName DF      Estimate       StdErr
    ## 1     Intercept     Intercept     Intercept  1  1.755274e+00 5.709205e-02
    ## 2        DemAge        DemAge        DemAge  1 -5.743798e-02 1.584786e-04
    ## 3 purchase_3mon purchase_3mon purchase_3mon  1 -1.617231e-06 5.495954e-05
    ## 4 purchase_6mon purchase_6mon purchase_6mon  1  3.872414e-05 3.890189e-05
    ##          ChiSq     ProbChiSq
    ## 1 9.452326e+02 1.442299e-207
    ## 2 1.313582e+05  0.000000e+00
    ## 3 8.658809e-04  9.765250e-01
    ## 4 9.908827e-01  3.195267e-01
    ## 
    ## $ResponseProfile
    ##   OrderedValue Outcome TargetBuy    Freq Modeled
    ## 1            1  Bought    Bought  387600       *
    ## 2            2      No        No 1186740        
    ## 
    ## $Timing
    ##            RowId                 Task         Time      RelTime
    ## 1          SETUP    Setup and Parsing 2.801013e-02 9.286004e-02
    ## 2   LEVELIZATION         Levelization 4.174995e-02 1.384107e-01
    ## 3 INITIALIZATION Model Initialization 7.716894e-03 2.558328e-02
    ## 4           SSCP     SSCP Computation 3.378510e-02 1.120054e-01
    ## 5        FITTING        Model Fitting 1.501288e-01 4.977118e-01
    ## 6        CLEANUP              Cleanup 3.099442e-06 1.027536e-05
    ## 7          TOTAL                Total 3.016381e-01 1.000000e+00

``` r
cas.regression.logistic(
  organics,
  target = 'TargetBuy',
  inputs = c('DemAge', 'Purchase_3mon', 'Purchase_6mon', 
             'DemGender','DemHomeowner'),
  nominals = c('DemGender', 'DemHomeowner'),
  display = list(names = 'ParameterEstimates')
)
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

    ## $ParameterEstimates
    ##          Effect DemGender DemHomeowner        Parameter         ParmName
    ## 1     Intercept                               Intercept        Intercept
    ## 2        DemAge                                  DemAge           DemAge
    ## 3 purchase_3mon                           purchase_3mon    purchase_3mon
    ## 4 purchase_6mon                           purchase_6mon    purchase_6mon
    ## 5     DemGender         F                   DemGender F      DemGender_F
    ## 6     DemGender         M                   DemGender M      DemGender_M
    ## 7     DemGender         U                   DemGender U      DemGender_U
    ## 8  DemHomeowner                    No   DemHomeowner No  DemHomeowner_No
    ## 9  DemHomeowner                    Yes DemHomeowner Yes DemHomeowner_Yes
    ##   DF      Estimate       StdErr        ChiSq    ProbChiSq
    ## 1  1  3.533485e-01 5.958582e-02 3.516582e+01 3.027918e-09
    ## 2  1 -5.647790e-02 1.624806e-04 1.208242e+05 0.000000e+00
    ## 3  1  8.864158e-06 5.694667e-05 2.422916e-02 8.763032e-01
    ## 4  1  3.650636e-05 4.033218e-05 8.192828e-01 3.653900e-01
    ## 5  1  1.817158e+00 7.381156e-03 6.060891e+04 0.000000e+00
    ## 6  1  8.579051e-01 8.215534e-03 1.090453e+04 0.000000e+00
    ## 7  0  0.000000e+00          NaN          NaN          NaN
    ## 8  1  3.197861e-04 4.226444e-03 5.724915e-03 9.396871e-01
    ## 9  0  0.000000e+00          NaN          NaN          NaN

``` r
cas.regression.logistic(
  organics,
  model = list(
    depvars = list(list(name = 'TargetBuy')),
    effects = list(list(
      vars = list('DemAge', 'Purchase_3mon', 'Purchase_6mon', 
                  'DemGender','DemHomeowner'))),
    link = 'PROBIT'
    ),
  class = list(list(vars = list('DemGender', 'DemHomeowner'))),
  display = list(names = list('ResponseProfile',
                                   'ParameterEstimates'))
)
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

    ## $ParameterEstimates
    ##          Effect DemGender DemHomeowner        Parameter         ParmName
    ## 1     Intercept                               Intercept        Intercept
    ## 2        DemAge                                  DemAge           DemAge
    ## 3 purchase_3mon                           purchase_3mon    purchase_3mon
    ## 4 purchase_6mon                           purchase_6mon    purchase_6mon
    ## 5     DemGender         F                   DemGender F      DemGender_F
    ## 6     DemGender         M                   DemGender M      DemGender_M
    ## 7     DemGender         U                   DemGender U      DemGender_U
    ## 8  DemHomeowner                    No   DemHomeowner No  DemHomeowner_No
    ## 9  DemHomeowner                    Yes DemHomeowner Yes DemHomeowner_Yes
    ##   DF      Estimate       StdErr        ChiSq    ProbChiSq
    ## 1  1  1.715821e-01 3.445468e-02 2.479977e+01 6.360516e-07
    ## 2  1 -3.153958e-02 8.963853e-05 1.238005e+05 0.000000e+00
    ## 3  1  6.638325e-06 3.299864e-05 4.046922e-02 8.405659e-01
    ## 4  1  2.108883e-05 2.336458e-05 8.146839e-01 3.667391e-01
    ## 5  1  1.011867e+00 3.790977e-03 7.124339e+04 0.000000e+00
    ## 6  1  4.538139e-01 4.258421e-03 1.135686e+04 0.000000e+00
    ## 7  0  0.000000e+00          NaN          NaN          NaN
    ## 8  1  5.764126e-04 2.448504e-03 5.541985e-02 8.138873e-01
    ## 9  0  0.000000e+00          NaN          NaN          NaN
    ## 
    ## $ResponseProfile
    ##   OrderedValue Outcome TargetBuy    Freq Modeled
    ## 1            1  Bought    Bought  387600       *
    ## 2            2      No        No 1186740

``` r
cas.regression.logistic(
  organics,
  model = list(
    depvars = list(list(name = 'TargetBuy')),
    effects = list(list(
      vars = list('DemAge', 'Purchase_3mon', 'Purchase_6mon', 
                  'DemGender','DemHomeowner'))),
    link = 'PROBIT'),
  class = list(list(vars = list('DemGender', 'DemHomeowner'))),
  output = list(casout  = list(name = 'predicted',
                               replace = TRUE),
                copyVars= 'ALL')
)
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

    ## $ClassInfo
    ##          Class Levels Values
    ## 1    DemGender      3  F M U
    ## 2 DemHomeowner      2 No Yes
    ## 
    ## $ConvergenceStatus
    ##                                          Reason Status  MaxGradient
    ## 1 Convergence criterion (GCONV=1E-8) satisfied.      0 2.971802e-08
    ## 
    ## $Dimensions
    ##         RowId                Description Value
    ## 1 NDESIGNCOLS          Columns in Design     9
    ## 2    NEFFECTS          Number of Effects     6
    ## 3   MAXEFCOLS         Max Effect Columns     3
    ## 4  DESIGNRANK             Rank of Design     7
    ## 5     OPTPARM Parameters in Optimization     7
    ## 
    ## $FitStatistics
    ##   RowId              Description   Value
    ## 1  M2LL        -2 Log Likelihood 1510421
    ## 2   AIC  AIC (smaller is better) 1510435
    ## 3  AICC AICC (smaller is better) 1510435
    ## 4   SBC  SBC (smaller is better) 1510521
    ## 
    ## $GlobalTest
    ##               Test DF    ChiSq ProbChiSq
    ## 1 Likelihood Ratio  6 246920.5         0
    ## 
    ## $ModelInfo
    ##         RowId            Description                       Value
    ## 1        DATA            Data Source                    ORGANICS
    ## 2 RESPONSEVAR      Response Variable                   TargetBuy
    ## 3        DIST           Distribution                      Binary
    ## 4        LINK          Link Function                      Probit
    ## 5        TECH Optimization Technique Newton-Raphson with Ridging
    ## 
    ## $NObs
    ##   RowId                 Description   Value
    ## 1 NREAD Number of Observations Read 1688948
    ## 2 NUSED Number of Observations Used 1574340
    ## 
    ## $OutputCasTables
    ##   casLib      Name Label    Rows Columns
    ## 1    HPS predicted       1688948      37
    ## 
    ## $ParameterEstimates
    ##          Effect DemGender DemHomeowner        Parameter         ParmName
    ## 1     Intercept                               Intercept        Intercept
    ## 2        DemAge                                  DemAge           DemAge
    ## 3 purchase_3mon                           purchase_3mon    purchase_3mon
    ## 4 purchase_6mon                           purchase_6mon    purchase_6mon
    ## 5     DemGender         F                   DemGender F      DemGender_F
    ## 6     DemGender         M                   DemGender M      DemGender_M
    ## 7     DemGender         U                   DemGender U      DemGender_U
    ## 8  DemHomeowner                    No   DemHomeowner No  DemHomeowner_No
    ## 9  DemHomeowner                    Yes DemHomeowner Yes DemHomeowner_Yes
    ##   DF      Estimate       StdErr        ChiSq    ProbChiSq
    ## 1  1  1.715821e-01 3.445468e-02 2.479977e+01 6.360516e-07
    ## 2  1 -3.153958e-02 8.963853e-05 1.238005e+05 0.000000e+00
    ## 3  1  6.638325e-06 3.299864e-05 4.046922e-02 8.405659e-01
    ## 4  1  2.108883e-05 2.336458e-05 8.146839e-01 3.667391e-01
    ## 5  1  1.011867e+00 3.790977e-03 7.124339e+04 0.000000e+00
    ## 6  1  4.538139e-01 4.258421e-03 1.135686e+04 0.000000e+00
    ## 7  0  0.000000e+00          NaN          NaN          NaN
    ## 8  1  5.764126e-04 2.448504e-03 5.541985e-02 8.138873e-01
    ## 9  0  0.000000e+00          NaN          NaN          NaN
    ## 
    ## $ResponseProfile
    ##   OrderedValue Outcome TargetBuy    Freq Modeled
    ## 1            1  Bought    Bought  387600       *
    ## 2            2      No        No 1186740        
    ## 
    ## $Timing
    ##            RowId                 Task         Time      RelTime
    ## 1          SETUP    Setup and Parsing 0.0449559689 0.0246906516
    ## 2   LEVELIZATION         Levelization 0.4678020477 0.2569255576
    ## 3 INITIALIZATION Model Initialization 0.0075638294 0.0041541954
    ## 4           SSCP     SSCP Computation 0.2437181473 0.1338545250
    ## 5        FITTING        Model Fitting 0.4273109436 0.2346870925
    ## 6         OUTPUT Creating Output Data 0.5744159222 0.3154798741
    ## 7        CLEANUP              Cleanup 0.0005559921 0.0003053612
    ## 8          TOTAL                Total 1.8207688332 1.0000000000

``` r
result1 = defCasTable(conn, 'predicted')
```

``` r
names(result1)
```

    ##  [1] "_PRED_"            "ID"                "DemAffl"          
    ##  [4] "DemAge"            "DemGender"         "DemHomeowner"     
    ##  [7] "DemAgeGroup"       "DemCluster"        "DemReg"           
    ## [10] "DemTVReg"          "DemFlag1"          "DemFlag2"         
    ## [13] "DemFlag3"          "DemFlag4"          "DemFlag5"         
    ## [16] "DemFlag6"          "DemFlag7"          "DemFlag8"         
    ## [19] "PromClass"         "PromTime"          "TargetBuy"        
    ## [22] "Bought_Beverages"  "Bought_Bakery"     "Bought_Canned"    
    ## [25] "Bought_Dairy"      "Bought_Baking"     "Bought_Frozen"    
    ## [28] "Bought_Meat"       "Bought_Fruits"     "Bought_Vegetables"
    ## [31] "Bought_Cleaners"   "Bought_PaperGoods" "Bought_Others"    
    ## [34] "purchase_3mon"     "purchase_6mon"     "purchase_9mon"    
    ## [37] "purchase_12mon"

``` r
cas.simple.crossTab(result1, row='DemGender', weight='_PRED_', aggregators='mean')
```

    ## $Crosstab
    ##   DemGender       Col1
    ## 1         F 0.34513810
    ## 2         M 0.16670282
    ## 3         U 0.07819483

``` r
# example 4 score code
result <- cas.regression.logistic(
  organics,
  target = 'TargetBuy',
  inputs = c('DemAge', 'Purchase_3mon', 'Purchase_6mon'),
  code = list(tabForm = FALSE)
)
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

``` r
result$`_code_`
```

    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              SASCode
    ## 1    /*---------------------------------------------------------\n     Generated SAS Scoring Code\n     Date: 27Jul2018:01:16:21\n     -------------------------------------------------------*/\n\n   drop _badval_ _linp_ _temp_ _i_ _j_;\n   _badval_ = 0;\n   _linp_   = 0;\n   _temp_   = 0;\n   _i_      = 0;\n   _j_      = 0;\n   drop MACLOGBIG;\n   MACLOGBIG= 7.0978271289338392e+02;\n\n   array _xrow_0_0_{4} _temporary_;\n   array _beta_0_0_{4} _temporary_ (    1.75527422456684\n          -0.05743797902413\n        -1.6172314911887E-6\n           0.00003872414134);\n\n   if missing(purchase_3mon) \n      or missing(DemAge) \n      or missing(purchase_6mon) \n      then do;\n         _badval_ = 1;\n         goto skip_0_0;\n   end;\n\n   do _i_=1 to 4; _xrow_0_0_{_i_} = 0; end;\n\n   _xrow_0_0_[1] = 1;\n\n   _xrow_0_0_[2] = DemAge;\n\n   _xrow_0_0_[3] = purchase_3mon;\n\n   _xrow_0_0_[4] = purchase_6mon;\n\n   do _i_=1 to 4;\n      _linp_ + _xrow_0_0_{_i_} * _beta_0_0_{_i_};\n   end;\n\n   skip_0_0:\n   length I_TargetBuy $6;\n   label I_TargetBuy = 'Into: TargetBuy';\n   array _levels_0_{2} $ 6 _TEMPORARY_ ('Bought'\n   ,'No'\n   );\n   label P_TargetBuy = 'Predicted: TargetBuy';\n   if (_badval_ eq 0) and not missing(_linp_) then do;\n      if (_linp_ > 0) then do;\n         P_TargetBuy = 1 / (1+exp(-_linp_));\n      end; else do;\n         P_TargetBuy = exp(_linp_) / (1+exp(_linp_));\n      end;\n      if P_TargetBuy >= 0.5                  then do;\n         I_TargetBuy = _levels_0_{1};\n      end; else do;\n         I_TargetBuy = _levels_0_{2};\n      end;\n   end; else do;\n      _linp_ = .;\n      P_TargetBuy = .;\n      I_TargetBuy = ' ';\n   end;\n\n

``` r
organics@groupby <- 'DemGender'
result <- cas.regression.logistic(
  organics,
  target = 'TargetBuy',
  inputs = c('DemAge', 'Purchase_3mon', 'Purchase_6mon')
)
```

    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.
    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.
    ## NOTE: Convergence criterion (GCONV=1E-8) satisfied.

``` r
result2 <- rbind.bygroups(result)
result2$ParameterEstimates
```

    ##    DemGender        Effect     Parameter      ParmName DF      Estimate
    ## 1          F     Intercept     Intercept     Intercept  1  2.181055e+00
    ## 2          F        DemAge        DemAge        DemAge  1 -5.701612e-02
    ## 3          F purchase_3mon purchase_3mon purchase_3mon  1  2.329004e-05
    ## 4          F purchase_6mon purchase_6mon purchase_6mon  1  3.788999e-05
    ## 5          M     Intercept     Intercept     Intercept  1  1.230455e+00
    ## 6          M        DemAge        DemAge        DemAge  1 -5.611447e-02
    ## 7          M purchase_3mon purchase_3mon purchase_3mon  1 -9.149473e-05
    ## 8          M purchase_6mon purchase_6mon purchase_6mon  1  6.508839e-05
    ## 9          U     Intercept     Intercept     Intercept  1  2.113357e-01
    ## 10         U        DemAge        DemAge        DemAge  1 -5.262068e-02
    ## 11         U purchase_3mon purchase_3mon purchase_3mon  1  1.391191e-04
    ## 12         U purchase_6mon purchase_6mon purchase_6mon  1 -4.563116e-05
    ##          StdErr        ChiSq     ProbChiSq
    ## 1  7.077208e-02 9.497499e+02 1.503554e-208
    ## 2  1.922815e-04 8.792658e+04  0.000000e+00
    ## 3  6.805278e-05 1.171248e-01  7.321740e-01
    ## 4  4.824883e-05 6.167020e-01  4.322755e-01
    ## 5  1.272447e-01 9.350871e+01  4.044172e-22
    ## 6  3.604855e-04 2.423116e+04  0.000000e+00
    ## 7  1.227469e-04 5.556110e-01  4.560341e-01
    ## 8  8.668951e-05 5.637337e-01  4.527598e-01
    ## 9  2.043550e-01 1.069486e+00  3.010615e-01
    ## 10 5.690589e-04 8.550638e+03  0.000000e+00
    ## 11 1.968059e-04 4.996857e-01  4.796382e-01
    ## 12 1.391582e-04 1.075240e-01  7.429809e-01

``` r
organics@groupby <- list()
```

Decision Trees
==============

``` r
cas.builtins.loadActionSet(conn, 'decisiontree')
```

    ## NOTE: Added action set 'decisiontree'.

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

    ## $actionset
    ## [1] "decisiontree"

``` r
cas.decisionTree.dtreeTrain(
  organics,
  target = 'TargetBuy',
  inputs = c('DemGender'),
  casout = list(name = 'treeModel1', replace = TRUE)
)
```

    ## $ModelInfo
    ##                           Descr        Value
    ## 1          Number of Tree Nodes 5.000000e+00
    ## 2        Max Number of Branches 2.000000e+00
    ## 3              Number of Levels 3.000000e+00
    ## 4              Number of Leaves 3.000000e+00
    ## 5                Number of Bins 2.000000e+01
    ## 6        Minimum Size of Leaves 3.236840e+05
    ## 7        Maximum Size of Leaves 9.233240e+05
    ## 8           Number of Variables 1.000000e+00
    ## 9  Confidence Level for Pruning 2.500000e-01
    ## 10  Number of Observations Used 1.688948e+06
    ## 11  Misclassification Error (%) 2.477163e+01
    ## 
    ## $OutputCasTables
    ##   casLib       Name Rows Columns
    ## 1    HPS treeModel1    5      24

``` r
output1 <- defCasTable(conn, 'treeModel1')
```

``` r
names(output1)
```

    ##  [1] "_Target_"         "_NumTargetLevel_" "_TargetValL_"    
    ##  [4] "_TargetVal0_"     "_TargetVal1_"     "_CI0_"           
    ##  [7] "_CI1_"            "_NodeID_"         "_TreeLevel_"     
    ## [10] "_NodeName_"       "_Parent_"         "_ParentName_"    
    ## [13] "_NodeType_"       "_Gain_"           "_NumObs_"        
    ## [16] "_TargetValue_"    "_NumChild_"       "_ChildID0_"      
    ## [19] "_ChildID1_"       "_PBranches_"      "_PBNameL0_"      
    ## [22] "_PBNameL1_"       "_PBName0_"        "_PBName1_"

``` r
output1[,c('_TreeLevel_', '_NodeID_', '_Parent_', '_ParentName_', 
        '_NodeType_', '_PBName0_', 
        '_PBName1_')]
```

    ##   _TreeLevel_ _NodeID_ _Parent_ _ParentName_ _NodeType_ _PBName0_
    ## 1           0        0       -1                       1          
    ## 2           2        4        1    DemGender          3         M
    ## 3           1        1        0    DemGender          1         M
    ## 4           1        2        0    DemGender          3         F
    ## 5           2        3        1    DemGender          3         U
    ##   _PBName1_
    ## 1          
    ## 2          
    ## 3         U
    ## 4          
    ## 5

``` r
output1[,c('_TreeLevel_', '_NodeID_', '_Parent_', 
           '_TargetVal0_', '_TargetVal1_', '_CI0_', '_CI1_', 
           '_Gain_', '_NumObs_')]
```

    ##   _TreeLevel_ _NodeID_ _Parent_ _TargetVal0_ _TargetVal1_      _CI0_
    ## 1           0        0       -1       Bought           No 0.24771633
    ## 2           2        4        1       Bought           No 0.16612210
    ## 3           1        1        0       Bought           No 0.12904507
    ## 4           1        2        0       Bought           No 0.34611902
    ## 5           2        3        1       Bought           No 0.07842216
    ##       _CI1_     _Gain_ _NumObs_
    ## 1 0.7522837 0.04771340  1688948
    ## 2 0.8338779 0.00000000   441940
    ## 3 0.8709549 0.01288632   765624
    ## 4 0.6538810 0.00000000   923324
    ## 5 0.9215778 0.00000000   323684

``` r
cas.decisionTree.dtreeTrain(
  organics,
  target = 'TargetBuy',
  inputs = c('DemGender'),
  casout = list(name = 'treeModel1', replace = TRUE),
  prune = TRUE
)
```

    ## $ModelInfo
    ##                           Descr        Value
    ## 1          Number of Tree Nodes 3.000000e+00
    ## 2        Max Number of Branches 2.000000e+00
    ## 3              Number of Levels 2.000000e+00
    ## 4              Number of Leaves 2.000000e+00
    ## 5                Number of Bins 2.000000e+01
    ## 6        Minimum Size of Leaves 7.656240e+05
    ## 7        Maximum Size of Leaves 9.233240e+05
    ## 8           Number of Variables 1.000000e+00
    ## 9  Confidence Level for Pruning 2.500000e-01
    ## 10  Number of Observations Used 1.688948e+06
    ## 11  Misclassification Error (%) 2.477163e+01
    ## 
    ## $OutputCasTables
    ##   casLib       Name Rows Columns
    ## 1    HPS treeModel1    3      24

``` r
output1 <- defCasTable(conn, 'treeModel1')
output1[,c('_TreeLevel_', '_NodeID_', '_Parent_', '_ParentName_', 
           '_NodeType_', '_PBName0_',
           '_PBName1_')]
```

    ##   _TreeLevel_ _NodeID_ _Parent_ _ParentName_ _NodeType_ _PBName0_
    ## 1           0        0       -1                       1          
    ## 2           1        1        0    DemGender          3         M
    ## 3           1        2        0    DemGender          3         F
    ##   _PBName1_
    ## 1          
    ## 2         U
    ## 3

``` r
cas.decisiontree.dtreePrune(
  conn, 
  table = 'your_validation_data',
  modelTable = 'treeModel1',
  casout = list(name = 'pruned_tree')
)
```

``` r
varlist <- c('DemGender', 'DemHomeowner', 'DemAgeGroup', 'DemCluster', 
             'DemReg', 'DemTVReg', 'DemFlag1', 'DemFlag2', 'DemFlag3', 
             'DemFlag4', 'DemFlag5', 'DemFlag6', 'DemFlag7', 'DemFlag8', 
             'PromClass')
cas.decisionTree.dtreeTrain(
  organics,
  target = 'TargetBuy',
  inputs = varlist,
  casout = list(name = 'treeModel2', replace = TRUE)
)
```

    ## $ModelInfo
    ##                           Descr        Value
    ## 1          Number of Tree Nodes 4.500000e+01
    ## 2        Max Number of Branches 2.000000e+00
    ## 3              Number of Levels 6.000000e+00
    ## 4              Number of Leaves 2.300000e+01
    ## 5                Number of Bins 2.000000e+01
    ## 6        Minimum Size of Leaves 7.600000e+01
    ## 7        Maximum Size of Leaves 5.972840e+05
    ## 8           Number of Variables 1.500000e+01
    ## 9  Confidence Level for Pruning 2.500000e-01
    ## 10  Number of Observations Used 1.688948e+06
    ## 11  Misclassification Error (%) 2.388966e+01
    ## 
    ## $OutputCasTables
    ##   casLib       Name Rows Columns
    ## 1    HPS treeModel2   45     130

``` r
cas.decisionTree.dtreeTrain(
  organics,
  target = 'TargetBuy',
  inputs = varlist,
  leafSize = 1000,
  maxLevel = 4,
  casout = list(name = 'treeModel2', replace = TRUE)
)
```

    ## $ModelInfo
    ##                           Descr        Value
    ## 1          Number of Tree Nodes 1.500000e+01
    ## 2        Max Number of Branches 2.000000e+00
    ## 3              Number of Levels 4.000000e+00
    ## 4              Number of Leaves 8.000000e+00
    ## 5                Number of Bins 2.000000e+01
    ## 6        Minimum Size of Leaves 1.216000e+03
    ## 7        Maximum Size of Leaves 8.891240e+05
    ## 8           Number of Variables 1.500000e+01
    ## 9  Confidence Level for Pruning 2.500000e-01
    ## 10  Number of Observations Used 1.688948e+06
    ## 11  Misclassification Error (%) 2.393466e+01
    ## 
    ## $OutputCasTables
    ##   casLib       Name Rows Columns
    ## 1    HPS treeModel2   15     130

``` r
cas.decisionTree.dtreeScore(
  organics,
  modelTable = 'treeModel2'
)
```

    ## $ScoreInfo
    ##                         Descr                            Value
    ## 1 Number of Observations Read                          1688948
    ## 2 Number of Observations Used                          1688948
    ## 3 Misclassification Error (%)                     23.934662287

``` r
result <- cas.decisionTree.dtreeScore(
  organics,
  modelTable = 'treeModel2',
  casout = list(name = 'predicted', replace = TRUE)
)
output3 <- defCasTable(conn, 'predicted')
names(output3)
```

    ##  [1] "_DT_PredName_"  "_DT_PredP_"     "_DT_PredLevel_" "_LeafID_"      
    ##  [5] "_MissIt_"       "_NumNodes_"     "_NodeList0_"    "_NodeList1_"   
    ##  [9] "_NodeList2_"    "_NodeList3_"

``` r
head(output3, n = 10L)
```

    ##    _DT_PredName_ _DT_PredP_ _DT_PredLevel_ _LeafID_ _MissIt_ _NumNodes_
    ## 1             No  0.6677494              1        9        0          4
    ## 2             No  0.6677494              1        9        0          4
    ## 3             No  0.6677494              1        9        0          4
    ## 4             No  0.6677494              1        9        0          4
    ## 5             No  0.9236861              1        7        0          4
    ## 6             No  0.6677494              1        9        0          4
    ## 7             No  0.9236861              1        7        0          4
    ## 8             No  0.6677494              1        9        0          4
    ## 9             No  0.6677494              1        9        0          4
    ## 10            No  0.6677494              1        9        0          4
    ##    _NodeList0_ _NodeList1_ _NodeList2_ _NodeList3_
    ## 1            0           1           4           9
    ## 2            0           1           4           9
    ## 3            0           1           4           9
    ## 4            0           1           4           9
    ## 5            0           1           3           7
    ## 6            0           1           4           9
    ## 7            0           1           3           7
    ## 8            0           1           4           9
    ## 9            0           1           4           9
    ## 10           0           1           4           9

Gradient Boosting, Random Forests, and Neural Networks
======================================================

``` r
cas.builtins.loadActionSet(conn, 'decisiontree')
```

    ## NOTE: Added action set 'decisiontree'.

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

    ## $actionset
    ## [1] "decisiontree"

Random Forests
--------------

``` r
varlist <- c('DemGender', 'DemHomeowner', 'DemAgeGroup',
             'DemCluster', 'DemReg', 'DemTVReg', 'DemFlag1', 
             'DemFlag2', 'DemFlag3', 'DemFlag4', 'DemFlag5', 
             'DemFlag6', 'DemFlag7', 'DemFlag8', 'PromClass')
cas.decisionTree.forestTrain(
  organics,
  target = 'TargetBuy',
  inputs = varlist,
  casout = list(name = 'forest1', replace = TRUE)
)
```

    ## $ModelInfo
    ##                               Descr        Value
    ## 1                   Number of Trees     50.00000
    ## 2  Number of Selected Variables (M)      4.00000
    ## 3                Random Number Seed      0.00000
    ## 4          Bootstrap Percentage (%)     63.21206
    ## 5                    Number of Bins     20.00000
    ## 6               Number of Variables     15.00000
    ## 7      Confidence Level for Pruning      0.25000
    ## 8          Max Number of Tree Nodes     61.00000
    ## 9          Min Number of Tree Nodes     27.00000
    ## 10           Max Number of Branches      2.00000
    ## 11           Min Number of Branches      2.00000
    ## 12             Max Number of Levels      6.00000
    ## 13             Min Number of Levels      6.00000
    ## 14             Max Number of Leaves     31.00000
    ## 15             Min Number of Leaves     14.00000
    ## 16           Maximum Size of Leaves 819091.00000
    ## 17           Minimum Size of Leaves     19.00000
    ## 18               Out-of-Bag MCR (%)          NaN
    ## 
    ## $OutputCasTables
    ##   casLib    Name Rows Columns
    ## 1    HPS forest1 2280     132

``` r
result <- cas.decisionTree.forestTrain(
  organics,
  target = 'TargetBuy',
  inputs = varlist,
  varimp = TRUE,
  casout = list(name = 'forest1', replace = TRUE)
)
result['DTreeVarImpInfo']
```

    ## $DTreeVarImpInfo
    ##        Variable   Importance          Std
    ## 1     DemGender 20034.158024 8742.8837738
    ## 2   DemAgeGroup  8086.779759 2632.7622275
    ## 3     PromClass  2226.404747 1292.6640562
    ## 4    DemCluster   262.071383   71.1782843
    ## 5      DemTVReg   247.586639   73.4768832
    ## 6      DemFlag6   224.033728  312.6744573
    ## 7      DemFlag2   163.813972  268.2667745
    ## 8      DemFlag1    97.169670  135.7820460
    ## 9        DemReg    90.843351   36.9619045
    ## 10     DemFlag7    51.919647   36.4789179
    ## 11     DemFlag5    12.477782   38.3416229
    ## 12     DemFlag8    12.208576   32.1060813
    ## 13     DemFlag4     8.049368   21.2739637
    ## 14     DemFlag3     3.175237   18.2867394
    ## 15 DemHomeowner     0.403269    0.6995111

``` r
result['OutputCasTables']
```

    ## $OutputCasTables
    ##   casLib    Name Rows Columns
    ## 1    HPS forest1 2306     132

``` r
cas.decisionTree.forestScore(
  organics,
  modelTable = 'forest1',
  casout = list(name = 'scored_output', replace = TRUE)
)
```

Gradient Boosting
-----------------

``` r
varlist <- c('DemGender', 'DemHomeowner', 'DemAgeGroup', 'DemCluster', 
             'DemReg', 'DemTVReg', 'DemFlag1', 'DemFlag2', 'DemFlag3', 
             'DemFlag4', 'DemFlag5', 'DemFlag6', 'DemFlag7', 'DemFlag8', 
             'PromClass')
cas.decisionTree.gbtreeTrain(
  organics,
  target = 'TargetBuy',
  inputs = varlist,
  casout = list(name = 'gbtree1', replace = TRUE)
)
```

    ## $ModelInfo
    ##                               Descr    Value
    ## 1                   Number of Trees     50.0
    ## 2                      Distribution      2.0
    ## 3                     Learning Rate      0.1
    ## 4                  Subsampling Rate      0.5
    ## 5  Number of Selected Variables (M)     15.0
    ## 6                    Number of Bins     20.0
    ## 7               Number of Variables     15.0
    ## 8          Max Number of Tree Nodes     63.0
    ## 9          Min Number of Tree Nodes     61.0
    ## 10           Max Number of Branches      2.0
    ## 11           Min Number of Branches      2.0
    ## 12             Max Number of Levels      6.0
    ## 13             Min Number of Levels      6.0
    ## 14             Max Number of Leaves     32.0
    ## 15             Min Number of Leaves     31.0
    ## 16           Maximum Size of Leaves 328286.0
    ## 17           Minimum Size of Leaves     31.0
    ## 18               Random Number Seed      0.0
    ## 
    ## $OutputCasTables
    ##   casLib    Name Rows Columns
    ## 1    HPS gbtree1 3148     123

``` r
cas.decisionTree.gbtreeScore(
  organics,
  modelTable = 'gbtree1',
 casout = list(name = 'scored_output', replace = TRUE)
)
```

Neural Networks
---------------

``` r
cas.builtins.loadActionSet(conn, 'neuralNet')
```

    ## NOTE: Added action set 'neuralNet'.

    ## NOTE: Information for action set 'neuralNet':

    ## NOTE:    neuralNet

    ## NOTE:       annTrain - Trains an artificial neural network

    ## NOTE:       annScore - Scores a table using an artificial neural network model

    ## NOTE:       annCode - Generates DATA step scoring code from an artificial neural network model

    ## $actionset
    ## [1] "neuralNet"

``` r
result <- cas.neuralNet.annTrain(
  organics,
  target = 'TargetBuy',
  inputs = c('DemAge','DemAffl','DemGender'),
  casout = list(name = 'ann1', replace = TRUE),
  hiddens = c(4,2),
  maxIter = 500
)
names(result)
```

    ## [1] "ConvergenceStatus" "ModelInfo"         "OptIterHistory"   
    ## [4] "OutputCasTables"

``` r
result['ModelInfo']
```

    ## $ModelInfo
    ##                          Descr        Value
    ## 1                        Model   Neural Net
    ## 2  Number of Observations Used      1498264
    ## 3  Number of Observations Read      1688948
    ## 4     Target/Response Variable    TargetBuy
    ## 5              Number of Nodes           13
    ## 6        Number of Input Nodes            5
    ## 7       Number of Output Nodes            2
    ## 8       Number of Hidden Nodes            6
    ## 9      Number of Hidden Layers            2
    ## 10 Number of Weight Parameters           30
    ## 11   Number of Bias Parameters            8
    ## 12                Architecture          MLP
    ## 13       Number of Neural Nets            1
    ## 14             Objective Value 1.7053979549

``` r
cas.neuralNet.annScore(
  organics,
  modelTable = 'ann1',
  casout = list(name = 'scored_output', replace = TRUE)
)
```

    ## $OutputCasTables
    ##   casLib          Name    Rows Columns
    ## 1    HPS scored_output 1688948       2
    ## 
    ## $ScoreInfo
    ##                         Descr                            Value
    ## 1 Number of Observations Read                          1688948
    ## 2 Number of Observations Used                          1498264
    ## 3 Misclassification Error (%)                     18.215481384

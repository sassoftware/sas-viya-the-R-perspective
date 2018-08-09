Chapter 4 - Managing Your Data in CAS
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

    ## SWAT 1.2.0.9000

Create a connection.

``` r
conn <- CAS('rdcgrdc.unx.sas.com', 39935)
```

    ## NOTE: Connecting to CAS and generating CAS action functions for loaded

    ##       action sets...

    ## NOTE: To generate the functions with signatures (for tab completion), set

    ##       options(cas.gen.function.sig=TRUE).

Find out what caslibs are available using the caslibinfo action.

``` r
cas.table.caslibInfo(conn)
```

    ## $CASLibInfo
    ##                  Name Type                 Description
    ## 1          CASTestTmp PATH        castest's test files
    ## 2     CASUSER(sasyqi) PATH Personal File System Caslib
    ## 3 CASUSERHDFS(sasyqi) HDFS        Personal HDFS Caslib
    ## 4             EngTest HDFS        engtest's HDAT files
    ## 5             Formats PATH               Format Caslib
    ## 6                 HPS HDFS          HDAT files on /hps
    ##                    Path Definition Subdirs Local Active Personal Hidden
    ## 1 /bigdisk/lax/castest/                  1     0      0        0      0
    ## 2            /u/sasyqi/                  1     0      0        1      0
    ## 3         /user/sasyqi/                  1     0      1        1      0
    ## 4        /user/engtest/                  1     0      0        0      0
    ## 5 /bigdisk/lax/formats/                  1     0      0        0      0
    ## 6                 /hps/                  1     0      0        0      0
    ##   Transient
    ## 1         0
    ## 2         1
    ## 3         1
    ## 4         0
    ## 5         0
    ## 6         0

List the items at a path relative to the given CASLib using the table.fileInfo action.

``` r
cas.table.fileInfo(conn, caslib = 'casuser', path = 'data')
```

    ## $FileInfo
    ##    Permission  Owner Group                    Name     Size Encryption
    ## 1  -rwxr-xr-x sasyqi users       titanic_train.csv    61194           
    ## 2  -rwxr-xr-x sasyqi users                iris.csv     4759           
    ## 3  -rwxr-xr-x sasyqi users TrainingDataAnomaly.csv   785679           
    ## 4  -rwxr-xr-x sasyqi users            mikedata.csv  1626264           
    ## 5  -rwxr-xr-x sasyqi users       mikedata_vars.txt     1312           
    ## 6  -rwxr-xr-x sasyqi users             irisout.csv     1766           
    ## 7  drwxr-xr-x sasyqi users                     RIA     4096           
    ## 8  -rwxr-xr-x sasyqi users   reviews_small_dev.csv  4075688           
    ## 9  -rwxr-xr-x sasyqi users         irisout.sashdat    21736       NONE
    ## 10 -rwxr-xr-x sasyqi users  reviews_small_test.csv  4174829           
    ## 11 -rwxr-xr-x sasyqi users reviews_small_train.csv 41315988           
    ## 12 -rwxr-xr-x sasyqi users     FraudDemoTrain1.csv  1911634           
    ## 13 -rwxr-xr-x sasyqi users JenDProcurementData.csv     2092           
    ## 14 drwxr-xr-x sasyqi users                  jukhar     4096           
    ##                         Time    ModTime
    ## 1  2015-10-02T10:28:22-04:00 1759415302
    ## 2  2017-01-17T14:31:05-04:00 1800297065
    ## 3  2017-11-08T11:03:34-04:00 1825772614
    ## 4  2017-11-14T12:35:29-04:00 1826296529
    ## 5  2017-11-17T14:49:36-04:00 1826563776
    ## 6  2018-07-25T21:21:36-04:00 1848187296
    ## 7  2017-11-29T18:03:42-04:00 1827612222
    ## 8  2017-11-07T17:21:29-04:00 1825708889
    ## 9  2018-07-25T21:21:36-04:00 1848187296
    ## 10 2017-11-07T17:21:42-04:00 1825708902
    ## 11 2017-11-07T17:21:03-04:00 1825708863
    ## 12 2018-03-22T14:59:10-04:00 1837364350
    ## 13 2018-05-22T12:06:59-04:00 1842624419
    ## 14 2018-06-05T14:26:15-04:00 1843842375

Use the fileinfo action with the active CASLib (i.e., casuser).

``` r
cas.sessionProp.setSessOpt(conn, caslib = "casuser")
```

    ## NOTE: 'CASUSER(sasyqi)' is now the active caslib.

    ## list()

``` r
cas.table.fileInfo(conn, path = 'data')
```

    ## $FileInfo
    ##    Permission  Owner Group                    Name     Size Encryption
    ## 1  -rwxr-xr-x sasyqi users       titanic_train.csv    61194           
    ## 2  -rwxr-xr-x sasyqi users                iris.csv     4759           
    ## 3  -rwxr-xr-x sasyqi users TrainingDataAnomaly.csv   785679           
    ## 4  -rwxr-xr-x sasyqi users            mikedata.csv  1626264           
    ## 5  -rwxr-xr-x sasyqi users       mikedata_vars.txt     1312           
    ## 6  -rwxr-xr-x sasyqi users             irisout.csv     1766           
    ## 7  drwxr-xr-x sasyqi users                     RIA     4096           
    ## 8  -rwxr-xr-x sasyqi users   reviews_small_dev.csv  4075688           
    ## 9  -rwxr-xr-x sasyqi users         irisout.sashdat    21736       NONE
    ## 10 -rwxr-xr-x sasyqi users  reviews_small_test.csv  4174829           
    ## 11 -rwxr-xr-x sasyqi users reviews_small_train.csv 41315988           
    ## 12 -rwxr-xr-x sasyqi users     FraudDemoTrain1.csv  1911634           
    ## 13 -rwxr-xr-x sasyqi users JenDProcurementData.csv     2092           
    ## 14 drwxr-xr-x sasyqi users                  jukhar     4096           
    ##                         Time    ModTime
    ## 1  2015-10-02T10:28:22-04:00 1759415302
    ## 2  2017-01-17T14:31:05-04:00 1800297065
    ## 3  2017-11-08T11:03:34-04:00 1825772614
    ## 4  2017-11-14T12:35:29-04:00 1826296529
    ## 5  2017-11-17T14:49:36-04:00 1826563776
    ## 6  2018-07-25T21:21:36-04:00 1848187296
    ## 7  2017-11-29T18:03:42-04:00 1827612222
    ## 8  2017-11-07T17:21:29-04:00 1825708889
    ## 9  2018-07-25T21:21:36-04:00 1848187296
    ## 10 2017-11-07T17:21:42-04:00 1825708902
    ## 11 2017-11-07T17:21:03-04:00 1825708863
    ## 12 2018-03-22T14:59:10-04:00 1837364350
    ## 13 2018-05-22T12:06:59-04:00 1842624419
    ## 14 2018-06-05T14:26:15-04:00 1843842375

In addition to the files that are accessible to this caslib, there can also be tables that have already been loaded that are available to the caslib. These can be seen using the table.tableInfo action.

``` r
out <- cas.table.tableInfo(conn)
```

    ## NOTE: No tables are available in caslib CASUSER(sasyqi) of Cloud Analytic Services.

Loading Data into a CAS Table
-----------------------------

Load data from the server-side using the table.loadTable action.

``` r
out <- cas.table.loadTable(
  conn, 
  path = 'data/iris.csv', 
  caslib='casuser'
)
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

Specify an output table name explicitly.

``` r
out <- cas.table.loadTable(
  conn, 
  path = 'data/iris.csv', 
  caslib='casuser', 
  casout=list(name='mydata', caslib='casuser')
  )
```

    ## NOTE: Cloud Analytic Services made the file data/iris.csv available as table MYDATA in caslib CASUSER(sasyqi).

``` r
out
```

    ## $caslib
    ## [1] "CASUSER(sasyqi)"
    ## 
    ## $tableName
    ## [1] "MYDATA"

Get information about the table using the table.tableInfo action.

``` r
cas.table.tableInfo(conn, name = 'data.iris', caslib = 'casuser')
```

    ## $TableInfo
    ##        Name Rows Columns IndexedColumns Encoding       CreateTimeFormatted
    ## 1 DATA.IRIS  150       5              0    utf-8 2018-07-25T21:23:44-04:00
    ##            ModTimeFormatted       AccessTimeFormatted JavaCharSet
    ## 1 2018-07-25T21:23:44-04:00 2018-07-25T21:23:44-04:00        UTF8
    ##   CreateTime    ModTime AccessTime Global Repeated View    SourceName
    ## 1 1848187424 1848187424 1848187424      0        0    0 data/iris.csv
    ##      SourceCaslib Compressed Creator Modifier    SourceModTimeFormatted
    ## 1 CASUSER(sasyqi)          0  sasyqi          2017-01-17T14:31:05-04:00
    ##   SourceModTime
    ## 1    1800297065

Get information about the table columns using columninfo.

``` r
cas.table.columnInfo(
  conn, 
  table = list(name = 'data.iris', caslib = 'casuser')
)
```

    ## $ColumnInfo
    ##         Column ID    Type RawLength FormattedLength NFL NFD
    ## 1 Sepal.Length  1  double         8              12   0   0
    ## 2  Sepal.Width  2  double         8              12   0   0
    ## 3 Petal.Length  3  double         8              12   0   0
    ## 4  Petal.Width  4  double         8              12   0   0
    ## 5      Species  5 varchar        15              15   0   0

### Displaying Data in a CAS Table

Use the table.fetch action to download rows of data.

``` r
cas.table.fetch(
  conn, 
  table = list(name = 'data.iris', caslib = 'casuser'), 
  to = 5
)
```

    ## $Fetch
    ##   _Index_ Sepal.Length Sepal.Width Petal.Length Petal.Width     Species
    ## 1       1          5.1         3.5          1.4         0.2 Iris-setosa
    ## 2       2          4.9         3.0          1.4         0.2 Iris-setosa
    ## 3       3          4.7         3.2          1.3         0.2 Iris-setosa
    ## 4       4          4.6         3.1          1.5         0.2 Iris-setosa
    ## 5       5          5.0         3.6          1.4         0.2 Iris-setosa

Specify sorting options to get a predictable set of data.

``` r
cas.table.fetch(
  conn, 
  table = list(name = 'data.iris', caslib = 'casuser'), 
  to = 5,
  sortby=list('Sepal.Length ', 'Sepal.Width')
)
```

    ## $Fetch
    ##   _Index_ Sepal.Length Sepal.Width Petal.Length Petal.Width     Species
    ## 1       1          4.3         3.0          1.1         0.1 Iris-setosa
    ## 2       2          4.4         2.9          1.4         0.2 Iris-setosa
    ## 3       3          4.4         3.0          1.3         0.2 Iris-setosa
    ## 4       4          4.4         3.2          1.3         0.2 Iris-setosa
    ## 5       5          4.5         2.3          1.3         0.3 Iris-setosa

### Computing Simple Statistics

Run the simple.summary action on the table.

``` r
cas.simple.summary(
  conn, 
  table = list(name='data.iris', caslib='casuser')
)
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

### Dropping a CAS Table

``` r
cas.table.dropTable(
  conn, 
  name = 'data.iris', 
  caslib = 'casuser'
)
```

    ## NOTE: Cloud Analytic Services dropped table data.iris from caslib CASUSER(sasyqi).

    ## list()

### The Active Caslib

Use the caslibinfo action to display information about CASLibs. The Active column indicates whether the CASLib is the active CASLib.

``` r
cas.table.caslibInfo(conn)
```

    ## $CASLibInfo
    ##                  Name Type                 Description
    ## 1          CASTestTmp PATH        castest's test files
    ## 2     CASUSER(sasyqi) PATH Personal File System Caslib
    ## 3 CASUSERHDFS(sasyqi) HDFS        Personal HDFS Caslib
    ## 4             EngTest HDFS        engtest's HDAT files
    ## 5             Formats PATH               Format Caslib
    ## 6                 HPS HDFS          HDAT files on /hps
    ##                    Path Definition Subdirs Local Active Personal Hidden
    ## 1 /bigdisk/lax/castest/                  1     0      0        0      0
    ## 2            /u/sasyqi/                  1     0      1        1      0
    ## 3         /user/sasyqi/                  1     0      0        1      0
    ## 4        /user/engtest/                  1     0      0        0      0
    ## 5 /bigdisk/lax/formats/                  1     0      0        0      0
    ## 6                 /hps/                  1     0      0        0      0
    ##   Transient
    ## 1         0
    ## 2         1
    ## 3         1
    ## 4         0
    ## 5         0
    ## 6         0

You can get the active CASLib setting using the getsessopt action.

``` r
cas.sessionProp.getSessOpt(conn, name = 'caslib')
```

    ## $caslib
    ## [1] "CASUSER(sasyqi)"

You can set the active CASLib using the setsessopt action.

``` r
cas.sessionProp.setSessOpt(conn, caslib = 'formats')
```

    ## NOTE: 'Formats' is now the active caslib.

    ## list()

``` r
cas.sessionProp.setSessOpt(conn, caslib = 'casuser')
```

    ## NOTE: 'CASUSER(sasyqi)' is now the active caslib.

    ## list()

Uploading Data Files to CAS Tables
----------------------------------

Use the cas.read.csv function on CAS connection objects to upload CSV data from client-side files. This uploads the file to the server as-is. It is then parsed on the server.

``` r
iris1 <- cas.read.csv(conn,'/u/sasyqi/data/iris.csv')
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table IRIS in caslib CASUSER(sasyqi).

``` r
cas.table.columnInfo(iris1)
```

    ## $ColumnInfo
    ##         Column ID    Type RawLength FormattedLength NFL NFD
    ## 1 Sepal.Length  1  double         8              12   0   0
    ## 2  Sepal.Width  2  double         8              12   0   0
    ## 3 Petal.Length  3  double         8              12   0   0
    ## 4  Petal.Width  4  double         8              12   0   0
    ## 5      Species  5 varchar        15              15   0   0

Specify an explicit table name on cas.read.csv

``` r
iris2 <- cas.read.csv(
  conn, 
  '/u/sasyqi/data/iris.csv',
  casOut = list(name = 'iris2', caslib='casuser')
)
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table IRIS2 in caslib CASUSER(sasyqi).

Use the sep SEP parameter to parse a tab-delimited file.

``` r
iris_tsv <- cas.read.csv(
  conn, 
  '/u/sasyqi/data/iris.tsv',
  sep = '\t', 
  casOut = list(name = "iris_tsv", caslib='casuser')
)
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table IRIS_TSV in caslib CASUSER(sasyqi).

``` r
head(iris_tsv)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2       0
    ## 2          4.9         3.0          1.4         0.2       0
    ## 3          4.7         3.2          1.3         0.2       0
    ## 4          4.6         3.1          1.5         0.2       0
    ## 5          5.0         3.6          1.4         0.2       0
    ## 6          5.4         3.9          1.7         0.4       0

Uploading Data from URLs to CAS Tables
--------------------------------------

Rather than specifying a filename, you can specify a URL.

``` r
out <- cas.read.csv(conn, 'https://raw.githubusercontent.com/sassoftware/sas-viya-programming/master/data/class.csv')
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table CLASS in caslib CASUSER(sasyqi).

``` r
head(out)
```

    ##      Name Sex Age Height Weight
    ## 1  Alfred   M  14   69.0  112.5
    ## 2   Alice   F  13   56.5   84.0
    ## 3 Barbara   F  13   65.3   98.0
    ## 4   Carol   F  14   62.8  102.5
    ## 5   Henry   M  14   63.5  102.5
    ## 6   James   M  12   57.3   83.0

Uploading Data from a data.frame to a CAS Table
-----------------------------------------------

In addition to files, you can upload data.frame.

``` r
head(iris)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa

``` r
class(iris)
```

    ## [1] "data.frame"

Upload iris data.frame to a CASTable with the name "iris".

``` r
iris_ct <- as.casTable(conn, iris, casOut = list(replace = TRUE))
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table IRIS in caslib CASUSER(sasyqi).

``` r
cas.table.fetch(conn,table = 'iris', to = 5)
```

    ## $Fetch
    ##   _Index_ Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1       1          5.1         3.5          1.4         0.2  setosa
    ## 2       2          4.9         3.0          1.4         0.2  setosa
    ## 3       3          4.7         3.2          1.3         0.2  setosa
    ## 4       4          4.6         3.1          1.5         0.2  setosa
    ## 5       5          5.0         3.6          1.4         0.2  setosa

Give the table a different name using the casOut parameter.

``` r
iris_tbl <- as.casTable(conn, iris, casOut = list(name = 'iris_cas',replace = TRUE))
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table IRIS_CAS in caslib CASUSER(sasyqi).

Exporting CAS Tables to Other Formats
-------------------------------------

Store the data from a CAS table to CSV form

``` r
cas.table.save(iris_tbl, name='data/irisout.csv', caslib='casuser', replace = TRUE)
```

    ## NOTE: Cloud Analytic Services saved the file data/irisout.csv in caslib CASUSER(sasyqi).

    ## $caslib
    ## [1] "CASUSER(sasyqi)"
    ## 
    ## $name
    ## [1] "data/irisout.csv"

Store the data from a CAS table to SASHDAT form

``` r
cas.table.save(iris_tbl, name='data/irisout.sashdat', caslib='casuser', replace = TRUE)
```

    ## NOTE: Cloud Analytic Services saved the file data/irisout.sashdat in caslib CASUSER(sasyqi).

    ## $caslib
    ## [1] "CASUSER(sasyqi)"
    ## 
    ## $name
    ## [1] "data/irisout.sashdat"

Managing Caslibs
----------------

### Creating a Caslib

``` r
cas.table.addCaslib(
  conn,
  path='/research/data ', 
  caslib='research', 
  description='Research Data',  
  subDirs=FALSE,
  session=FALSE,
  activeOnAdd=FALSE
  )
```

    ## NOTE: Cloud Analytic Services added the caslib 'research'.

    ## $CASLibInfo
    ##       Name Type   Description             Path Definition Subdirs Local
    ## 1 research HDFS Research Data /research/data /                  0     0
    ##   Active Personal Hidden Transient
    ## 1      0        0      0         0

### Setting an Active Caslib

``` r
cas.sessionProp.setSessOpt(conn, caslib = 'research')
```

    ## NOTE: 'research' is now the active caslib.

    ## list()

``` r
cas.table.caslibInfo(conn, caslib='research')
```

    ## $CASLibInfo
    ##       Name Type   Description             Path Definition Subdirs Local
    ## 1 research HDFS Research Data /research/data /                  0     0
    ##   Active Personal Hidden Transient
    ## 1      1        0      0         0

### Dropping a Caslib

``` r
cas.table.dropCaslib(conn, caslib = 'research')
```

    ## NOTE: 'CASUSERHDFS(sasyqi)' is now the active caslib.

    ## NOTE: Cloud Analytic Services removed the caslib 'research'.

    ## list()

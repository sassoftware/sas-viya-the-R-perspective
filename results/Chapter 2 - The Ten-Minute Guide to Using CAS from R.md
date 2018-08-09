Chapter 2 - The Ten-Minute Guide to Using CAS from R
================
Yue Qi
May 31, 2018

``` r
knitr::opts_chunk$set()
```

Loading SWAT and Getting Connected
----------------------------------

Import the SWAT package.

``` r
library("swat")
```

    ## NOTE: The extension module for binary protocol support is not available.

    ##       Only the CAS REST interface can be used.

    ## SWAT 1.2.0.9000

Create a connection.

``` r
conn <- CAS('rdcgrdc.unx.sas.com', 63596)
```

    ## NOTE: Connecting to CAS and generating CAS action functions for loaded

    ##       action sets...

    ## NOTE: To generate the functions with signatures (for tab completion), set

    ##       options(cas.gen.function.sig=TRUE).

Run the help action.

``` r
out <- cas.builtins.help(conn)
```

    ## NOTE: Available Action Sets and Actions:

    ## NOTE:    accessControl

    ## NOTE:       accessPersonalCaslibs - Provides administrative access to all personal caslibs (CASUSER and CASUSERHDFS)

    ## NOTE:       assumeRole - Assumes a role

    ## NOTE:       dropRole - Relinquishes a role

    ## NOTE:       showRolesIn - Shows the currently active role

    ## NOTE:       showRolesAllowed - Shows the roles that a user is a member of

    ## NOTE:       isInRole - Shows whether a role is assumed

    ## NOTE:       isAuthorized - Shows whether access is authorized

    ## NOTE:       isAuthorizedActions - Shows whether access is authorized to actions

    ## NOTE:       isAuthorizedTables - Shows whether access is authorized to tables

    ## NOTE:       isAuthorizedColumns - Shows whether access is authorized to columns

    ## NOTE:       listAllPrincipals - Lists all principals that have explicit access controls

    ## NOTE:       whatIsEffective - Lists effective access and explanations (Origins)

    ## NOTE:       whatCheckoutsExist - Lists checkouts held on an object, its parents, and optionally its children

    ## NOTE:       checkOutObject - Reserves an object (and all of its children) for update by only the current client session. Prevents an object (and all of its parents) from being checked out exclusively by another session if checkOutType=Shared

    ## NOTE:       checkInAllObjects - Releases all objects that are checked out. Use this action if the current client session does not have a transaction

    ## NOTE:       startTransaction - Initiates an access control transaction in the current client session. Within a transaction, changes are private. Within a transaction, only exclusively reserved (checked-out) objects and their children can be updated. A transaction terminates when it is committed or rolled back

    ## NOTE:       commitTransaction - Persists all changes in an access control transaction to the server, releases all checked-out objects, and terminates the transaction

    ## NOTE:       rollbackTransaction - Discards all changes in an access control transaction, releases all checked-out objects, and terminates the transaction

    ## NOTE:       statusTransaction - Shows whether client session has an active transaction

    ## NOTE:       listAcsData - Lists access controls for caslibs, tables, and columns

    ## NOTE:       listAcsActionSet - Lists access controls for an action or action set

    ## NOTE:       repAllAcsCaslib - Replaces all access controls for a caslib

    ## NOTE:       repAllAcsTable - Replaces all access controls for a table

    ## NOTE:       repAllAcsColumn - Replaces all access controls for a column

    ## NOTE:       repAllAcsActionSet - Replaces all access controls for an action set

    ## NOTE:       repAllAcsAction - Replaces all access controls for an action

    ## NOTE:       updSomeAcsCaslib - Adds, deletes, and modifies some access controls for a caslib

    ## NOTE:       updSomeAcsTable - Adds, deletes, and modifies some access controls for a table

    ## NOTE:       updSomeAcsColumn - Adds, deletes, and modifies some access controls for a column

    ## NOTE:       updSomeAcsActionSet - Adds, deletes, and modifies some access controls for an action set

    ## NOTE:       updSomeAcsAction - Adds, deletes, and modifies some access controls for an action

    ## NOTE:       remAllAcsData - Removes all access controls for a caslib, table, or column

    ## NOTE:       remAllAcsActionSet - Removes all access controls for an action set or action

    ## NOTE:       operTableMd - Adds, deletes, and modifies table metadata

    ## NOTE:       operColumnMd - Adds, deletes, and modifies column metadata

    ## NOTE:       operActionSetMd - Adds, deletes, and modifies action set metadata

    ## NOTE:       operActionMd - Adds, deletes, and modifies action metadata

    ## NOTE:       operAdminMd - Assigns users and groups to roles and modifies administrator metadata

    ## NOTE:       listMetadata - Lists the metadata for caslibs, tables, columns, action sets, actions, or administrators

    ## NOTE:       createBackup - Creates a backup if one is not in progress

    ## NOTE:       completeBackup - Flags a backup as complete

    ## NOTE:       operBWPaths - Configures a blacklist or whitelist of paths

    ## NOTE:       deleteBWList - Deletes a blacklist or a whitelist

    ## NOTE:    builtins

    ## NOTE:       addNode - Adds a machine to the server

    ## NOTE:       removeNode - Remove one or more machines from the server

    ## NOTE:       help - Shows the parameters for an action or lists all available actions

    ## NOTE:       listNodes - Shows the host names used by the server

    ## NOTE:       loadActionSet - Loads an action set for use in this session

    ## NOTE:       installActionSet - Loads an action set in new sessions automatically

    ## NOTE:       log - Shows and modifies logging levels

    ## NOTE:       queryActionSet - Shows whether an action set is loaded

    ## NOTE:       queryName - Checks whether a name is an action or action set name

    ## NOTE:       reflect - Shows detailed parameter information for an action or all actions in an action set

    ## NOTE:       serverStatus - Shows the status of the server

    ## NOTE:       about - Shows the status of the server

    ## NOTE:       shutdown - Shuts down the server

    ## NOTE:       userInfo - Shows the user information for your connection

    ## NOTE:       actionSetInfo - Shows the build information from loaded action sets

    ## NOTE:       history - Shows the actions that were run in this session

    ## NOTE:       casCommon - Provides parameters that are common to many actions

    ## NOTE:       ping - Sends a single request to the server to confirm that the connection is working

    ## NOTE:       echo - Prints the supplied parameters to the client log

    ## NOTE:       modifyQueue - Modifies the action response queue settings

    ## NOTE:       getLicenseInfo - Shows the license information for a SAS product

    ## NOTE:       getLicensedProductInfo - Shows the information for licensed SAS products

    ## NOTE:       refreshLicense - Refresh SAS license information from a file

    ## NOTE:       httpAddress - Shows the HTTP address for the server monitor

    ## NOTE:       defineActionSet - Defines a new user-defined action set

    ## NOTE:       describeActionSet - Describes a user-defined action set

    ## NOTE:       dropActionSet - Drops a user-defined action set

    ## NOTE:       actionSetToTable - Makes an in-memory table from a user-defined action set

    ## NOTE:       actionSetFromTable - Restores a user-defined action set from a saved table

    ## NOTE:    configuration

    ## NOTE:       setServOpt - sets a server option

    ## NOTE:       getServOpt - displays the value of a server option

    ## NOTE:       listServOpts - Displays the server options and server values

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

    ## NOTE:    dataStep

    ## NOTE:       runCodeTable - Runs DATA step code stored in a CAS table

    ## NOTE:       runCode - Runs DATA step code

    ## NOTE:    percentile

    ## NOTE:       percentile - Calculate quantiles and percentiles

    ## NOTE:       boxPlot - Calculate quantiles, high and low whiskers, and outliers

    ## NOTE:       assess - Assess and compare models

    ## NOTE:    search

    ## NOTE:       searchIndex - Searches for a query against an index and retrieves records, documents, and tuples that are relevant to that query

    ## NOTE:       searchAggregate - Aggregates certain fields in a table that is usually generated by searchIndex

    ## NOTE:       valueCount - value count for multiple fields

    ## NOTE:       buildIndex - Creates an empty index using a schema (the first step of Search)

    ## NOTE:       getSchema - Gets the schema of an index

    ## NOTE:       appendIndex - Loads data to an index after the buildIndex action is performed

    ## NOTE:       deleteDocuments - Deletes a portion of documents from an index

    ## NOTE:    session

    ## NOTE:       listSessions - Displays a list of the sessions on the server

    ## NOTE:       addNodeStatus - Lists details about machines currently being added to the server

    ## NOTE:       timeout - Changes the time-out for a session

    ## NOTE:       actionstatus - Get action status for a session

    ## NOTE:       endSession - Ends the current session

    ## NOTE:       sessionId - Displays the name and UUID of the current session

    ## NOTE:       sessionName - Changes the name of the current session

    ## NOTE:       sessionStatus - Displays the status of the current session

    ## NOTE:       listresults - Lists the saved results for a session

    ## NOTE:       batchresults - Change current action to batch results

    ## NOTE:       fetchresult - Fetch the specified saved result for a session

    ## NOTE:       flushresult - Flush the saved result for this session

    ## NOTE:       setLocale - Changes the locale for the current session

    ## NOTE:       metrics - Displays the metrics for each action after it executes

    ## NOTE:    sessionProp

    ## NOTE:       setSessOpt - Sets a session option

    ## NOTE:       getSessOpt - Displays the value of a session option

    ## NOTE:       listSessOpts - Displays the session options and session values

    ## NOTE:       addFmtLib - Adds a format library

    ## NOTE:       listFmtLibs - Lists the format libraries that are associated with the session

    ## NOTE:       setFmtSearch - Sets the format libraries to search

    ## NOTE:       listFmtSearch - Shows the format library search order

    ## NOTE:       dropFmtLib - Drops a format library from global scope for all sessions

    ## NOTE:       deleteFormat - Deletes a format from a format library

    ## NOTE:       addFormat - Adds a format to a format library

    ## NOTE:       listFmtValues - Shows the values for a format

    ## NOTE:       saveFmtLib - Saves a format library

    ## NOTE:       promoteFmtLib - Promotes a format library to global scope for all sessions

    ## NOTE:       listFmtRanges - Displays the range information for a format

    ## NOTE:       combineFmtLibs - Combine a list of format libraries into a single format library.

    ## NOTE:    simple

    ## NOTE:       mdSummary - Calculates multidimensional summaries of numeric variables

    ## NOTE:       numRows - Shows the number of rows in a Cloud Analytic Services table

    ## NOTE:       summary - Generates descriptive statistics of numeric variables such as the sample mean, sample variance, sample size, sum of squares, and so on

    ## NOTE:       correlation - Computes Pearson product-moment correlations.

    ## NOTE:       regression - Performs a linear regression up to 3rd-order polynomials

    ## NOTE:       crossTab - Performs one-way or two-way tabulations

    ## NOTE:       distinct - Computes the distinct number of values of the variables in the variable list

    ## NOTE:       topK - Returns the top-K and bottom-K distinct values of each variable included in the variable list based on a user-specified ranking order

    ## NOTE:       groupBy - Builds BY groups in terms of the variable value combinations given the variables in the variable list

    ## NOTE:       freq - Generates a frequency distribution for one or more variables

    ## NOTE:       paraCoord - Generates a parallel coordinates plot of the variables in the variable list

    ## NOTE:       groupByInfo - Computes the index and frequency of each group-by group, and the index of each record within its group. Optionally keep or remove duplicates

    ## NOTE:    table

    ## NOTE:       view - Creates a view from files or tables

    ## NOTE:       attribute - Manages extended table attributes

    ## NOTE:       upload - Transfers binary data to the server to create objects like tables

    ## NOTE:       loadTable - Loads a table from a caslib's data source

    ## NOTE:       tableExists - Checks whether a table has been loaded

    ## NOTE:       index - Create indexes on one or more table variables

    ## NOTE:       columnInfo - Shows column information

    ## NOTE:       fetch - Fetches rows from a table or view

    ## NOTE:       save - Saves a table to a caslib's data source

    ## NOTE:       addTable - Add a table by sending it from the client to the server

    ## NOTE:       tableInfo - Shows information about a table

    ## NOTE:       tableDetails - Get detailed information about a table

    ## NOTE:       dropTable - Drops a table

    ## NOTE:       deleteSource - Delete a table or file from a caslib's data source

    ## NOTE:       fileInfo - Lists the files in a caslib's data source

    ## NOTE:       promote - Promote a table to global scope

    ## NOTE:       addCaslib - Adds a new caslib to enable access to a data source

    ## NOTE:       dropCaslib - Drops a caslib

    ## NOTE:       caslibInfo - Shows caslib information

    ## NOTE:       queryCaslib - Checks whether a caslib exists

    ## NOTE:       partition - Partitions a table

    ## NOTE:       shuffle - Randomly shuffles a table

    ## NOTE:       recordCount - Shows the number of rows in a Cloud Analytic Services table

    ## NOTE:       loadDataSource - Loads one or more data source interfaces

    ## NOTE:       update - Updates rows in a table

    ## NOTE:       alterTable - Rename tables, change labels and formats, drop columns

``` r
names(out)
```

    ##  [1] "accessControl"  "builtins"       "configuration"  "dataPreprocess"
    ##  [5] "dataStep"       "percentile"     "search"         "session"       
    ##  [9] "sessionProp"    "simple"         "table"

``` r
out
```

    ## $accessControl
    ##                     name
    ## 1  accessPersonalCaslibs
    ## 2             assumeRole
    ## 3               dropRole
    ## 4            showRolesIn
    ## 5       showRolesAllowed
    ## 6               isInRole
    ## 7           isAuthorized
    ## 8    isAuthorizedActions
    ## 9     isAuthorizedTables
    ## 10   isAuthorizedColumns
    ## 11     listAllPrincipals
    ## 12       whatIsEffective
    ## 13    whatCheckoutsExist
    ## 14        checkOutObject
    ## 15     checkInAllObjects
    ## 16      startTransaction
    ## 17     commitTransaction
    ## 18   rollbackTransaction
    ## 19     statusTransaction
    ## 20           listAcsData
    ## 21      listAcsActionSet
    ## 22       repAllAcsCaslib
    ## 23        repAllAcsTable
    ## 24       repAllAcsColumn
    ## 25    repAllAcsActionSet
    ## 26       repAllAcsAction
    ## 27      updSomeAcsCaslib
    ## 28       updSomeAcsTable
    ## 29      updSomeAcsColumn
    ## 30   updSomeAcsActionSet
    ## 31      updSomeAcsAction
    ## 32         remAllAcsData
    ## 33    remAllAcsActionSet
    ## 34           operTableMd
    ## 35          operColumnMd
    ## 36       operActionSetMd
    ## 37          operActionMd
    ## 38           operAdminMd
    ## 39          listMetadata
    ## 40          createBackup
    ## 41        completeBackup
    ## 42           operBWPaths
    ## 43          deleteBWList
    ##                                                                                                                                                                                                                                                         description
    ## 1                                                                                                                                                                                  Provides administrative access to all personal caslibs (CASUSER and CASUSERHDFS)
    ## 2                                                                                                                                                                                                                                                    Assumes a role
    ## 3                                                                                                                                                                                                                                               Relinquishes a role
    ## 4                                                                                                                                                                                                                                   Shows the currently active role
    ## 5                                                                                                                                                                                                                        Shows the roles that a user is a member of
    ## 6                                                                                                                                                                                                                                   Shows whether a role is assumed
    ## 7                                                                                                                                                                                                                                Shows whether access is authorized
    ## 8                                                                                                                                                                                                                     Shows whether access is authorized to actions
    ## 9                                                                                                                                                                                                                      Shows whether access is authorized to tables
    ## 10                                                                                                                                                                                                                    Shows whether access is authorized to columns
    ## 11                                                                                                                                                                                                          Lists all principals that have explicit access controls
    ## 12                                                                                                                                                                                                                Lists effective access and explanations (Origins)
    ## 13                                                                                                                                                                                      Lists checkouts held on an object, its parents, and optionally its children
    ## 14                                             Reserves an object (and all of its children) for update by only the current client session. Prevents an object (and all of its parents) from being checked out exclusively by another session if checkOutType=Shared
    ## 15                                                                                                                                             Releases all objects that are checked out. Use this action if the current client session does not have a transaction
    ## 16 Initiates an access control transaction in the current client session. Within a transaction, changes are private. Within a transaction, only exclusively reserved (checked-out) objects and their children can be updated. A transaction terminates when it is c
    ## 17                                                                                                                            Persists all changes in an access control transaction to the server, releases all checked-out objects, and terminates the transaction
    ## 18                                                                                                                                          Discards all changes in an access control transaction, releases all checked-out objects, and terminates the transaction
    ## 19                                                                                                                                                                                                           Shows whether client session has an active transaction
    ## 20                                                                                                                                                                                                           Lists access controls for caslibs, tables, and columns
    ## 21                                                                                                                                                                                                                Lists access controls for an action or action set
    ## 22                                                                                                                                                                                                                        Replaces all access controls for a caslib
    ## 23                                                                                                                                                                                                                         Replaces all access controls for a table
    ## 24                                                                                                                                                                                                                        Replaces all access controls for a column
    ## 25                                                                                                                                                                                                                   Replaces all access controls for an action set
    ## 26                                                                                                                                                                                                                       Replaces all access controls for an action
    ## 27                                                                                                                                                                                                    Adds, deletes, and modifies some access controls for a caslib
    ## 28                                                                                                                                                                                                     Adds, deletes, and modifies some access controls for a table
    ## 29                                                                                                                                                                                                    Adds, deletes, and modifies some access controls for a column
    ## 30                                                                                                                                                                                               Adds, deletes, and modifies some access controls for an action set
    ## 31                                                                                                                                                                                                   Adds, deletes, and modifies some access controls for an action
    ## 32                                                                                                                                                                                                       Removes all access controls for a caslib, table, or column
    ## 33                                                                                                                                                                                                          Removes all access controls for an action set or action
    ## 34                                                                                                                                                                                                                       Adds, deletes, and modifies table metadata
    ## 35                                                                                                                                                                                                                      Adds, deletes, and modifies column metadata
    ## 36                                                                                                                                                                                                                  Adds, deletes, and modifies action set metadata
    ## 37                                                                                                                                                                                                                      Adds, deletes, and modifies action metadata
    ## 38                                                                                                                                                                                            Assigns users and groups to roles and modifies administrator metadata
    ## 39                                                                                                                                                                         Lists the metadata for caslibs, tables, columns, action sets, actions, or administrators
    ## 40                                                                                                                                                                                                                       Creates a backup if one is not in progress
    ## 41                                                                                                                                                                                                                                       Flags a backup as complete
    ## 42                                                                                                                                                                                                                     Configures a blacklist or whitelist of paths
    ## 43                                                                                                                                                                                                                               Deletes a blacklist or a whitelist
    ## 
    ## $builtins
    ##                      name
    ## 1                 addNode
    ## 2              removeNode
    ## 3                    help
    ## 4               listNodes
    ## 5           loadActionSet
    ## 6        installActionSet
    ## 7                     log
    ## 8          queryActionSet
    ## 9               queryName
    ## 10                reflect
    ## 11           serverStatus
    ## 12                  about
    ## 13               shutdown
    ## 14               userInfo
    ## 15          actionSetInfo
    ## 16                history
    ## 17              casCommon
    ## 18                   ping
    ## 19                   echo
    ## 20            modifyQueue
    ## 21         getLicenseInfo
    ## 22 getLicensedProductInfo
    ## 23         refreshLicense
    ## 24            httpAddress
    ## 25        defineActionSet
    ## 26      describeActionSet
    ## 27          dropActionSet
    ## 28       actionSetToTable
    ## 29     actionSetFromTable
    ##                                                                           description
    ## 1                                                        Adds a machine to the server
    ## 2                                         Remove one or more machines from the server
    ## 3                   Shows the parameters for an action or lists all available actions
    ## 4                                             Shows the host names used by the server
    ## 5                                         Loads an action set for use in this session
    ## 6                                   Loads an action set in new sessions automatically
    ## 7                                                   Shows and modifies logging levels
    ## 8                                               Shows whether an action set is loaded
    ## 9                               Checks whether a name is an action or action set name
    ## 10 Shows detailed parameter information for an action or all actions in an action set
    ## 11                                                     Shows the status of the server
    ## 12                                                     Shows the status of the server
    ## 13                                                              Shuts down the server
    ## 14                                     Shows the user information for your connection
    ## 15                                Shows the build information from loaded action sets
    ## 16                                    Shows the actions that were run in this session
    ## 17                                Provides parameters that are common to many actions
    ## 18     Sends a single request to the server to confirm that the connection is working
    ## 19                                   Prints the supplied parameters to the client log
    ## 20                                        Modifies the action response queue settings
    ## 21                                    Shows the license information for a SAS product
    ## 22                                    Shows the information for licensed SAS products
    ## 23                                        Refresh SAS license information from a file
    ## 24                                      Shows the HTTP address for the server monitor
    ## 25                                              Defines a new user-defined action set
    ## 26                                                Describes a user-defined action set
    ## 27                                                    Drops a user-defined action set
    ## 28                            Makes an in-memory table from a user-defined action set
    ## 29                              Restores a user-defined action set from a saved table
    ## 
    ## $configuration
    ##           name                                   description
    ## 1   setServOpt                          sets a server option
    ## 2   getServOpt         displays the value of a server option
    ## 3 listServOpts Displays the server options and server values
    ## 
    ## $dataPreprocess
    ##               name
    ## 1          rustats
    ## 2           impute
    ## 3          outlier
    ## 4          binning
    ## 5       discretize
    ## 6         catTrans
    ## 7        histogram
    ## 8        transform
    ## 9              kde
    ## 10 highCardinality
    ##                                                                                                                                                                                    description
    ## 1                                                                                 Computes robust univariate statistics, centralized moments, quantiles, and frequency distribution statistics
    ## 2                                                                                                                                                   Performs data matrix (variable) imputation
    ## 3                                                                                                                                                     Performs outlier detection and treatment
    ## 4                                                                                                                                                Performs unsupervised variable discretization
    ## 5                                                                                                                                 Performs supervised and unsupervised variable discretization
    ## 6                                                                                               Groups and encodes categorical variables using unsupervised and supervised grouping techniques
    ## 7                                                                                                               Generates histogram bins and simple bin-based statistics for numeric variables
    ## 8  Performs pipelined variable imputation, outlier detection and treatment, functional transformation, binning, and robust univariate statistics to evaluate the quality of the transformation
    ## 9                                                                                                                                                           Computes kernel density estimation
    ## 10                                                                                                                                                  Performs randomized cardinality estimation
    ## 
    ## $dataStep
    ##           name                               description
    ## 1 runCodeTable Runs DATA step code stored in a CAS table
    ## 2      runCode                       Runs DATA step code
    ## 
    ## $percentile
    ##         name                                              description
    ## 1 percentile                      Calculate quantiles and percentiles
    ## 2    boxPlot Calculate quantiles, high and low whiskers, and outliers
    ## 3     assess                                Assess and compare models
    ## 
    ## $search
    ##              name
    ## 1     searchIndex
    ## 2 searchAggregate
    ## 3      valueCount
    ## 4      buildIndex
    ## 5       getSchema
    ## 6     appendIndex
    ## 7 deleteDocuments
    ##                                                                                                          description
    ## 1 Searches for a query against an index and retrieves records, documents, and tuples that are relevant to that query
    ## 2                                      Aggregates certain fields in a table that is usually generated by searchIndex
    ## 3                                                                                    value count for multiple fields
    ## 4                                                   Creates an empty index using a schema (the first step of Search)
    ## 5                                                                                        Gets the schema of an index
    ## 6                                                    Loads data to an index after the buildIndex action is performed
    ## 7                                                                       Deletes a portion of documents from an index
    ## 
    ## $session
    ##             name
    ## 1   listSessions
    ## 2  addNodeStatus
    ## 3        timeout
    ## 4   actionstatus
    ## 5     endSession
    ## 6      sessionId
    ## 7    sessionName
    ## 8  sessionStatus
    ## 9    listresults
    ## 10  batchresults
    ## 11   fetchresult
    ## 12   flushresult
    ## 13     setLocale
    ## 14       metrics
    ##                                                         description
    ## 1                     Displays a list of the sessions on the server
    ## 2  Lists details about machines currently being added to the server
    ## 3                                Changes the time-out for a session
    ## 4                                   Get action status for a session
    ## 5                                          Ends the current session
    ## 6                 Displays the name and UUID of the current session
    ## 7                           Changes the name of the current session
    ## 8                        Displays the status of the current session
    ## 9                             Lists the saved results for a session
    ## 10                           Change current action to batch results
    ## 11                   Fetch the specified saved result for a session
    ## 12                          Flush the saved result for this session
    ## 13                       Changes the locale for the current session
    ## 14           Displays the metrics for each action after it executes
    ## 
    ## $sessionProp
    ##              name
    ## 1      setSessOpt
    ## 2      getSessOpt
    ## 3    listSessOpts
    ## 4       addFmtLib
    ## 5     listFmtLibs
    ## 6    setFmtSearch
    ## 7   listFmtSearch
    ## 8      dropFmtLib
    ## 9    deleteFormat
    ## 10      addFormat
    ## 11  listFmtValues
    ## 12     saveFmtLib
    ## 13  promoteFmtLib
    ## 14  listFmtRanges
    ## 15 combineFmtLibs
    ##                                                         description
    ## 1                                             Sets a session option
    ## 2                            Displays the value of a session option
    ## 3                   Displays the session options and session values
    ## 4                                             Adds a format library
    ## 5   Lists the format libraries that are associated with the session
    ## 6                               Sets the format libraries to search
    ## 7                             Shows the format library search order
    ## 8         Drops a format library from global scope for all sessions
    ## 9                            Deletes a format from a format library
    ## 10                                Adds a format to a format library
    ## 11                                    Shows the values for a format
    ## 12                                           Saves a format library
    ## 13       Promotes a format library to global scope for all sessions
    ## 14                      Displays the range information for a format
    ## 15 Combine a list of format libraries into a single format library.
    ## 
    ## $simple
    ##           name
    ## 1    mdSummary
    ## 2      numRows
    ## 3      summary
    ## 4  correlation
    ## 5   regression
    ## 6     crossTab
    ## 7     distinct
    ## 8         topK
    ## 9      groupBy
    ## 10        freq
    ## 11   paraCoord
    ## 12 groupByInfo
    ##                                                                                                                                     description
    ## 1                                                                                    Calculates multidimensional summaries of numeric variables
    ## 2                                                                                   Shows the number of rows in a Cloud Analytic Services table
    ## 3        Generates descriptive statistics of numeric variables such as the sample mean, sample variance, sample size, sum of squares, and so on
    ## 4                                                                                                 Computes Pearson product-moment correlations.
    ## 5                                                                                      Performs a linear regression up to 3rd-order polynomials
    ## 6                                                                                                       Performs one-way or two-way tabulations
    ## 7                                                                  Computes the distinct number of values of the variables in the variable list
    ## 8         Returns the top-K and bottom-K distinct values of each variable included in the variable list based on a user-specified ranking order
    ## 9                                         Builds BY groups in terms of the variable value combinations given the variables in the variable list
    ## 10                                                                                 Generates a frequency distribution for one or more variables
    ## 11                                                                  Generates a parallel coordinates plot of the variables in the variable list
    ## 12 Computes the index and frequency of each group-by group, and the index of each record within its group. Optionally keep or remove duplicates
    ## 
    ## $table
    ##              name
    ## 1            view
    ## 2       attribute
    ## 3          upload
    ## 4       loadTable
    ## 5     tableExists
    ## 6           index
    ## 7      columnInfo
    ## 8           fetch
    ## 9            save
    ## 10       addTable
    ## 11      tableInfo
    ## 12   tableDetails
    ## 13      dropTable
    ## 14   deleteSource
    ## 15       fileInfo
    ## 16        promote
    ## 17      addCaslib
    ## 18     dropCaslib
    ## 19     caslibInfo
    ## 20    queryCaslib
    ## 21      partition
    ## 22        shuffle
    ## 23    recordCount
    ## 24 loadDataSource
    ## 25         update
    ## 26     alterTable
    ##                                                          description
    ## 1                                Creates a view from files or tables
    ## 2                                  Manages extended table attributes
    ## 3  Transfers binary data to the server to create objects like tables
    ## 4                          Loads a table from a caslib's data source
    ## 5                             Checks whether a table has been loaded
    ## 6                      Create indexes on one or more table variables
    ## 7                                           Shows column information
    ## 8                                  Fetches rows from a table or view
    ## 9                            Saves a table to a caslib's data source
    ## 10           Add a table by sending it from the client to the server
    ## 11                                   Shows information about a table
    ## 12                            Get detailed information about a table
    ## 13                                                     Drops a table
    ## 14                Delete a table or file from a caslib's data source
    ## 15                         Lists the files in a caslib's data source
    ## 16                                   Promote a table to global scope
    ## 17               Adds a new caslib to enable access to a data source
    ## 18                                                    Drops a caslib
    ## 19                                          Shows caslib information
    ## 20                                    Checks whether a caslib exists
    ## 21                                                Partitions a table
    ## 22                                         Randomly shuffles a table
    ## 23       Shows the number of rows in a Cloud Analytic Services table
    ## 24                          Loads one or more data source interfaces
    ## 25                                           Updates rows in a table
    ## 26            Rename tables, change labels and formats, drop columns

``` r
out$builtins
```

    ##                      name
    ## 1                 addNode
    ## 2              removeNode
    ## 3                    help
    ## 4               listNodes
    ## 5           loadActionSet
    ## 6        installActionSet
    ## 7                     log
    ## 8          queryActionSet
    ## 9               queryName
    ## 10                reflect
    ## 11           serverStatus
    ## 12                  about
    ## 13               shutdown
    ## 14               userInfo
    ## 15          actionSetInfo
    ## 16                history
    ## 17              casCommon
    ## 18                   ping
    ## 19                   echo
    ## 20            modifyQueue
    ## 21         getLicenseInfo
    ## 22 getLicensedProductInfo
    ## 23         refreshLicense
    ## 24            httpAddress
    ## 25        defineActionSet
    ## 26      describeActionSet
    ## 27          dropActionSet
    ## 28       actionSetToTable
    ## 29     actionSetFromTable
    ##                                                                           description
    ## 1                                                        Adds a machine to the server
    ## 2                                         Remove one or more machines from the server
    ## 3                   Shows the parameters for an action or lists all available actions
    ## 4                                             Shows the host names used by the server
    ## 5                                         Loads an action set for use in this session
    ## 6                                   Loads an action set in new sessions automatically
    ## 7                                                   Shows and modifies logging levels
    ## 8                                               Shows whether an action set is loaded
    ## 9                               Checks whether a name is an action or action set name
    ## 10 Shows detailed parameter information for an action or all actions in an action set
    ## 11                                                     Shows the status of the server
    ## 12                                                     Shows the status of the server
    ## 13                                                              Shuts down the server
    ## 14                                     Shows the user information for your connection
    ## 15                                Shows the build information from loaded action sets
    ## 16                                    Shows the actions that were run in this session
    ## 17                                Provides parameters that are common to many actions
    ## 18     Sends a single request to the server to confirm that the connection is working
    ## 19                                   Prints the supplied parameters to the client log
    ## 20                                        Modifies the action response queue settings
    ## 21                                    Shows the license information for a SAS product
    ## 22                                    Shows the information for licensed SAS products
    ## 23                                        Refresh SAS license information from a file
    ## 24                                      Shows the HTTP address for the server monitor
    ## 25                                              Defines a new user-defined action set
    ## 26                                                Describes a user-defined action set
    ## 27                                                    Drops a user-defined action set
    ## 28                            Makes an in-memory table from a user-defined action set
    ## 29                              Restores a user-defined action set from a saved table

Running CAS Actions
-------------------

For example, the userInfo action is called as follows.

``` r
cas.builtins.userInfo(conn)
```

    ## $userInfo
    ## $userInfo$anonymous
    ## [1] FALSE
    ## 
    ## $userInfo$groups
    ## $userInfo$groups[[1]]
    ## [1] "users"
    ## 
    ## 
    ## $userInfo$guest
    ## [1] FALSE
    ## 
    ## $userInfo$hostAccount
    ## [1] FALSE
    ## 
    ## $userInfo$providedName
    ## [1] "sasyqi"
    ## 
    ## $userInfo$providerName
    ## [1] "Active Directory"
    ## 
    ## $userInfo$uniqueId
    ## [1] "sasyqi"
    ## 
    ## $userInfo$userId
    ## [1] "sasyqi"

Loading Data
------------

We use the classic Iris data set in the following data loading example.

``` r
iris_ct <- as.casTable(conn,iris,casOut = list(replace = TRUE))
```

    ## NOTE: Cloud Analytic Services made the uploaded file available as table IRIS in caslib CASUSERHDFS(sasyqi).

``` r
attributes(iris_ct)
```

    ## $conn
    ## CAS(hostname=rdcgrdc.unx.sas.com, port=63596, username=sasyqi, session=67b38040-9e1f-1e4b-bc96-c6ce51c624f9, protocol=http)
    ## 
    ## $tname
    ## [1] "IRIS"
    ## 
    ## $caslib
    ## [1] "CASUSERHDFS(sasyqi)"
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

We can use actions such as tableInfo and columnInfo in the table action set to access general information about the table itself and its columns.

``` r
# Call the tableInfo action on the CASTable object.
cas.table.tableInfo(conn)
```

    ## $TableInfo
    ##   Name Rows Columns IndexedColumns Encoding       CreateTimeFormatted
    ## 1 IRIS  150       5              0    utf-8 2018-06-29T15:35:34-04:00
    ##            ModTimeFormatted       AccessTimeFormatted JavaCharSet
    ## 1 2018-06-29T15:35:34-04:00 2018-06-29T15:35:34-04:00        UTF8
    ##   CreateTime    ModTime AccessTime Global Repeated View SourceName
    ## 1 1845920134 1845920134 1845920134      0        0    0           
    ##   SourceCaslib Compressed Creator Modifier    SourceModTimeFormatted
    ## 1                       0  sasyqi          2018-06-29T15:35:33-04:00
    ##   SourceModTime
    ## 1    1845920133

Executing Actions on CAS Tables
-------------------------------

Let's run the summary action in simple action set on our CAS table.

``` r
summ <- cas.simple.summary(iris_ct)
summ
```

    ## $Summary
    ##         Column Min Max   N NMiss     Mean   Sum       Std     StdErr
    ## 1 Sepal.Length 4.3 7.9 150     0 5.843333 876.5 0.8280661 0.06761132
    ## 2  Sepal.Width 2.0 4.4 150     0 3.057333 458.6 0.4358663 0.03558833
    ## 3 Petal.Length 1.0 6.9 150     0 3.758000 563.7 1.7652982 0.14413600
    ## 4  Petal.Width 0.1 2.5 150     0 1.199333 179.9 0.7622377 0.06223645
    ##         Var     USS       CSS       CV   TValue         ProbT   Skewness
    ## 1 0.6856935 5223.85 102.16833 14.17113 86.42537 3.331256e-129  0.3149110
    ## 2 0.1899794 1430.40  28.30693 14.25642 85.90830 8.004458e-129  0.3189657
    ## 3 3.1162779 2582.71 464.32540 46.97441 26.07260  2.166017e-57 -0.2748842
    ## 4 0.5810063  302.33  86.56993 63.55511 19.27060  2.659021e-42 -0.1029667
    ##    Kurtosis
    ## 1 -0.552064
    ## 2  0.228249
    ## 3 -1.402103
    ## 4 -1.340604

If you want them in a form similar to what R users are used to, you can use the summary() method (just like on R data.frame objects).

``` r
# Waiting for bug fixing
#summary(iris_ct)
```

Data Visualization
------------------

The following example uses the plot method to download the data set and plot it using the default options. To prevent downloading very large data set to the client, only 10,000 rows is randomly sampled and downloaded if the data set has more than 10,000 rows.

``` r
plot(iris_ct$Sepal.Length, iris_ct$Sepal.Width)
```

![](https://raw.githubusercontent.com/qi-yue/sas-viya-the-R-perspective/master/figures/2_1.png)

Closing the Connection
----------------------

As with any network or file resource in R, you should close your CAS connections when you are finished.

``` r
cas.session.endSession(conn)
```

    ## list()

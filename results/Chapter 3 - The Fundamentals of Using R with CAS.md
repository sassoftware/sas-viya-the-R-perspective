Chapter 3 - The Fundamentals of R with Viya
================
Yue Qi

``` r
knitr::opts_chunk$set()
```

Connecting to CAS
-----------------

Import the SWAT package.

``` r
library("swat")
```

    ## NOTE: The extension module for binary protocol support is not available.

    ##       Only the CAS REST interface can be used.

    ## SWAT 1.2.0.9000

Create a connection.

``` r
conn <- CAS('rdcgrdc.unx.sas.com', 25970)
```

    ## NOTE: Connecting to CAS and generating CAS action functions for loaded

    ##       action sets...

    ## NOTE: To generate the functions with signatures (for tab completion), set

    ##       options(cas.gen.function.sig=TRUE).

Running CAS Actions
-------------------

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

The following code displays the help content for the builtins action set.

``` r
out <- cas.builtins.help(conn, actionset = 'builtins')
```

    ## NOTE: Information for action set 'builtins':

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

Display the Help for the help action.

``` r
out <- cas.builtins.help(conn, action = 'help')
```

    ## NOTE: Information for action 'builtins.help':

    ## NOTE: The following parameters are accepted.  Default values are shown.

    ## NOTE:    string action=NULL,

    ## NOTE:       specifies the name of the action for which you want help. The name can be in the form 'actionSetName.actionName' or just 'actionName.

    ## NOTE:    string actionSet=NULL,

    ## NOTE:       specifies the name of the action set for which you want help. This parameter is ignored if the action parameter is specified.

    ## NOTE:    boolean verbose=true

    ## NOTE:       when set to True, provides more detail for each parameter.

Suppress the parameter descriptions by specifying verbose=False.

``` r
out <- cas.builtins.help(conn, action = 'help', verbose = FALSE)
```

    ## NOTE: Information for action 'builtins.help':

    ## NOTE: The following parameters are accepted.  Default values are shown.

    ## NOTE:    string action=NULL,

    ## NOTE:    string actionSet=NULL,

    ## NOTE:    boolean verbose=true

Let's see what the R help function displays.

``` r
help(as.casTable)
```

    ## starting httpd help server ... done

You can also use a question mark followed by the function name.

``` r
?as.casTable
```

You can also use tab-completion on action sets in RStudio or other IDEs that support it.

``` r
cas.builtins.<tab>
```

### Specifying Action Parameters

Using the echo action to practice more complex parameter types.

``` r
out <- cas.builtins.help(conn, action = 'help')
```

    ## NOTE: Information for action 'builtins.help':

    ## NOTE: The following parameters are accepted.  Default values are shown.

    ## NOTE:    string action=NULL,

    ## NOTE:       specifies the name of the action for which you want help. The name can be in the form 'actionSetName.actionName' or just 'actionName.

    ## NOTE:    string actionSet=NULL,

    ## NOTE:       specifies the name of the action set for which you want help. This parameter is ignored if the action parameter is specified.

    ## NOTE:    boolean verbose=true

    ## NOTE:       when set to True, provides more detail for each parameter.

Using nested structures in parameters.

``` r
out <- cas.builtins.echo(
  conn,
  boolean_true = TRUE,
  boolean_false = FALSE,
  double = 3.14159,
  int32 = 1776,
  int64 = 2**60,
  string = 'I like snowmen! \u2603',
  vector = c('item1', 'item2', 'item3'),
  list = list(key1 = 'value1',
              key2 = 'value2',
              key3 = 3)
  )
```

    ## NOTE: builtin.echo called with 9 parameters.

    ## NOTE:    parameter 1: _messagelevel = 'note'

    ## NOTE:    parameter 2: boolean_false = false

    ## NOTE:    parameter 3: boolean_true = true

    ## NOTE:    parameter 4: double = 3.1416

    ## NOTE:    parameter 5: int32 = 1776

    ## NOTE:    parameter 6: int64 = 1.15292e+18

    ## NOTE:    parameter 7: list = {key1 = 'value1', key2 = 'value2', key3 = 3}

    ## NOTE:    parameter 8: string = 'I like snowmen! <U+2603>'

    ## NOTE:    parameter 9: vector = {'item1', 'item2', 'item3'}

Using nested structures in parameters.

``` r
out <- cas.builtins.echo(
  conn,
  list = list(
    'item1', 
    'item2',
    list(
      key1 = 'value1',
      key2 = list(
        value2 = c(0, 1, 1, 2, 3)
        )
      )
    ))
```

    ## NOTE: builtin.echo called with 2 parameters.

    ## NOTE:    parameter 1: _messagelevel = 'note'

    ## NOTE:    parameter 2: list = {'item1', 'item2', {key1 = 'value1', key2 = {value2 = {0, 1, 1, 2, 3}}}}

#### Automatic Type Casting

The CAS server will attempt to cast values to the appropriate type. Here we are getting the history from the server using integers.

``` r
# Using integers
out <- cas.builtins.history(conn, first = 5, last = 7)
```

    ## NOTE: 5: action builtins.actionSetInfo / extensions={'accessControl'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 6: action builtins.listActions / actionSet='accessControl', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 7: action builtins.actionSetInfo / extensions={'builtins'}, _messageLevel='error'; /* (SUCCESS) */

Here we are using strings, which get converted to integers on the server side.

``` r
# Using strings as integer values
out <- cas.builtins.history(conn, first = '5', last = '7')
```

    ## NOTE: 5: action builtins.actionSetInfo / extensions={'accessControl'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 6: action builtins.listActions / actionSet='accessControl', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 7: action builtins.actionSetInfo / extensions={'builtins'}, _messageLevel='error'; /* (SUCCESS) */

#### Scalar Parameter to List Conversion

Many times when using an action parameter that requires a list as an argument, you use only the first key in the list to specify the parameter. For example, the builtins.help action takes a parameter called casout. This parameter specifies an output table to put the history information into.

``` r
out <- cas.builtins.help(conn, action = 'history')
```

    ## NOTE: Information for action 'builtins.history':

    ## NOTE: The following parameters are accepted.  Default values are shown.

    ## NOTE:    int32 first=1,

    ## NOTE:       specifies the ordinal position for the first action to report on. Negative values are subtracted from the current action's ordinal position.

    ## NOTE:    int32 last=-1,

    ## NOTE:       specifies the ordinal position of the last action to report on. Negative values are subtracted from the current action's ordinal position.

    ## NOTE:    boolean verbose=true,

    ## NOTE:       prints action information to the client log as well as returns the action information in the results.

    ## NOTE:    list casOut={

    ## NOTE:       specifies the settings for saving the action history to an output table.

    ## NOTE:       string name=NULL,

    ## NOTE:       specifies the name to associate with the table.

    ## NOTE:       string caslib=NULL,

    ## NOTE:       specifies the name of the caslib to use.

    ## NOTE:       string timeStamp=NULL,

    ## NOTE:       specifies the timestamp to apply to the table. Specify the value in the form that is appropriate for your session locale.

    ## NOTE:       boolean compress=false,

    ## NOTE:       when set to True, data compression is applied to the table.

    ## NOTE:       boolean replace=false,

    ## NOTE:       when set to True, overwrites an existing table with the same name.

    ## NOTE:       int32 replication=1 (value >= 0),

    ## NOTE:       specifies the number of copies of the table to make for fault tolerance. Larger values result in slower performance and use more memory, but provide high availability for data in the event of a node failure.

    ## NOTE:       string label=NULL,

    ## NOTE:       specifies the descriptive label to associate with the table.

    ## NOTE:       int64 maxMemSize=0,

    ## NOTE:       specifies the maximum amount of memory, in bytes, that each thread should allocate for in-memory blocks before converting to a memory-mapped file. Files are written in the directories that are specified in the CAS_DISK_CACHE environment variable.

    ## NOTE:       boolean promote=false,

    ## NOTE:       when set to True, adds the output table with a global scope. This enables other sessions to access the table, subject to access controls. The target caslib must also have a global scope.

    ## NOTE:       boolean onDemand=true,

    ## NOTE:       This parameter is deprecated.

    ## NOTE:       string list where={

    ## NOTE:       specifies an expression for subsetting the output data.

    ## NOTE:       },

    ## NOTE:       specifies an expression for subsetting the output data.

    ## NOTE:       string list indexVars={

    ## NOTE:       specifies the list of variables to create indexes for in the output data

    ## NOTE:       }

    ## NOTE:       specifies the list of variables to create indexes for in the output data

    ## NOTE:    }

    ## NOTE:       specifies the settings for saving the action history to an output table.

The first key in the casout parameter is name and indicates the name of the CAS table to create. The complete way of specifying this parameter with only the name key follows:

``` r
out <- cas.builtins.history(conn, casout = list(name='hist'))
```

    ## NOTE: 36 records were written to table 'hist

    ## NOTE: 1: action session.sessionId...; /* (SUCCESS) */

    ## NOTE: 2: action builtins.actionSetInfo / all=false, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 3: action builtins.actionSetInfo / extensions={'accessControl'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 4: action builtins.listActions / actionSet='accessControl', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 5: action builtins.actionSetInfo / extensions={'accessControl'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 6: action builtins.listActions / actionSet='accessControl', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 7: action builtins.actionSetInfo / extensions={'builtins'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 8: action builtins.listActions / actionSet='builtins', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 9: action builtins.actionSetInfo / extensions={'configuration'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 10: action builtins.listActions / actionSet='configuration', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 11: action builtins.actionSetInfo / extensions={'dataPreprocess'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 12: action builtins.listActions / actionSet='dataPreprocess', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 13: action builtins.actionSetInfo / extensions={'dataStep'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 14: action builtins.listActions / actionSet='dataStep', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 15: action builtins.actionSetInfo / extensions={'percentile'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 16: action builtins.listActions / actionSet='percentile', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 17: action builtins.actionSetInfo / extensions={'search'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 18: action builtins.listActions / actionSet='search', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 19: action builtins.actionSetInfo / extensions={'session'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 20: action builtins.listActions / actionSet='session', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 21: action builtins.actionSetInfo / extensions={'sessionProp'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 22: action builtins.listActions / actionSet='sessionProp', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 23: action builtins.actionSetInfo / extensions={'simple'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 24: action builtins.listActions / actionSet='simple', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 25: action builtins.actionSetInfo / extensions={'table'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 26: action builtins.listActions / actionSet='table', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 27: action builtins.help /  / _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 28: action builtins.help / actionSet='builtins', _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 29: action builtins.help / action='help', _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 30: action builtins.help / action='help', verbose=false, _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 31: action builtins.help / action='help', _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 32: action builtins.echo...; /* (SUCCESS) */

    ## NOTE: 33: action builtins.echo...; /* (SUCCESS) */

    ## NOTE: 34: action builtins.history / first=5, last=7, _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 35: action builtins.history / first=5, last=7, _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 36: action builtins.help / action='history', _messageLevel='note'; /* (SUCCESS) */

This is such a common idiom that the server enables you to specify list values with only the first specified key given (for example, name), just using the value of that key. That is a mouthful, but it is easier than it sounds. It just means that rather than having to use the list to create a nested list, you could simply do the following:

``` r
out <- cas.builtins.history(conn, casout = list(name = 'hist', replace = TRUE))
```

    ## NOTE: 37 records were written to table 'hist

    ## NOTE: 1: action session.sessionId...; /* (SUCCESS) */

    ## NOTE: 2: action builtins.actionSetInfo / all=false, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 3: action builtins.actionSetInfo / extensions={'accessControl'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 4: action builtins.listActions / actionSet='accessControl', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 5: action builtins.actionSetInfo / extensions={'accessControl'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 6: action builtins.listActions / actionSet='accessControl', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 7: action builtins.actionSetInfo / extensions={'builtins'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 8: action builtins.listActions / actionSet='builtins', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 9: action builtins.actionSetInfo / extensions={'configuration'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 10: action builtins.listActions / actionSet='configuration', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 11: action builtins.actionSetInfo / extensions={'dataPreprocess'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 12: action builtins.listActions / actionSet='dataPreprocess', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 13: action builtins.actionSetInfo / extensions={'dataStep'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 14: action builtins.listActions / actionSet='dataStep', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 15: action builtins.actionSetInfo / extensions={'percentile'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 16: action builtins.listActions / actionSet='percentile', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 17: action builtins.actionSetInfo / extensions={'search'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 18: action builtins.listActions / actionSet='search', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 19: action builtins.actionSetInfo / extensions={'session'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 20: action builtins.listActions / actionSet='session', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 21: action builtins.actionSetInfo / extensions={'sessionProp'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 22: action builtins.listActions / actionSet='sessionProp', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 23: action builtins.actionSetInfo / extensions={'simple'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 24: action builtins.listActions / actionSet='simple', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 25: action builtins.actionSetInfo / extensions={'table'}, _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 26: action builtins.listActions / actionSet='table', _messageLevel='error'; /* (SUCCESS) */

    ## NOTE: 27: action builtins.help /  / _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 28: action builtins.help / actionSet='builtins', _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 29: action builtins.help / action='help', _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 30: action builtins.help / action='help', verbose=false, _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 31: action builtins.help / action='help', _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 32: action builtins.echo...; /* (SUCCESS) */

    ## NOTE: 33: action builtins.echo...; /* (SUCCESS) */

    ## NOTE: 34: action builtins.history / first=5, last=7, _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 35: action builtins.history / first=5, last=7, _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 36: action builtins.help / action='history', _messageLevel='note'; /* (SUCCESS) */

    ## NOTE: 37: action builtins.history / casOut={name='hist'}, _messageLevel='note'; /* (SUCCESS) */

Of course, if you need to use any other keys in the casout parameter, you must use the list form. This conversion of a scalar value to a list value is common when specifying input tables and variable lists of tables, which we see later on.

### CAS Action Results

Up to now, all of our examples have stored the result of the action calls in a variable, but we have not done anything with the results yet. Let's start by using our example of all of the CAS parameter types.

``` r
out <- cas.builtins.echo(
  conn,
  boolean_true = TRUE,
  boolean_false = FALSE,
  double = 3.14159,
  int32 = 1776,
  int64 = 2**60,
  string = 'I like snowmen! \u2603',
  vector = c('item1', 'item2', 'item3'),
  list = list(key1 = 'value1',
              key2 = 'value2',
              key3 = 3)
)
```

    ## NOTE: builtin.echo called with 9 parameters.

    ## NOTE:    parameter 1: _messagelevel = 'note'

    ## NOTE:    parameter 2: boolean_false = false

    ## NOTE:    parameter 3: boolean_true = true

    ## NOTE:    parameter 4: double = 3.1416

    ## NOTE:    parameter 5: int32 = 1776

    ## NOTE:    parameter 6: int64 = 1.15292e+18

    ## NOTE:    parameter 7: list = {key1 = 'value1', key2 = 'value2', key3 = 3}

    ## NOTE:    parameter 8: string = 'I like snowmen! <U+2603>'

    ## NOTE:    parameter 9: vector = {'item1', 'item2', 'item3'}

Displaying the contents of the out variable gives:

``` r
out
```

    ## $`_messagelevel`
    ## [1] "note"
    ## 
    ## $boolean_false
    ## [1] FALSE
    ## 
    ## $boolean_true
    ## [1] TRUE
    ## 
    ## $double
    ## [1] 3.1416
    ## 
    ## $int32
    ## [1] 1776
    ## 
    ## $int64
    ## [1] 1.152922e+18
    ## 
    ## $list
    ## $list$key1
    ## [1] "value1"
    ## 
    ## $list$key2
    ## [1] "value2"
    ## 
    ## $list$key3
    ## [1] 3
    ## 
    ## 
    ## $string
    ## [1] "I like snowmen! <U+2603>"
    ## 
    ## $vector
    ## $vector[[1]]
    ## [1] "item1"
    ## 
    ## $vector[[2]]
    ## [1] "item2"
    ## 
    ## $vector[[3]]
    ## [1] "item3"

``` r
class(out)
```

    ## [1] "list"

The object that is held in the out variable a list object. You can traverse and modify the result just as you could any other R list object. For example, if you wanted to walk through the items and print each key and value explicitly, you could do the following:

``` r
for (key in names(out)){
  print(key)
  print(out[[key]])
  cat('\n')
}
```

    ## [1] "_messagelevel"
    ## [1] "note"
    ## 
    ## [1] "boolean_false"
    ## [1] FALSE
    ## 
    ## [1] "boolean_true"
    ## [1] TRUE
    ## 
    ## [1] "double"
    ## [1] 3.1416
    ## 
    ## [1] "int32"
    ## [1] 1776
    ## 
    ## [1] "int64"
    ## [1] 1.152922e+18
    ## 
    ## [1] "list"
    ## $key1
    ## [1] "value1"
    ## 
    ## $key2
    ## [1] "value2"
    ## 
    ## $key3
    ## [1] 3
    ## 
    ## 
    ## [1] "string"
    ## [1] "I like snowmen! <U+2603>"
    ## 
    ## [1] "vector"
    ## [[1]]
    ## [1] "item1"
    ## 
    ## [[2]]
    ## [1] "item2"
    ## 
    ## [[3]]
    ## [1] "item3"

#### Using casDataFrames

The following code runs the builtins.help action, lists the names in the list object that is returned, verifies that it is a casDataFrame object using R's class function, and displays the contents of the casDataFrame (some output is reformatted slightly for readability):

``` r
out <- cas.builtins.help(conn)
```

``` r
names(out)
```

    ##  [1] "accessControl"  "builtins"       "configuration"  "dataPreprocess"
    ##  [5] "dataStep"       "percentile"     "search"         "session"       
    ##  [9] "sessionProp"    "simple"         "table"

``` r
class(out[['builtins']])
```

    ## [1] "casDataFrame"
    ## attr(,"package")
    ## [1] "swat"

``` r
out[['builtins']]
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

We can store this casDataFrame in another variable to make it a bit easier to work with. Because the returned results are list objects, you can access keys as attributes. This means that we can access the builtins key of the out variable in either of the following ways:

``` r
blt <- out[['builtins']]
```

``` r
blt <- out$builtins 
```

Now that we have a handle on the casDataFrame, we can do typical data.frame operations on it such as sorting and filtering. For example, to sort the builtins actions by the name column, you might do the following.

``` r
blt[order(blt$name),]
```

    ##                      name
    ## 12                  about
    ## 29     actionSetFromTable
    ## 15          actionSetInfo
    ## 28       actionSetToTable
    ## 1                 addNode
    ## 17              casCommon
    ## 25        defineActionSet
    ## 26      describeActionSet
    ## 27          dropActionSet
    ## 19                   echo
    ## 22 getLicensedProductInfo
    ## 21         getLicenseInfo
    ## 3                    help
    ## 16                history
    ## 24            httpAddress
    ## 6        installActionSet
    ## 4               listNodes
    ## 5           loadActionSet
    ## 7                     log
    ## 20            modifyQueue
    ## 18                   ping
    ## 8          queryActionSet
    ## 9               queryName
    ## 10                reflect
    ## 23         refreshLicense
    ## 2              removeNode
    ## 11           serverStatus
    ## 13               shutdown
    ## 14               userInfo
    ##                                                                           description
    ## 12                                                     Shows the status of the server
    ## 29                              Restores a user-defined action set from a saved table
    ## 15                                Shows the build information from loaded action sets
    ## 28                            Makes an in-memory table from a user-defined action set
    ## 1                                                        Adds a machine to the server
    ## 17                                Provides parameters that are common to many actions
    ## 25                                              Defines a new user-defined action set
    ## 26                                                Describes a user-defined action set
    ## 27                                                    Drops a user-defined action set
    ## 19                                   Prints the supplied parameters to the client log
    ## 22                                    Shows the information for licensed SAS products
    ## 21                                    Shows the license information for a SAS product
    ## 3                   Shows the parameters for an action or lists all available actions
    ## 16                                    Shows the actions that were run in this session
    ## 24                                      Shows the HTTP address for the server monitor
    ## 6                                   Loads an action set in new sessions automatically
    ## 4                                             Shows the host names used by the server
    ## 5                                         Loads an action set for use in this session
    ## 7                                                   Shows and modifies logging levels
    ## 20                                        Modifies the action response queue settings
    ## 18     Sends a single request to the server to confirm that the connection is working
    ## 8                                               Shows whether an action set is loaded
    ## 9                               Checks whether a name is an action or action set name
    ## 10 Shows detailed parameter information for an action or all actions in an action set
    ## 23                                        Refresh SAS license information from a file
    ## 2                                         Remove one or more machines from the server
    ## 11                                                     Shows the status of the server
    ## 13                                                              Shuts down the server
    ## 14                                     Shows the user information for your connection

### Working with CAS Action Sets

See what action sets are available on our server. Depending on your installation and licensing, the list varies from system to system.

``` r
# Run the builtins.actionsetinfo action.
asinfo <- cas.builtins.actionSetInfo(conn, all = TRUE)

# Filter the casDataFrame to contain only action sets that
# have not been loaded yet.
asinfoNotLoaded <- asinfo$setinfo[asinfo$setinfo$loaded == 0, ]

# Create a new casDataFrame with only columns between 
# actionset and label.
asinfoNotLoaded <- asinfoNotLoaded[,c('actionset','label')]

asinfoNotLoaded
```

    ##                   actionset                         label
    ## 1                    access                              
    ## 3                actionTest                              
    ## 4               actionTest2                              
    ## 5               aggregation                              
    ## 6                    astore                              
    ## 7                     audio                              
    ## 8                  autotune                              
    ## 9     bayesianNetClassifier                              
    ## 10                 bglimmix                              
    ## 11              bioMedImage                              
    ## 12                 boolRule                              
    ## 14              cardinality                              
    ## 15                      cdm                              
    ## 16               clustering                              
    ## 17  conditionalRandomFields                              
    ## 19                   copula                              
    ## 20                 countreg                              
    ## 21            dataDiscovery                              
    ## 23              dataQuality                  Data Quality
    ## 25             decisionTree                              
    ## 26                deepLearn                              
    ## 27               deepNeural                              
    ## 28                  deepRnn                              
    ## 29                       dq                              
    ## 30                      ds2                              
    ## 31                      ecm                              
    ## 32            elasticsearch                              
    ## 33                entityRes                              
    ## 34               espCluster                              
    ## 35                  factmac                              
    ## 36                  fastKnn                              
    ## 37                     fcmp                          FCMP
    ## 38                     fcmp                          FCMP
    ## 39                   fedSql                              
    ## 40                  freqTab                              
    ## 41                      gam                              
    ## 42                    gleam                              
    ## 43                     glrm                              
    ## 44        graphSemiSupLearn                              
    ## 45              gVarCluster                              
    ## 46        hiddenMarkovModel                              
    ## 47               hyperGroup                              
    ## 48                      ica                              
    ## 49                    image                              
    ## 50                langModel                              
    ## 51                 ldaTopic                              
    ## 52            linearAlgebra                              
    ## 53              loadStreams                              
    ## 54              localSearch                              
    ## 55                      mbc                              
    ## 56                      mdc                              
    ## 57          midTierServices                              
    ## 58                    mixed                              
    ## 59                  mlTools                              
    ## 60          modelPublishing                              
    ## 61                  network                       Network
    ## 62                  network                       Network
    ## 63      networkOptimization                              
    ## 64                neuralNet                              
    ## 65                nonlinear                              
    ## 66       nonParametricBayes                              
    ## 67             optimization                              
    ## 68                 override                              
    ## 69                    panel                              
    ## 70                      pca                              
    ## 72                    phreg                              
    ## 73                      pls                              
    ## 74               pseudoL10n                              
    ## 75                     qlim                              
    ## 76                 quantreg                              
    ## 77                recommend                              
    ## 78               regression                              
    ## 79              riskMethods                              
    ## 80              riskResults                              
    ## 81                  riskRun                              
    ## 82                  riskSim                              
    ## 83                robustPca                              
    ## 84                    rteng                              
    ## 85               ruleMining                              
    ## 86                 sampling                              
    ## 87                   sccasl                              
    ## 89          searchAnalytics                              
    ## 90        sentimentAnalysis                              
    ## 91                 sequence                              
    ## 94                 severity                              
    ## 95                   sgComp                              
    ## 97           simpleForecast                              
    ## 98                smartData                              
    ## 99               spatialreg                              
    ## 100                     spc                              
    ## 101     stabilityMonitoring                              
    ## 102       svDataDescription                              
    ## 103                     svm                              
    ## 105                 testair                              
    ## 106           testairaction                              
    ## 107        testairactionDog              testairactionDog
    ## 108                 testlev                              
    ## 109                 testodt                              
    ## 110                textMgmt                              
    ## 111              textMining                              
    ## 112               textParse                              
    ## 113         textRuleDevelop                              
    ## 114        textRuleDiscover                              
    ## 115           textRuleScore                              
    ## 116       textSummarization                              
    ## 117               textTopic                              
    ## 118                textUtil                              
    ## 119                timeData                              
    ## 120             timeFilters                              
    ## 121           timeFrequency                              
    ## 122               transpose                              
    ## 123                  tsInfo                              
    ## 124                    tSne                              
    ## 125             tsReconcile                              
    ## 126                tstdalby                              
    ## 127               tutorial2                              
    ## 128           uniTimeSeries                              
    ## 129               varReduce                              
    ## 130   visualAnalyticActions                              
    ## 131               tkcsestst                 Session Tests
    ## 132                generact      No description available
    ## 133                  gctest                              
    ## 134                tkcaslua                              
    ## 135                testsscp  Actions for SSCP Computation
    ## 136               dmcastest Data Management Test Services
    ## 137                 clarify        Call Tool API routines
    ## 138                 csptest                        CASCLP
    ## 139                  cmpcas                              
    ## 140                 sandcas              SANDWICH library
    ## 141                  riskut                        RiskUT

Load the **simple** action set.

``` r
cas.builtins.loadActionSet(conn, 'simple')
```

    ## NOTE: Added action set 'simple'.

    ## NOTE: Information for action set 'simple':

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

    ## $actionset
    ## [1] "simple"

Get help on the simple action set using the usual builtins. Or refer to the documents on SAS Help Center. For example, here is the link to [the help of SAS Viya version 3.3](http://go.documentation.sas.com/?cdcId=pgmsascdc&cdcVersion=9.4_3.3&docsetId=allprodsactions&docsetTarget=actionsByName.htm&locale=en).

``` r
out <- cas.builtins.help(conn, actionset = 'simple')
```

    ## NOTE: Information for action set 'simple':

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

Details
-------

### CAS Session Options

out &lt;- cas.sessionProp.getSessOpt(conn, name = 'metrics')

``` r
out <- cas.sessionProp.getSessOpt(conn, name = 'metrics')
out
```

    ## $metrics
    ## [1] 0

Get the actual value of the metrics option by accessing that key of the list object.

``` r
out$metrics
```

    ## [1] 0

Setting the values of options using sessionProp.setSessOpt with keyword arguments for the option names.

``` r
cas.sessionProp.setSessOpt(conn, metrics = TRUE, collate = 'MVA')
```

    ## NOTE: Action 'sessionProp.setSessOpt' used (Total process time):

    ## NOTE:       real time               0.004341 seconds

    ## NOTE:       cpu time                0.010998 seconds (253.35%)

    ## NOTE:       total nodes             5 (200 cores)

    ## NOTE:       total memory            1.48T

    ## NOTE:       memory                  1.42M (0.00%)

    ## list()

Notice that the metrics option takes effect immediately. We now get performance metrics of the action that is printed to the output. Checking the value of collate, you see that it has been set to MVA.

``` r
out <- cas.sessionProp.getSessOpt(conn, name = 'collate')
```

    ## NOTE: Executing action 'sessionProp.getSessOpt'.

    ## NOTE: Action 'sessionProp.getSessOpt' used (Total process time):

    ## NOTE:       real time               0.002978 seconds

    ## NOTE:       cpu time                0.006000 seconds (201.48%)

    ## NOTE:       total nodes             5 (200 cores)

    ## NOTE:       total memory            1.48T

    ## NOTE:       memory                  1.15M (0.00%)

``` r
out$collate
```

    ## [1] "MVA"

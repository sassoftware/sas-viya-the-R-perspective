Chapter 10 - Advanced Topics
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
conn <- CAS('rdcgrdc.unx.sas.com', 39935)
```

    ## NOTE: Connecting to CAS and generating CAS action functions for loaded

    ##       action sets...

    ## NOTE: To generate the functions with signatures (for tab completion), set

    ##       options(cas.gen.function.sig=TRUE).

The Binary Interface
====================

``` r
binconn <- swat::CAS(cashost, casport, protocol='cas')
```

The REST Interface
==================

``` r
conn <- swat::CAS(cashost, casrestport, protocol='http')
```

Connecting to Existing Sessions
===============================

``` r
conn
```

    ## CAS(hostname=rdcgrdc.unx.sas.com, port=39935, username=sasyqi, session=8feeb993-171c-bf44-a71b-85ad1e2d7445, protocol=http)

``` r
currSession <- conn$session
currSession
```

    ## [1] "8feeb993-171c-bf44-a71b-85ad1e2d7445"

``` r
cas.session.listSessions(conn)
```

    ## $Session
    ##                           SessionName                                 UUID
    ## 1    Session:Thu Jul 26 22:53:10 2018 f765c2bc-3372-4b4b-9931-3f3a5ed492e6
    ## 2    Session:Thu Jul 26 23:32:38 2018 8d9885aa-8e17-e547-9295-2fb144862d70
    ## 3    Session:Thu Jul 26 23:43:00 2018 b776ddf0-4af6-3740-8da1-f299c6649a51
    ## 4    Session:Fri Jul 27 00:05:42 2018 607f5c45-3d0f-6144-bf97-b52d7b019b64
    ## 5    Session:Fri Jul 27 00:07:02 2018 8e61d18a-43f7-f449-8e1a-cb158478d65f
    ## 6    Session:Fri Jul 27 00:08:26 2018 7962c3e6-9120-8341-9919-f670d4bd1796
    ## 7    Session:Fri Jul 27 00:09:09 2018 3d4b93c5-888e-fc47-a3c7-1a834a15cdce
    ## 8    Session:Fri Jul 27 00:12:48 2018 896d5edd-e3cb-d44b-8ae5-61fcce7a470a
    ## 9    Session:Fri Jul 27 00:12:54 2018 c897bba8-3082-7248-9a35-7a3a1f4ec3b3
    ## 10   Session:Fri Jul 27 00:15:22 2018 79d5c977-eeb7-ef49-8a5a-346467aaf0f6
    ## 11   Session:Fri Jul 27 00:18:04 2018 15750174-25ea-7a4f-b43d-effb9da77a0f
    ## 12   Session:Fri Jul 27 00:34:15 2018 56d26fce-9385-1e41-9fa7-329e7bb35817
    ## 13   Session:Fri Jul 27 00:42:47 2018 52df5a04-06b2-0c4b-8c14-a61a60e650e7
    ## 14   Session:Fri Jul 27 01:12:26 2018 b1615072-669c-6a40-b4d7-8e0507521a7f
    ## 15   Session:Fri Jul 27 01:24:10 2018 ef21d5ac-9c7e-0c4c-b4f4-174730569efe
    ## 16   Session:Fri Jul 27 01:25:49 2018 dddd3106-88b1-b649-853e-69b840ae4007
    ## 17   Session:Fri Jul 27 01:26:58 2018 25608ac2-e85e-3243-a449-8d0d7d65483b
    ## 18   Session:Fri Jul 27 01:28:21 2018 8feeb993-171c-bf44-a71b-85ad1e2d7445
    ##        State   Authentication Userid
    ## 1  Connected Active Directory sasyqi
    ## 2  Connected Active Directory sasyqi
    ## 3  Connected Active Directory sasyqi
    ## 4  Connected Active Directory sasyqi
    ## 5  Connected Active Directory sasyqi
    ## 6  Connected Active Directory sasyqi
    ## 7  Connected Active Directory sasyqi
    ## 8  Connected Active Directory sasyqi
    ## 9  Connected Active Directory sasyqi
    ## 10 Connected Active Directory sasyqi
    ## 11 Connected Active Directory sasyqi
    ## 12 Connected Active Directory sasyqi
    ## 13 Connected Active Directory sasyqi
    ## 14 Connected Active Directory sasyqi
    ## 15 Connected Active Directory sasyqi
    ## 16 Connected Active Directory sasyqi
    ## 17 Connected Active Directory sasyqi
    ## 18 Connected Active Directory sasyqi

``` r
conn2 <- CAS(hostname = 'rdcgrd001.unx.sas.com', port = 39935,
  session = currSession)
```

    ## NOTE: Connecting to CAS and generating CAS action functions for loaded

    ##       action sets...

    ## NOTE: To generate the functions with signatures (for tab completion), set

    ##       options(cas.gen.function.sig=TRUE).

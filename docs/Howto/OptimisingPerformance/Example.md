<!-- Name: Howto/OptimisingPerformance/Example -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/09/17 22:24:59 -->
<!-- Author: demian -->
# Example Optimisation

## Server Environment


    Linux version 2.6.20-1.2320.fc5smp
    Dual-core Xeon 3060
    4GB DDR2 RAM

## Software


    Seagull 0.6.3 [3405]
    Apache 2.2.2
    PHP 5.1.6
    MySQL  5.0.27


## Tweaks and Results


    load testing
    ============
    
    target: http://example.com/seagull/branches/0.6-bugfix/www/index.php
    
    ab -n 100 -c 5 http://example.com/seagull/branches/0.6-bugfix/www/index.php
    
    
    Requests per second:    18.21
     - default
    
    
    Requests per second:    18.30
     - no navigation 
    
    Requests per second:    22.52
     - no navigation 
     - no blocks 
    
    Requests per second:    18.38
     - no navigation 
     - output buffering
    
    Requests per second:    18.34
     - no navigation 
     - output buffering
     - gzip compression
    
    Requests per second:    21.37
     - no navigation
     - no custom error handler
    
    Requests per second:    18.37
     - no navigation
     - no authorisation
    
    Requests per second:    18.70
     - no navigation
     - no URL strategies
    
    Requests per second:    17.81
     - no navigation
     - sessions in the DB
    
    Requests per second:    18.46
     - no navigation
     - multiple seq tables
    
    
    Requests per second:    23.05
     - no navigation
     - global caching
    
    Requests per second:    24.49
     - no navigation
     - global caching
     - lib caching
    
    Requests per second:    15.38
     - logs enabled
    
    Requests per second:    43.82
     - eaccelerator
    
    Requests per second:    72.78
     - eaccelerator
     - global caching
    
    Requests per second:    74.73
     - eaccelerator
     - global caching
     - lib caching
    
    Requests per second:    78.89
     - eaccelerator
     - global caching
     - lib caching
     - minimal filter chain:
    SGL_Task_Init,SGL_Task_SetupORM,SGL_Task_StripMagicQuotes,SGL_Task_DiscoverClientOs,
    SGL_Task_ResolveManager,SGL_Task_CreateSession,SGL_Task_SetupLangSupport,SGL_Task_SetupPerms,
    SGL_Task_AuthenticateRequest,SGL_Task_BuildHeaders,SGL_Task_BuildView,SGL_Task_SetupBlocks,
    SGL_Task_SetupGui,SGL_Task_BuildOutputData,SGL_MainProcess

## Notes

 * eaccelerator install: yum install php-eaccelerator  (default settings)


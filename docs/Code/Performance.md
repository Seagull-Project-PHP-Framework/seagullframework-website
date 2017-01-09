<!-- Name: Code/Performance -->
<!-- Version: 3 -->
<!-- Last-Modified: 2005/11/08 23:35:41 -->
<!-- Author: werner -->
# About Seagulls Performance
[[TOC]]

## Overview
On average hardware, ie a 1.4 Ghz AMD Linux desktop with 512 RAM and apc opcode caching extension enabled, Seagull currently serves around 64 requests/second.


    another note regarding benchmarking, it looks like i made a small error with ab, 
    and also had full debug logging enabled, new revised results of the same test 
    give twice the performance that what was previously reported:
    > 
    > hardware
    > ========
    > cpu: amd 1.4 Ghz
    > ram: 512MB
    > 
    > PHP
    > ===
    > 4.3.3
    > 
    > Requests per sec (with apc)
    > ==========================
    > 37.25
    
    new test results:
    
    Requests per second:    63.97 [#/sec] (mean)
    
    (with caching and apc enabled)
    
    cheers
    
    demian

## File Inclusion
Seagull out of the box comes in 'development mode', ie, it's not tweaked at all for performance, but rather for maximum feedback for developers for easy debugging. The various framework tasks and responsibilities have been split out into a task-per-file basis as much as possible to simplify maintenance.  As such, Seagull 0.3.11 includes 38 files per page execution by default.  If you have the luxury of using Zend as your IDE, run a profile page to see the list, if you don't, try the trial copy, it's quite interesting.

Whilst in production, where better performance is always key, the following steps can be taken to reduce the include tree:
  * enable caching
  * enable production mode for less verbose errors

For maximum performance, and to reduce includes to about 20 files, make these additional changes:
  * disable logging
  * disable custom error handler
  * go to admin's preferences and disable 'display page exec times'
  * in your master templates (seagull/www/themes/default/default/master*.html), copy in the included header/footer files

## Relative Version Performance Benchmarks


||*Seagull version* || *default files inc.* || *optimsed files inc.* || *default req. time* || *optimised req. time*||
|| 0.4.0dev3 || 41 || 31 || 680 || 390||
|| 0.4.0dev2 || 42 || 31 || 655 || 340||


  * test environment: Intel Centrino 1.6 Ghz, 512MB RAM, PHP 4.3.10, Apache 1.3.31
  * request time is for the index.php page in milliseconds
  * optimised means with caching enabled, no logging, custom error handler disabled
  * a bytecode cache would significantly improved request times
  * data gathered with Zend's profiler
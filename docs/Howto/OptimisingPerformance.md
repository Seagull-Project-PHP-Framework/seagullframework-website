<!-- Name: Howto/OptimisingPerformance -->
<!-- Version: 5 -->
<!-- Last-Modified: 2007/09/17 23:57:48 -->
<!-- Author: demian -->

# Performance Tuning
* TOC
{:toc}

## Introduction
There are a number of adjustments you can make to a Seagull install to optimise performance.

## Configuration Optimisations
In the <server-name>.conf.php file in seagull/etc:
  1. in [db][type] use the 'mysql' driver instead of 'mysql\_SGL', mysql\_SGL forces all sequences into a single table which can have a negative impact on a busy site
  1. as of 0.6.3 you can use the various SGL enhanced DB drivers, and still choose *not* to have all sequences in a single table - see the config option in the DB section
  1. as of 0.6.3 you can use the system default /tmp directory for storing the session files - this is recommended when you're not on a shared server mainly because linux will take care of removing old session files for you, if too many files accumulate your site will slow down
  1. in [site][outputBuffering], set this to 'true' - this way apache flushes all output to the browser at once which is more efficient
  1. set [cache][enabled] to 'true' - this is the single biggest performance improver
  1. as of Seagull version 0.6.1 you can set 'library cache' to true - this merges \~20 files used in every request into a single compressed file
  1. set [debug][customErrorHandler] to false, this is really just a developer aid and there shouldn't be any errors in your deployed sites
  1. ensure logging is disabled, a lot of data is written to the filesystem or DB depending on backend chosen.  Alternatively you can raise the error reporting level, eg to emergency, which results in fewer writes
  1. Don't use translated strings for the interface, this avoids loading the sometime sizeable language files

## Webserver Setup
The single biggest enhancement you can make to performance is to use an opcode cache for PHP, like e-accelerator or APC.  Expect requests/sec output to increase 5 to 6 times.

If you want to use Seagull in a professional setting or for a heavy traffic site, it is recommended you use one of following recommended open source and commercial products:

  * [eAccelerator][1]
  * [APC][2]
  * [Turck MMCache][3]
  * [ionCube PHP Accelerator][4]
  * [Zend Optimiser][5]

[[AddComment]]

[1]:	http://eaccelerator.net/
[2]:	http://apc.communityconnect.com/
[3]:	http://turck-mmcache.sourceforge.net/
[4]:	http://www.php-accelerator.co.uk/
[5]:	http://www.zend.com/store/products/zend-performance-suite.php
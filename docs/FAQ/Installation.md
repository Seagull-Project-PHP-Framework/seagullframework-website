<!-- Name: FAQ/Installation -->
<!-- Version: 9 -->
<!-- Last-Modified: 2006/05/24 11:57:18 -->
<!-- Author: werner -->
<!-- Status: Original -->

# Installation
* TOC
{:toc}

[[SubWiki(FAQ)]]

## Which package to choose?
When you go to the SF page, you have 3 things you can download:

  1) Seagull: API docs
  2) Seagull: PEAR installable (Only for PEAR installer)
  3) Seagull: whole package (tar.gz with all files, libraries etc...)

anybody NOT using the `pear command line` should install *item 3*.

## So which package should i choose?
Take the "whole package" and feel happy!

## How do i install Seagull?
Download choice #3 (whole package), unzip it, and run the installer located in seagull/www/index.php, from your web browser, add hot water...

## Does it work with PHP5?
Yes, it does. In doubt have a look at the /KnownIssues section.

## Does it work with IIS?
Yes. But it can cause problems with the front controller and search friendly urls. So change in your _conf.ini_ file:


	frontScriptName=index.php?

## Does it work with PHP as CGI?
See above "Does it work with IIS?"...

## I get error messages like 'cannot include file @include\_path@' etc...
The above error 
results from manually trying to install the .tgz from *item 2* (PEAR installable).  The directive:

	    include_path='.;@PEAR-DIR@'

is something ONLY the PEAR installer understands, what it's doing is rewriting 
constants.php and assigning PEAR's ENV php\_dir var to the include\_path.

## 404 errors when accessing modules
www.mysite.com/www/index.php/user/login gives me a 404 error on Apache 2


If you're running Apache 2, you need to have

	AcceptPathInfo on
set in the _/www_ directory.

It can go into _.htaccess_ files - see http://httpd.apache.org/docs-2.0/mod/core.html

If you don't have access to the apache configuration or the .htaccess file you can change the front controller script name in your installation's _conf.ini_ file:

	frontScriptName=index.php?

## what's the difference between mysql\_SGL and mysql drivers?
The mysql\_SGL driver puts all the sequence IDs in one table, the mysql driver creates 1 sequence table per table you add, this is the default behaviour of PEAR sequence emulation.  The latter choice is better for performance, but you end up with double the tables.

## How can i install an additional module?
Put all modules you want to install in the _/modules/_ directory and run this script in commandline:


	$ php etc/envRebuild.php

This rebuilds all your data from the SQL files in the modules.

Then you can add it in "Manage Modules" manually.

Soon a "point and click" version will come.
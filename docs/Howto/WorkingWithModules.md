<!-- Name: Howto/WorkingWithModules -->
<!-- Version: 9 -->
<!-- Last-Modified: 2008/11/26 16:29:09 -->
<!-- Author: demian -->

# Working With Modules
* TOC
{:toc}

in progress ..

## Overview
Modules are the main extension points of the framework, ie, developers create and modify modules to develop their application functionality using the resources provided by the framework.

## Installing Modules

## Uninstalling Modules

## A Modules Provides
 * a data access object
 * an Ajax provider to wrap requested data
 * business logic classes including controllers and observers
 * a set of database tables, with default, sample, custom and block data
 * aliases for the table names
 * ORM mappings
 * translations for the GUI data
 * configuration data
 * navigation data
 * html/xml templates for page output
 * web assets, ie CSS, javascript, images/media
 * unit and functional tests

## Linking in Web Assets
On Win32 platforms symlinking of files is not supported (until Vista apparently).  In the meanwhile Windows users can simulate symlink as follows:

 * *note:* You must be able to run "exec" in PHP

 1. Download http://www.dynawell.com/download/reskit/microsoft/win2000/linkd.zip
 1. Unzip, put linkd.exe in C:\Windows\System32 or C:\WINNT\System32
 1. Beware of dimwit Windows instructions, when /? help says LINKD Source Target, they really mean the reverse

With this adaptation the symlink() function in PHP works identically under Windows as it does on Linux/Mac.

Adapted from : http://fr.php.net/manual/fr/function.symlink.php#56654 

## Extending and Customising Module Registration

## Loading Data
A schematic for data deps in task loading:


	1) default data -> modules IDs, perms/roles
	2) sample data -> whatever u like, just to populate model for demo purposes
	3) custom data -> data specific for your app, in most cases all you need to extend svn base code
	4) navigation -> requires roles IDs, module IDs (created in 1)
	5) block data -> requires section/page IDs, roles IDs (created in above)

## Module initialisation
If a file named init.php is discovered in the root of your module, it will be loaded on every request whether the specific module is requested or not.

Eg, the CMS init file might define some constants that are required in Foo module, because it uses CMS functionality.  But if only the Foo module is requested, these constants will still be needed.  The global init loading takes this typical situation into account.

## Rebuilding
During the rebuild process it is possible to add custom tasks to the queue.  An example of where this might be useful is when you need to setup stored procedures, views and functions in your database.  You'd probably want to execute an SQL file directly as PEAR has problems with the delimeter syntax.  Try the following:

 1. create a custom task that will get read in as long as it appears in an init.php file located in the root of your custom module
 1. have the task invoke mysql directly with a shell\_exec(), passing in the path to your SQL file
 1. add the name of your task to 


	$conf['site']['customRebuildTasks']


## Tests
### Unit tests
As long as your test files in the 'tests' folder follow the set naming convention from example Seagull module, the test nodes will show up in the tree hierarchy of the Test Runner.

### Functional tests
You can also include your web tests in the same 'tests' folder, but to have them appear in the Selenium test runner (currently) you must reference them from seagull/tests/SeleniumTestSuite.html.
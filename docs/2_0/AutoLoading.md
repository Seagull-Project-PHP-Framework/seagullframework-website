---
layout: page
title: Autoloading Support
permalink: 2_0/AutoLoading
---

<!-- Name: 2_0/AutoLoading -->
<!-- Version: 3 -->
<!-- Last-Modified: 2010/03/23 18:53:33 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Autoloading Support
* TOC
{:toc}

## Overview
Thanks to PEAR/PHP naming standards, it's very easy to load classes just by naming them.  Seagull add some extra features:

 * no need for a SGL::load() type method
 * register as many vendor _namespaces_ per project as required 
 * libraries for a single vendor namespace can exist in multiple locations

And selectively takes away some

 * no support for PHP 5.3 Namespaces

## PHP 5.3 Namespaces
I believe it's too early to support PHP 5.3's Namespacing (capital N) feature.  Currently the feature appears to introduce more complexity without delivering any substantial benefit.  Also the bugginess introduced into basic class/object discovery outweighs any potential advantages.  See [http://bugs.php.net/51126][1]

## Vendor Namespace Support
In place of native Namespace support, vendor namespacing (small n) and use of decent (PEAR) naming conventions makes autoloading a very powerful tool where all framework resources become *addressable*.

## Setting up Autoloading and Namespaces
Since you'll want your classes autoloading as near as possible to the beginning of script execution, you can setup the autoloader either in your index.php front controller script, or near the top of the file you are setting up.  In the case of sgl2 the autoloader is setup as follows, in your project's index.php file.


	$root = dirname(dirname(dirname(__FILE__)));
	$sglLibDir = $root .'/sgl2/src/lib';
	
	require $sglLibDir.'/Uber.php';
	//  Initialise autoloader
	Uber::init();

In Seagull's case, we have several 3rd party libs in the libs folder, but the autoloader can be used for any directory you specify, ie vendor, etc.  In the following code we register namespaces for each of the main vendors used.


	Uber_Loader::registerNamespace('Uber', $sglLibDir);
	Uber_Loader::registerNamespace('SGL2', $sglLibDir);
	Uber_Loader::registerNamespace('HTML', $sglLibDir);
	Uber_Loader::registerNamespace('Zend', $sglLibDir);
	Uber_Loader::registerNamespace('Horde', $sglLibDir);

That means any of the following calls will load the class automatically:


	$a = new Uber_Event_Dispatcher();
	$b = new SGL2_Router();
	$c = new HTML_Template_Flexy();
	$d = Zend_Registry::isRegistered($key)

## Multiple Search Paths for Vendor Classes
Quite often you need classes stored in a app-wide libs or vendor directories, but you need additional classes from the same vendor stored in a module.

The Seagull autoloader handles this easily.  Let's say you have some app-wide classes here:


	/path/to/sgl2/src/lib/SGL2

and some module-specific classes here


	/path/to/my/app/modules/cms/lib/SGL2

The Seagull autoloader will automatically load classes from each if you setup your environment as follows:


	//  sgl libs + cms libs
	$sglPath = dirname(dirname(__FILE__));
	$sglLibDir = $sglPath .'/lib';
	$cmsLibDir = $sglPath .'/modulesSCMS/cms/lib';
	// look for SGL2 libs in this list of dirs
	Uber_Loader::registerNamespace('SGL2', array($sglLibDir, $cmsLibDir));

## Also In This Series

 - [Overview][2]
 - [Events and Plugins][3]
 - [Autoloading][4]
 - [API Docs][5]
 - [Unit Tests][6]





[1]:	http://bugs.php.net/51126
[2]:	/2_0/Overview.html
[3]:	/2_0/EventsAndPlugins.html
[4]:	/2_0/AutoLoading.html
[5]:	/2_0/ApiDocs.html
[6]:	/2_0/UnitTests.html
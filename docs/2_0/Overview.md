---
layout: page
title: Overview Seagull 2.0
permalink: /2_0/Overview.html
---

<!-- Name: 2_0/Overview -->
<!-- Version: 16 -->
<!-- Last-Modified: 2010/03/23 19:17:53 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Overview Seagull 2.0
* TOC
{:toc}

## Intro
Here you will all the info you need to get up to speed with the PHP5 only version of Seagull.  The main points are:

 * code location: [http://github.com/demianturner/sgl2][1]
 * current version: 2.0.0
 * requires \>= PHP 5.2.6
 * no compatibility with 0.6-bugfix branch
 * 0.6-bugfix will continue to be maintained separately

## What's New?

 * clarified distinction between tasks and filters
 * managers have been renamed to controllers
 * master templates have been renamed to layouts
 * actions changed from `_cmd_actionName()` to `_doAction()`
 * exclusive use of exceptions, try/catch, no PEAR errors
   * all php errors captured and thrown as exceptions
 * much easier to extend and override all classes in the framework
 * cleaner request handling
   * distinction between tainted/clean data
   * `Zend_Filter_Input` used for powerful filtering and validation of data
 * class member visibility identifiers (private/protected/public) enforced
 * type hinting used throughout
 * we've moved to [PHPunit][2] for testing
 * routing now standard, Horde\_Routes used and encapsulated in `SGL2_Router`
 * CSS/Javscript - Yaml and jQuery are the currently preferred frameworks but of course you can use whatever you want
 * The [SGL2 autoloader][3] includes all files automatically, no need to require files if you follow PEAR naming conventions
   * use-at-will architecture, only named classes are loaded
   * environment (autoload, include paths)
 * PEAR no longer bundled
 * Coding Standards enhancements
   * includes, if needed, must go at top of files
   * no more conditional includes
   * reducing number of files is not a top concern as opcode cache usage is presumed
 * single installation of Seagull framework for all your projects

## What's Planned

 * PEAR packaging and hosting of all components, ie "pear upgrade seagull/core" instead of "svn up"
 * continuous integration environment with PHPunit and friends
 * 100% code coverage (well as close as possible)
 * improved component quality using latest QA tools [http://qualityassuranceinphpprojects.com/][4]pages/tools.html

## What's Completed

 * event hooks and plugin architecture (see [EventsAndPlugins][5])
 * new `SGL2_Router` component that wraps Horde\_Routes, supply your own driver if required
 * using `Zend_Config` for all config handling
 * all components are addressable through auto-loading
 * clean separation between vendor framework code and generated projects
 * namespacing for modules, controllers and actions/commands

## What you Need

 * see [http://github.com/demianturner/sgl2/blob/master/README.markdown][6]

## New Conventions

### Resources stored in Registry

All framework resources are now stored in a registry (`SGL_Registry`), get them from the registry rather than using singletons

	$registry = SGL_Registry2::getInstance();
	$req = $registry->get('request');
	$cache = $registry->get('cache');
	$db = $registry->get('db');
	// ... etc

### Input/Output objects replaced by Request/Response
The parameters to all controller action methods were `SGL_Registry` and `SGL_Output`, respectively, in 0.6-bugfix, referred to by the variables $input and $output.  The same variables are used in \>= 2.0 but now refer to the `SGL2_Request` and `SGL2_Response` objects, and are only used for storing request and response data.

## Core Project Values

 * less is more
 * avoiding code bloat: all additional features/enhancements are installed on a plugin basis, core is kept clean and minimal
 * no wheel reinvention tolerated: usage of existing high quality libs that conform with PEAR/PHP coding standards
 * doing things the 'PHP way' preferred rather than over-engineering code
 * performance is key: Seagull continues position itself as a problem solver for high traffic environments
 * no dumbing down for PHP newbs, not worth the tradeoff in code bloat

## Also In This Series

 - [Overview][7]
 - [Events and Plugins][8]
 - [Autoloading][9]
 - [API Docs][10]
 - [Unit Tests][11]

[1]:	http://github.com/demianturner/sgl2
[2]:	/2_0/UnitTests.html
[3]:	/2_0/AutoLoading.html
[4]:	http://qualityassuranceinphpprojects.com
[5]:	/2_0/EventsAndPlugins.html
[6]:	http://github.com/demianturner/sgl2/blob/master/README.markdown
[7]:	/2_0/Overview.html
[8]:	/2_0/EventsAndPlugins.html
[9]:	/2_0/AutoLoading.html
[10]:	/2_0/ApiDocs.html
[11]:	/2_0/UnitTests.html
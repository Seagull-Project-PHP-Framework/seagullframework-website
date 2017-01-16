---
layout: page
title: Internationalisation Overview
permalink: /Howto/Internationalisation/General.html
---

<!-- Name: Howto/Internationalisation/General -->
<!-- Version: 8 -->
<!-- Last-Modified: 2009/03/04 10:52:36 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Internationalisation Overview
* TOC
{:toc}

 - [Start][1]
 - [General overview][2]
 - [How to setup the environment][3]
 - [Best practices][4]
 - [How to convert your Seagull site to utf-8][5]
 - [RTL support][6]
 - [Submitting translations][7]
## Intro
With the new translation module in 0.6.3 developers have a range of tools at their disposal for many aspects of internationalising a website/application.  This module focusing on translating interface elements, not content.  News articles would be considered content and their translation is in the domain of CMS tools.

## Features
The main features are;
 * ability to select languages and character set encodings to be used for translation (utf-8 recommended for multilingual sites)
 * a translation admin interface showing master key phrases with inputs for translations
 * ability to create a dedicated translator role, exposing only translation screens
 * ability to group translations into categories
 * ability to add comments to each phrase to help translators contextualise strings
 * translation strings can accept multiple positionable variables
 * a stats interface indicating percentage completion of all translations, broken down by module and with grand totals
 * modifications in master file (adding/removing keys) are propagated to translated (slave) files
 * file locking so multiple translators cannot edit the same file simultaneously
 * versioning of translation files

## Save translation files as part of source code control
When you are version controlling a project, and perhaps have a translation team continually updating the translated copy for the site, it's quite handy to have an automated commit in the language directories of your modules.  In a recent project with 8 translators working on 2 modules I set commits to execute every 30 minutes - this gave us good incremental control over the internationalisation process.  Translation work was done on a staging version of the site.

## Customise the language set your app uses
Create an init.php file in the root of your project's main module, this will be loaded on every request, even if your module is not specified in the request.  In the init.php file you can customise the app's language list with code similar to the following:

	<?php
	$GLOBALS['_SGL']['LANGUAGE'] = array(
	    'af-utf-8'      => array('af|afrikaans', 'afrikaans-utf-8', 'af'),
	    'ar-utf-8'      => array('ar|arabic', 'arabic-utf-8', 'ar'),
	    'en-utf-8'      => array('en([-_][[:alpha:]]{2})?|english', 'english-utf-8','en'),
	    'de-utf-8'      => array('de([-_][[:alpha:]]{2})?|german',  'german-utf-8', 'de'),
	    'es-utf-8'      => array('es([-_][[:alpha:]]{2})?|spanish', 'spanish-utf-8','es'),
	    'fr-utf-8'      => array('fr([-_][[:alpha:]]{2})?|french',  'french-utf-8', 'fr'),
	    );
	?>

## Show percent complete translation stats for only your app's modules
When you use the admin interface to edit the config settings for the Translation module, note the "onlyModules" element.  If you don't want the "percent complete" stats to report on, eg, admin modules, just provide a comma separated list of modules you want included in the results.

## Start translating new languages while only showing existing ones
We've also built functionality into the Translation module so the front end of your site will only list the completed translations, however translators can work on new ones in the back end. To enable new backend translation languages, simply list then against the key 'extraLanguages', ie


	                  ;  key      | name    | file name
	                  ;  ----------------------------------
	extraLanguages   =; "hi-utf-8 : hindi   : hindi-utf-8,
	                  ;  ru-utf-8 : russian : russian-utf-8"

## Master file is not editable by web interface
This strategy works well because often in team environments multiple users will edit the master file which can result in undesired modifications in translated (slave) files.  For best results assign one reliable member of your development team the responsibility for maintaining the master file.

## Adding comments and categories
The following pseudo array elements are converted to category headers and comments in the web interface:


	$defaultWords['__SGL_CATEGORY_category_name'] = 'This is my category name';
	$defaultWords['aMonths']['01'] = 'January';
	$defaultWords['aMonths']['02'] = 'February';
	$defaultWords['__SGL_COMMENT_example'] = 'here is a comment example';

## Translation changes tracked


	$words['__SGL_UPDATED_BY'] = 'foo';
	$words['__SGL_UPDATED_BY_ID'] = '10';
	$words['__SGL_LAST_UPDATED'] = '2007-08-24 10:03:55';

## Converting translate method calls to lowercase
It's much easier if all your master English keys are in lowercase.  If you have access to the TextMate editor there are only 2 steps to do this:

 1. convert your language file keys to lowercase by vertically selecting the array keys (use option key with the Mac)
 1. run a regex over your template files, again in TextMate


	Search : {translate\(\#(.\*)\#\)}

	Replace : {translate\(\\#\L\1\E\\#\)}

[1]:	/Howto/Internationalisation.html
[2]:	/Howto/Internationalisation/General.html
[3]:	/Howto/Internationalisation/TechSetup.html
[4]:	/Howto/Internationalisation/TranslationBestPractices.html
[5]:	/Howto/Internationalisation/ConvertingSeagullSitesToUtf8.html
[6]:	/Howto/Internationalisation/HebrewAndRtlLanguages.html
[7]:	/Howto/Internationalisation/SubmittingTranslations.html
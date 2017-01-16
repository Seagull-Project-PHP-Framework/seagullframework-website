---
layout: page
title: Internationalisation - Tech Setup
permalink: /Howto/Internationalisation/TechSetup.html
---

<!-- Name: Howto/Internationalisation/TechSetup -->
<!-- Version: 3 -->
<!-- Last-Modified: 2009/03/04 10:52:55 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Internationalisation - Tech Setup
* TOC
{:toc}

 - [Start][1]
 - [General overview][2]
 - [How to setup the environment][3]
 - [Best practices][4]
 - [How to convert your Seagull site to utf-8][5]
 - [RTL support][6]
 - [Submitting translations][7]

## Overview
 1. it's easiest to translate a Seagull installation if you setup a copy of your live environment on a different host, ie translate.mydomain.com
 1. the translations will eventually be committed to your source code repo, but you only activate the new languages when they're ready
 1. create an init.php file for your main customised module, let's say it's called Foo.

	sgl/modules/foo/init.php
 1. in the init.php file create a task for adding new translators

	// define a new role for translators
	define('FOO\_ROLE\_TRANSLATOR',    3);
	class SGL\_Task\_CreateTranslators extends SGL\_Task
	{
		public function run($data)
		{
		    SGL::logMessage(null, PEAR_LOG_DEBUG);
		
		    $aData[] = array(
		        'username' => 'hugo',
		        'first_name' => 'Hugo',
		        'last_name' => 'french',
		        'passwd' => 'my_passwd',
		        );
		
		    // add more accounts as array elements
		
		    $isActive  = true;
		    $createdBy = SGL_ADMIN;
		    $now       = SGL_Date::getTime(true);
		
		    // process users
		    foreach ($aData as $aUser) {
		        $aUser['email']  = $aUser['username'];
		        $aUser['passwd'] = md5($aUser['passwd']);
		
		        // add user
		        $userId = User2DAO::singleton()->addUser($aUser, $isActive,
		            FOO_ROLE_TRANSLATOR, $createdBy);
		        // add prefs
		        if (!PEAR::isError($userId)) {
		            User2DAO::singleton()->addMasterPrefsByUserId($userId);
		        }
		    }
		}
	}
 1. edit the config for the translation module and add languages you intend to translate.  The 'onlyModules' key means only these modules will show up in your statistics screen which indicates what percentage of the translation is complete.

		onlyModules="foo,user2"
		otherDictionaries=
		extraLanguages="sv-utf-8 : swedish : swedish-utf-8,
		zh-utf-8 : chinese simplified : chinese\_simplified-utf-8,
		zhtw-utf-8 : chinese traditional : chinese\_traditional-utf-8,
		de-utf-8 : german : german-utf-8,
		es-utf-8 : spanish : spanish-utf-8,
		fr-utf-8: french : french-utf-8"

 1. ensure "Mark words which were not translated" is set to YES
 1. add the default data that corresponds to your new role, and its perms
 1. rebuild Seagull to activate the new roles, perms and translator users
 1. in the global filter chain add the SGL\_Task\_SetupExtraLanguages task after SGL\_Task\_CreateSession, this allows your translators to see their language translations in progress, even if their languages are not enabled in the global config
 1. if your site uses Ajax, don't forget to add the same task to your Ajax filter chain, also after SGL\_Task\_CreateSession
 1. create a cronjob that updates your languages files every 1/2 hour or so, eg:

		cd /var/www/html/my\_site/translate/trunk;
		modules=ls modules;
		for module in $modules;
		do
		 svn up modules/$module/lang/;
		done;
		updating js language files
		svn up www/foo/js/Localisation;

 1. create a cronjob that commits the language files and fixes the permissions, also every 1/2 hour or so

		!/bin/bash
		WEBROOT=/var/www/html/my\_site/translate/trunk
		PHP=/usr/bin/php
		cd $WEBROOT;
		chmod 777 www/foo/js/Localisation/\*.js;
		syncing js translation files
		$PHP $WEBROOT/www/index.php \
		--moduleName=translation --managerName=jstranslation \
		--action=createFiles --targetModule=foo --modulesToScan= foo,user2
		listing files to be committed
		moduleLangFiles='modules';
		jslangFiles='www/foo/js/Localisation';
		committing
		svn ci -m "automated commit for translation - module lang files" $moduleLangFiles;
		svn ci -m "automated commit for translation - js lang files" $jslangFiles;
		setting correct permissions
		modules="foo user2";
		for module in $modules;
		do
		chmod 777 modules/$module/lang/*.php;
		chmod 777 modules/$module/lang/email/*.php;
		done;


[1]:	/Howto/Internationalisation.html
[2]:	/Howto/Internationalisation/General.html
[3]:	/Howto/Internationalisation/TechSetup.html
[4]:	/Howto/Internationalisation/TranslationBestPractices.html
[5]:	/Howto/Internationalisation/ConvertingSeagullSitesToUtf8.html
[6]:	/Howto/Internationalisation/HebrewAndRtlLanguages.html
[7]:	/Howto/Internationalisation/SubmittingTranslations.html
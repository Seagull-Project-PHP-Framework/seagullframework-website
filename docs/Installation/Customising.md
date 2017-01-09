---
layout: page
title: Customising the Installer
permalink: /Installation/Customising.html
---

<!-- Name: Installation/Customising -->
<!-- Version: 3 -->
<!-- Last-Modified: 2007/04/20 16:36:34 -->
<!-- Author: demian -->
# Customising the Installer
* TOC
{:toc}

## Overview
Once you've developed your site in Seagull, built your custom modules and created your sample data, you are going to want to customise the installation process to your customer's needs.

You can do this renaming the file [gitlink:/branches/0.6-bugfix/etc/customInstallDefaults.ini.dist seagull/etc/customInstallDefaults.ini.dist] to seagull/etc/customInstallDefaults.ini and modifying the sample settings in it.

## Customising config settings
As of Seagull 0.6.2 you can customise any config setting from the global config file, not just params from the installer.  Just add the ini settings as required to seagull/etc/customInstallDefaults.ini, ie

	[group]
	key = value

## Advanced example
In the example below many customisation techniques are used:

	; Add your customised settings below, and rename this file to customInstallDefaults.ini.
	; Then when you run the Seagull installer, these values will be used.
	
	adminFirstName   = Demian
	adminLastName    = Turner
	siteName         = Foo
	adminEmail       = demian@phpkitchen.com
	siteKeywords     =
	siteDesc         =
	siteCookie       = FOOSESSID
	prefix           =
	name             = foo ; this is database name
	aModuleList      = block,cms,export,default,media,user
	
	[site]
	defaultTheme = babyfy
	showLogo = logo.gif
	inputUrlHandlers = Classic,Sef
	
	[path]
	moduleDirOverride = modulesFOO
	pathToCustomConfigFile = ../modulesFOO/constants.php
	uploadDirOverride = /www/themes/foo/images/uploads

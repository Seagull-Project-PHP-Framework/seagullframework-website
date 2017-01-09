---
layout: page
title: Using The Pear Package Manager
permalink: /Installation/UsingThePearPackageManager.html
---

<!-- Name: Installation/UsingThePearPackageManager -->
<!-- Version: 18 -->
<!-- Last-Modified: 2007/06/11 22:41:03 -->
<!-- Author: demian -->
# Using The Pear Package Manager
* TOC
{:toc}

This method requires basic familiarity with the [PEAR package manager][1] and is the easiest/fastest way to get up and running with Seagull.  

## For Seagull \>= 0.5.5
There are a few requirements:

  * You must be running a recent version of PHP 4.3.11 or greater with the base PEAR packages installed
  * You must use PEAR version 1.4.6 or greater (pear list to check version) after having set your preferred state to devel

		$ pear config-set preferred_state devel
		$ pear upgrade-all

  * You will be getting the code from the Seagull Channel Server

		$ pear channel-discover pear.phpkitchen.com

  * This version uses pearified.com's `Role_Web` to denote the system www directory (to view your current settings use `$ pear config-show`).  Once installed and the post-install script is run, you will have a pear variable 'web\_dir' which is set to your apache web root.

		$ pear channel-discover pearified.com
		$ pear install pearified/Role_Web
		$ pear run-scripts pearified/Role_Web

  * enter the path to your web server's htdocs (doc\_root) when prompted
  * once this is successful simply to the following:

		$ pear install phpkitchen/seagull

What you get by default is the core Seagull framework and the 3 minimum required modules, (currently) Default, Navigation and User.  Keep in mind that your directory structure will be different than had you downloaded the code from SF or svn.  The PEAR "way of doing things" is to put your code as follows

	/path/to/system/pear/Seagull/lib
	/path/to/system/pear/Seagull/modules
	/path/to/system/pear/data/Seagull/etc
	/path/to/system/pear/data/Seagull/var
	/path/to/system/pear/docs/Seagull/docs
	/path/to/pear/web_dir/Seagull/www

So if you need to delete the `INSTALL_COMPLETE.php` file, eg, you can find it typically in

	/usr/local/lib/php/data/Seagull/var/INSTALL_COMPLETE.php

*One word of warning:* if you have installed and uninstalled the Seagull PEAR package various times and find things are missing, run


	$ pear clear-cache

and try a fresh install.

Once you have performed the installation with the PEAR package manager:
  * future updates are even easier:

		$ pear upgrade phpkitchen/Seagull

  * don't forget to revert your PEAR configuration settings to their original state if you use the PEAR package manager for managing other repos
  * use the [Web Installer][2] to finish the installation, point your browser at !http://<htdocs>/Seagull/www/
  * give the webserver write perms to the var directory, ie /path/to/your/pear/install/data/Seagull/var
  * have fun using seagull ;)

> note: if you want to install PEAR-packages to your `<seagull>/lib/pear/` directory, have a look at [FAQ/Modifying][3]

### Uninstall

	pear uninstall phpkitchen/seagull_user
	pear uninstall phpkitchen/seagull_navigation
	pear uninstall phpkitchen/seagull_default
	pear uninstall phpkitchen/seagull
	pear uninstall pearified/role_web

## For Seagull \<= 0.5.4
There are a few requirements:

  * You must be running a recent version of PHP 4.3.11 or greater with the base PEAR packages installed
  * You must use PEAR version 1.4.5 or great (pear list to check version)
  * You must set the pear data\_dir to your webroot or point it to anywhere on your filesystem and subsequently create a Virtual Host to expose the www directory (to view your current settings use `$ pear config-show`)

  (this is done with the switch `-d data_dir=/path/to/data/dir`)
 
  * Your preferred package state must be set to 'alpha'.  The current state of the Seagull project is stable but there is a dependency on the Validate lib which has been 'alpha' for ages now.

  (this is done with the switch `-d preferred_state=alpha`)

So to install Seagull, the following commandline sequence is necessary (on one line):

	$ pear -d data_dir=/path/to/web/root  -d preferred_state=alpha install --onlyreqdeps http://osdn.dl.sourceforge.net/sourceforge/seagull/seagull-0.4.3.tgz

Once you have performed the installation with the PEAR package manager:
  * don't forget to revert your PEAR configuration settings to their original state if you use the PEAR package manager for managing other repos
  * use the [Web Installer][4] to finish the installation, point your browser at !http://<htdocs>/seagull/www/
  * give the webserver write perms to the var directory, ie `/path/to/your/htdocs/seagull/var`
  * have fun using seagull ;)

## Reinstalling PEAR from scratch
### Windows
In windows with any recent install of php (hopefully you used the zip file from php.net) you should find a file call go-pear.bat in the root of your php install.   Double-click this and follow the instructions in the wizard.

### Linux
In Linux things are naturally even easier, check out this page: http://go-pear.org/ and run the following code from the commandline:

	lynx -source http://pear.php.net/go-pear | php -q

> nb: this wizard recommends you install pear in /usr/local/share/php but installing php from src recommends /usr/local/lib/php.  To ensure consistency later on down the road I recommend you use the latter path._ 
### Post Install
After installing either version you should have the 'pear' command at your disposal.  Type 'pear' to verify, you should get a long list of the pear commands as a result.

The first thing you need to do after an install is test your libs are up to date:

	$ pear upgrade-all

## Problems: Troubleshooting your PEAR Install
It seems to happen quite often that a PEAR installation will just stop working, unfortunately that is just the name of the game as a lot of the code, in particular the installer code, is in heavy development.  There are a few scenarios where I can guarantee your PEAR install will not work properly:
 * you install it with WAMP
 * you installed it with YAST
 * you installed it as RPMs or with yum

In these cases best thing is to blow away the PEAR repo entirely, you can find it here on windows:

	c:\php\PEAR

or on linux either:

	/usr/local/lib/php

	/usr/local/share/php

Just delete the whole thing, and also it is *very* import you delete the following two files as well: `pear.conf` and `.pearrc`

## Related Info
 * [prepare Seagull as a PEAR package][5]
 * [Setting up your own PEAR channel][6]

## Setting up a PEAR channel
 * remember to register the channel with the channel itself, ie pear.phpkitchen.com must be registered at the pear.phpkitchen.com channel
 * also remember to register custom roles with the channel, ie Role\_Web

[1]:	http://pear.php.net/manual/en/installation.cli.php
[2]:	/Installation.html
[3]:	/FAQ/Modifying.html
[4]:	/Installation.html
[5]:	/Internal/PrepareSeagullAsPearPackage.html
[6]:	http://greg.chiaraquartet.net/archives/123-Setting-up-your-own-PEAR-channel-the-official-way.html
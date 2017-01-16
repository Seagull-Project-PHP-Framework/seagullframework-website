---
layout: page
title: Installing Seagull
permalink: /Installation.html
---

<!-- Name: Installation -->
<!-- Version: 26 -->
<!-- Last-Modified: 2007/10/06 21:46:15 -->
<!-- Author: osde8info -->
<!-- Status: In Progress -->

# Installing Seagull

* TOC
{:toc}

## Downloading Seagull
[Download][1]

## Getting Started
	For the 0.6.0 release there is a memory requirement if your PHP installation was 
	compiled with --enable-memory-limit.    In php.ini you need to set memory_limit 
	to at least 16M.

After you have downloaded the source, unzip it to your Web document root and rename the resulting directory to `seagull`. Now, point your Web browser at the [web-doc-root]/seagull/www directory, which should be a URI similar to the following:

	http://localhost/seagull/www

Another approach is to [setup a Virtual Host][2], this provides a much more secure environment and is recommended for live installs on the web.

## Using the Installer
The installation process breaks down into the following steps:

 1. The installer will first prompt you to comply with the end user license agreement, please read carefully and confirm your agreement by selecting the 'I accept' radio button and choosing next.
 1. The next step determines if your environment is setup correctly for Seagull to run.  On a Unix system you will get, as a minimum, a message prompting you to give the webserver write permissions to the [web-doc-root]/seagull/var directory.  Do this by issuing the following at the command line (Windows users with write perms on the web document root will not get this message):

	> $ chmod 777 [web-doc-root]/seagull/var

 1. Enter database details: enter your DB details as prompted, selecting MySQL, PostgreSQL, etc. appropriately, and supplying host, username, password and port details accordingly.  If you are on a local machine and running MySQL as root with no password, you do not need to enter any details.  If the database user you specify has database create permissions, Seagull will create the database for you.  If not, you must create the empty database before you initialise it, and select 'use existing db'. [FIXME: one word on unix vs tcp and ports...]
 1. On the final screen, choose the admin username and password, and customise the install paths as necessary.  You can also set general information for your install such as site description, keywords, language and server time offset.  Also please set the install password, this should be different from your admin password but it's up to you.  You can use the install password to re-setup an existing Seagull installation.
 1. Next the installer creates the schema and loads the default data which will take anywhere from 1 second to 30 depending on the speed of your machine.
 1. Once you have received messages about the schema being created and default data loaded, hit the 'launch Seagull' link.

NOTE: If www.example.com/index.php/module/manager/action mapping isn't properly working with Apache 2.x try adding "AcceptPathInfo On" to your Apache Config.

## apache 2.0 enable modrewrite (on Ubuntu 6.06)

	# a2enmod rewrite
	# /etc/init.d/apache2 restart

## seagull (apache.conf)

	<Directory /var/www/seagull/www/>
	        Options FollowSymLinks
	        AllowOverride All
	</Directory>
	
	<VirtualHost *>
	        ServerName sea.gull
	        DocumentRoot    /var/www/seagull/www
	        DirectoryIndex  index.php
	        AcceptPathInfo On
	</VirtualHost>

## Reinstalling
If you want to run the installer again, once Seagull has been setup, just call setup.php instead of the index.php front controller file, eg:

	http://localhost/seagull/www/setup.php

You will be prompted for a password which is the one you specified during install.

## If you're using PHP installed in CGI mode
On some Plesk installs it is possible to run SGL only if _CGI support_ option is checked. See an image below.
![][image-1]

## See also
 * there is a [Flash Movie][3] about the installation process.
 * the [gitlink:/trunk/INSTALL.txt install file]() included in the distribution

[1]:	/Installation/Download.html
[2]:	/Installation/SettingUpApacheVirtualHosts.html
[3]:	/files/flashDemos/installingSeagull.htm


[image-1]:	/images/Installation/plesk_config.jpg
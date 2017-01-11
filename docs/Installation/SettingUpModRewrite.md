---
layout: page
title: Setting up mod\_rewrite
permalink: /Installation/SettingUpModRewrite.html
---

<!-- Name: Installation/SettingUpModRewrite -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/01/02 20:38:54 -->
<!-- Author: demian -->

# Setting up mod\_rewrite
* TOC
{:toc}

Although many servers are setup with this module enabled by default, there are always exceptions.

## Is the module loaded?
In some cases, the module will be compiled into the the httpd binary, if so you can check for mod\_rewrite's presence with


	httpd -l

If it doesn't show up in the list, it may be loaded as a Dynamic Shared Object, in this case you should be able to find it in the output of 



	<?php phpinfo(); ?>

If you're compiling Apache yourself, you simply need to add the following to your configure line


	./configure --enable-module=rewrite

If you're using a distribution's packaged version of Apache, eg, Fedora Core, you have to install the related devel tools


	yum install apache2-devel

These effectively make the mod\_rewrite module available.

## Now mod\_rewrite is loaded, but still it doesn't work
This is very typical, it just means your Apache configuration by default doesn't allow the use of custom .htaccess files.  You will need to change 

	AllowOverride None

to

	AllowOverride All


To get around this you need to edit your httpd.conf file, and search for *AllowOverride*.

The first instance you will find applies to the root directory, it's *not* the one you want:


	<Directory />

The second one applies to the webroot, you can modify this but you may not want to allow mod\_rewrite on all your sites.  Ideally you need to create a directory stanza just for your particular site, 


	<Directory /path/to/your/site>
	    Options FollowSymLinks
	    AllowOverride All
	</Directory>

Once you've saved your changes, restart Apache for them to take effect.
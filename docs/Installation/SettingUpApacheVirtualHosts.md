---
layout: page
title: How to setup a Virtual Host in Apache
permalink: /Installation/SettingUpApacheVirtualHosts.html
---

<!-- Name: Installation/SettingUpApacheVirtualHosts -->
<!-- Version: 2 -->
<!-- Last-Modified: 2007/01/09 16:51:12 -->
<!-- Author: demian -->

# How to setup a Virtual Host in Apache
If you haven't done this before it's easier than you think, in fact only 3 steps are required.

* TOC
{:toc}

## Locate the httpd.conf file
This is usually found in

	/etc/httpd/conf/httpd.conf

Open it up in your preferred editor, and search for 'Virtual Hosts', this should take you to a section near the end of the file that looks like this:


	### Section 3: Virtual Hosts

## Enable name-based virtual hosts
This is as simple as adding or uncommenting the line


	NameVirtualHost *:80

## Setup the first catch-all host
Request to your server will use these details by default if they can't match any configured setting, eg, if you request foo.your-domain.com and foo hasn't been setup.

The first virtual host stanza looks like this, following with our example of a local setup:


	<VirtualHost *:80>
	ServerName      localhost
	DocumentRoot    /var/www/html
	</VirtualHost>

If your local machine's hostname is 'foobar', replace localhost with 'foobar'.

## Setup your virtual host
Not very different from above, with one exception: 


	<VirtualHost *:80>
	ServerName my_host
	DocumentRoot    /var/www/html/my_site
	DirectoryIndex  index.php
	</VirtualHost>

The exception being that whatever name you give your virtual host, this name must also appear in your hosts file.  Open it now:


	$ vi /etc/hosts

For our above example, on a local machine, your file should look like this::


	127.0.0.1       localhost.localdomain   localhost
	127.0.0.1       my_host

Note that the DocumentRoot directive points to the webroot of the website you want to serve.  In the case of Seagull it will be something like:


	/var/www/html/seagull/www

You can of course add many other directives in the virtual host container, for more info see the list of [available directives][1] or some [examples][2].

## Gotchas
For starters, don't forget to restart your webserver so the new configuration in httpd.conf can be read in.  Also ensure all files can be read by the webserver, occasionally you can have a situation where apache can't find the files you've specified, or read them, and it will fail to restart.


[1]:	http://httpd.apache.org/docs/2.0/mod/directives.html
[2]:	http://httpd.apache.org/docs/2.0/vhosts/examples.html
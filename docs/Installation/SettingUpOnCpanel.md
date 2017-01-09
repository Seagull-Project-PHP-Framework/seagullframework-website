---
layout: page
title: Setting up on a host running Cpanel
permalink: /Installation/SettingUpOnCpanel.html
---

<!-- Name: Installation/SettingUpOnCpanel -->
<!-- Version: 2 -->
<!-- Last-Modified: 2007/01/02 15:37:03 -->
<!-- Author: demian -->
# Setting up on a host running Cpanel
* TOC
{:toc}

## How to Install
 * select either your customised version of Seagull or a standard distribution
 * if you're uploading a project you've customised, remove unnecessary files which include:
   * everything in the seagull/var directory
   * the top level docs and test dirs, as well as www/smarty and www/savant if you're not using those template engines
   * remove modules you won't use, ie seagull/modules/$module
   * zip everything up - with 4 custom modules, data and a custom theme and libs I had 3.2 MB archive
 * use Cpanel's upload feature, in its File Manager - for me it took \< 1min on 4mb connection
 * select the zipped file in File Manager and choose 'extract file contents'
 * go to http://yoursite.com/www and run setup wizard
 * download AUTH.txt file to your desktop and re-upload it to your server in the root of seagull dir
 * you will then be asked to make Seagull's var dir writable, which you can do via the File Manager
 * use Cpanel's control panel to create a database, and a db user and password, then supply these to Seagull wizard, ticking the appropriate boxes to indicate the database already exists but that you don't want to preserve old data (if tables exist in that database they will be dropped)
 * follow the wizard, accept most defaults, don't request translations in DB

## Getting rid of the www in the URL
If you don't have the possibility of creating virtual hosts, one quick and easy way to get rid of the www  in the url is to copy the contents of seagull/www to the seagull root folder.  It will be easiest to do this with a standard ftp package.

Because your webroot now exists at two urls, nothing will be broken when you go to login as admin to change your config settings.  What you need to change is 

 * the Config setting 'web root' to the Seagull root folder
 * the Config setting 'base url' - remove the 'www' from the end

You will also need to make a small change to the index.php file.  Change the following lines


	} else {
	    $rootDir = dirname(__FILE__) . '/..';
	    $varDir = dirname(__FILE__) . '/../var';
	}

to 

	} else {
	    $rootDir = dirname(__FILE__) . '/';
	    $varDir = dirname(__FILE__) . '/var';
	}

If you want to use the setup.php file in the future, make the same change there.
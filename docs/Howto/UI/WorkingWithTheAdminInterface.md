<!-- Name: Howto/UI/WorkingWithTheAdminInterface -->
<!-- Version: 2 -->
<!-- Last-Modified: 2007/08/07 18:11:07 -->
<!-- Author: demian -->
# Working with the Administration Interface

[[TOC]]
## Overview
Since its 0.5.5 release, the Seagull framework has been provided with an administration interface (referred to as AdminGUI for Administration Graphical User Interface) which gives it the look and feel of a web application while performing admin tasks.

Currently the admin interface consists of its own theme (default\_admin) and a post-process task which enables it, overriding the current user's theme if the following two criteria are satisfied:

 * does the current manager allow for an admin interface
 * is the admin interface requested

If these two criteria are satisfied, then the current theme, which is first set by reading the user's preferences, is overridden by the value of the adminGuiTheme config key, $conf['site']['adminGuiTheme']

## How to Enable the Admin Interface for a Manager
You can enable the admin interface on a per-module basis using the module's config file.


	seagull/module/<module_name>/conf.ini

For example, in the publisher module's conf.ini file, we have the following :


	[ArticleViewMgr]
	requiresAuth    = false
	
	[ArticleMgr]
	requiresAuth    = true
	adminGuiAllowed = true
	fleschScore     = true
	backDateNumYears = 0
	noExpiry        = false

ArticleView Manager has no setting for _adminGuiAllowed_, so it will then be rendered with the frontend theme, i.e. with the theme set in the user's preferences.

Article Manager, which is meant to perform admin tasks, has the key _adminGuiAllowed_ set to true, therefore it can be rendered with the admin interface.

## How to Request the Admin Interface
Having _adminGuiAllowed_ set to true is not enough is not enough in itself to enable the admin interface, the user must also be granted the right to use the interface.

The permission check is currently hard-coded in a post-process task (see seagull/lib/SGL/Task/Process.php SGL\_Task\_SetupGui).
Only the SGL\_ADMIN role requests the admin interface by default.


	if ($userRid == SGL_ADMIN) {
	    $adminGuiAllowed = true;
	}


## Admin Look and Feel
Icons used in the admin interface (the default\_admin theme) mainly come from Nuvola icon set :

http://www.kde-look.org/content/show.php?content=5358


## Creating Forms
Problem: "I created a admin\_moduleEdit.html page, but my labels and input fields do not line up in nice rows like the users or other existing modules".

If  you created admin\_moduleEdit.html, you should place your CSS file in SGL\_WEB\_DIR/themes/default\_admin/css directory and in module.php file 
enter the following code:


	/*
	-- itemEdit.html --------------------------------------------*/
	#frmItemEdit p label {
	    width: 200px;
	}

where frmItemEdit is your form name.
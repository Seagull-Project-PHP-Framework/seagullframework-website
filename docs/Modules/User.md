---
layout: page
title: How to Use the User Module
permalink: /Modules/User.html
---

<!-- Name: Modules/User -->
<!-- Version: 10 -->
<!-- Last-Modified: 2007/01/04 03:19:44 -->
<!-- Author: demian -->
# How to Use the User Module
* TOC
{:toc}

## Introduction
One of the more important modules of the Seagull framework is the *Users and Security* module which allows site admins to 
  * add users to the system and administer them
  * group users together in terms or organisations
  * create permissions which limit user actions against the system
  * create roles which group similar permissions together
  * add preferences to the system and manage them.

For the full set of features provided by the User module, see [here][1]

## Managing Users
Using the User screens admins can add users to the system as if they registered themselves.  A few additional features are available to admin additions such as the ability to set the user's role, his/her default organisation, and whether they are active or not.  Also, admins can reset the password for any users, and optionally have them notified by email of the new password.  Finally, admins can tweak the user's perms on an individual basis, customising as necessary.

## Managing Organisations
An organisation is an arbitrary grouping of users, most typically used for when they work for the same company.  The concept of orgs is optional in Seagull and can be turned off in the user modules conf.ini file, ie, some smaller, simpler installs of Seagull may not require user groupings.

Organisations are currently designed with a nested set hierarchy, so for example the following situation could be accommodated quite easily:
  * company ABC has many suppliers
  * it also has a number of distributors
  * some suppliers are also clients, or have clients of their own
  * some clients have resellers under them
In such a case the following would not be unheard of

  * company ABC
	* supplier DEF
	  * client GHI
		* reseller JKL

Therefore, using a nested set getBranch operation, you could trace the supply chain from company to reseller with ease, easing the amount of code required for admin operations.  Using the Org manager you can set the organisation type to further aid grouping, in the above scenario this would equate to company, supplier, client, reseller respectively.

In addition, each org has the concept of a default role and default preferences, so each user created in the org would assume the defaults assigned.  This can be useful when managing a large number of users in the system.

## Enabling Organisations
Do the following to create a 'manage orgs' link in the admin interface:

1) in modules/user/conf.ini setting:


	[OrgMgr]
	enabled = true
	requiresAuth = true
	adminGuiAllowed = true
	typeEnabled = false ; organisations can be typed, choose this to enabled editing options

2) from admin panel choose Navigation -\> New Section and insert:

	Section Info
	============
	Title: Manage Organisation
	Parent Page: Users and Security
	Target: output from specified module
	Module: user
	Manager: OrgMgr
	Action: none
	
	
	Editing Options
	===============
	Publish (ON)
	Can view: root


Now go to Users and security -\> Manage Organisation.

## Managing Perms and Roles
Roles and permissions are explained in greater detail [here][2].

## Managing Preferences
Using the preferences screen admins can add any number of preferences to the system.  A preference is defined as a user setting that allows for customisation of the software environment on an individual basis.  A user's preferences persist throughout a session with the system.  Typical instances of preferences are
  * number of results per page
  * look and feel of site
  * interface language
  * locale settings including currency and date formatting such as £1,234.56 vs. €1.234,56
Whatever preferences are set for the admin are what all default, public users will see.  For example, if you login as an admin, and in the preferences section set the theme to 'default', all public users of the site will see this look and feel schema.

Any preferences that are set on a global level can be customised by individual users, the 'My Account' screen which is where users get redirected to by default allows users to modify their preferences.

## User Import Mgr
To use the import manage:

  * using the Document Mgr in the Publisher module, upload a CSV file (the current parser is hard-coded to deal with what you get from a Thunderbird address book CSV export) or use [this example][3]
  * navigate to `http://yourseagullsite/index.php/user/userimport/`
	* select the file you uploaded
	* specify an org
	* choose a non-root role, like 'member'
  * hit 'process file'
  * when the import is complete, navigate to the Users module to see imported users

## Managing Logins
There are a number of options you can set for logins, for example
 * where a member gets forwarded to
 * where an admin gets forwarded to
 * if the login should be recorded, which if it is, can be viewed in the User Manager section of Admin
 * if some observers should be attached to the login event.

All these settings are made in the user module's conf.ini file, found at seagull/modules/user/conf.ini, or you can simply edit it by logging in as admin, listing the systems's modules, and clicking on the link for 'Users and Security'.

The default format for redirects is $moduleName^$managerName, though if you specify just the module, the manager of the same name (default manager) will automatically be selected.

In terms of [observers][4], you can attach additional behaviour to any event in the system simply by registering an observer, and this works quite well for logins.  Typical behaviours you might want to register include:
 * track logins for statistical or marketing purposes
 * log users into multiple systems at once with single sign on
 * check if maximum logins have been exceeded for security purposes
 * update a 'who's online' static list

[1]:	/Modules/User/Features.html
[2]:	/Howto/RolesAndPermissions.html
[3]:	/files/seagull.csv
[4]:	/Howto/PragmaticPatterns/Observers.html
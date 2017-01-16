<!-- Name: Howto/ApplicationMaintenance -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/11/30 15:55:27 -->
<!-- Author: demian -->
<!-- Status: In Progress -->

# Maintaining Your Seagull Apps
## Overview
There are a number of useful tools included in the default module for maintaining Seagull apps.

 * *version checker* - Determines if your install is up to date, this uses a  simple XML RPC call
 * *rebuild DataObject entities* - Every time you modify your database you need to rebuild DataObject entities to reflect the changes.  If you switch from PHP4 to  PHP5 midway during app development you'll also need to do this.
 * *rebuild database sequences* - Seagull strongly encourages devs to to avoid MySQL-specific features like "auto-increment" and standardises on sequence usage for generating primary keys for database records.  Using this approach it is very easy to switch between a MySQL backend and a more robust database like PostgreSQL or Oracle. 
 * *cache management* - Currently there are only tools to delete the various caches - set the global expiry time in Configuration
 * *rebuild environment* - A new feature, use with caution.  When developing locally with a root database user it is extremely useful to be able to drop the database and rebuild your environment (db, tables, default and sample data, navigation, config) from scratch.  After testing your modules use this feature to reset your app to its default state.
 * *manage translations* - This tool allows you to maintain the translation files and check if your particular translation is up to date.
 * *manage PEAR packages* - Currently under development, this tool will allow you to manage your PEAR packages using the browser.  Also use it to install and update pearified Seagull packages.
 * *generate new modules* - Using this feature you can build the fully functional CRUD scaffolding for a new module. It generates the necessary folders, Managers classes, translation files, table aliases and language files for your module.
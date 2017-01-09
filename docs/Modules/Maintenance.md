<!-- Name: Modules/Maintenance -->
<!-- Version: 5 -->
<!-- Last-Modified: 2005/11/15 15:11:06 -->
<!-- Author: aj -->
[[TOC]]

## Maintenance Module
This module is for maintaining some framework related stuff. It contains:

### Manage Translations

    #!html 
    In this section you can manage the translation files for the SGL <abbr title="Graphical User Interface">GUI</abbr>.
It compares your language with the English language files, which should be the most up to date files.
You can choose your `Module` and `Language`, and whether you want to:
  * `validate`: if your module translation is up to date or there are new or outdated words
  * `edit`: your modules translation
  * `check all modules`: same like `validate` but for all registered modules. You get to a overview screen where you can see the status of each module translation.

Please mention that you need proper write rights on the server to add/edit words to the language files.

### Rebuild Data Objects
Here you can rebuild the DataObject classes for your DB tables. These are stored in _var/cache/entities_/

### Rebuild DB Sequences
For rebuilding the sequence table

### Manage Caches
Here you can clear the caches for
  * Templates
  * Navigation & Blocks

### Create a module
This is a basic module skeleton creator. You can tell it the `Module Name` and `Manager Name` and whether it should create
  * `add`
  * `insert`
  * `edit`
  * `update`
  * `delete`
  * `list`

methods and `templates` or `ini-files`
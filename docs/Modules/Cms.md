---
layout: page
title: CMS Module
permalink: /Modules/Cms.html
---

<!-- Name: Modules/Cms -->
<!-- Version: 48 -->
<!-- Last-Modified: 2010/11/12 11:29:50 -->
<!-- Author: demian -->
# CMS Module
* TOC
{:toc}

	#!html
	<blockquote class="note">
	<h3>Download</h3>
	<p>Version 1.7.2 is available for download here: 
	<ul>
	 <li><a href="http://dl.dropbox.com/u/370960/Seagull_CMS-1.7.2.zip">Download</a>
	</ul>
	</blockquote>

## Introduction
Seagull's CMS module allows website owners to create, store and retrieve content quickly and easily. The content data is easily manipulated through the CMS [API][1].

A special effort has been made to give website owners greater flexibility when defining content types thus allowing use of the CMS module in many different contexts: to create FAQs, Personal Datasheets, Weather Reports, Recipes, etc.

[[Image(cms\_multlang.jpg, nolink)]]

[CMS Multilang screencast (9.1 MB)][2]

## Features
 * Ajax web interface and [programmatic API][3]
 * Easy to use installation wizard
 * Light footprint, module that runs on Seagull only 153kb
 * Ability to define unlimited content types and instances
 * Content types can be built from range of attribute types (integer, string, list, money, etc)
 * Support for nested lists (Attribute lists)
 * Content can by classified into categories and by tag
 * Multi-language support
 * Content versioning
 * Multiple export formats supported (RSS, SQL)
 * Import data from Seagull's Publisher module, from local or remote site
 * Dynamic web search interface, ability to refine by attribute and content meta data
 * Manage navigation, categories and content items, all in multiple versioned languages
	 
## Product plan
The community edition can be freely downloaded and allows access to most of the CMS module's features through an intuitive [API][4].  Standard and Plus versions are also available that allow for greater flexibility in managing your content.  A Pro version is available where you can have access to the source code and modify it freely.  Pro customers will also be granted access to the module's source code repository.

The CMS module developers have made most of the module's features available for free to the Seagull community and are committed to improving the product based on community and customer feedback.

## Demo
You can try the demo at [http://demo.cms.seagullsystems.com/][5]

Or a preview of our new version with improved UI: http://preview.cms.seagullsystems.com/index.php/user/login/  [user: moderator  password: qwerty, then click on 'admin area' in the top right]

## Pricing
|---
| *Feature* | *Community Edition* | *Standard* | *Plus* | *Pro* |
| Price | FREE | €99 | €399 | €999 |
| Source Code | obfuscated | obfuscated | obfuscated | available |
| Commercial use permitted | yes | yes | yes | yes |
| Access to base [API][6] | yes | yes | yes | yes |
| AJAX interface | yes | yes | yes | yes |
| Import from Publisher| yes | yes | yes | yes |
| Data Versioning | yes | yes | yes | yes |
| Tags | no | yes | yes | yes |
| Export (XML, PDF) | no | no | yes | yes |
| Multiple Language Support | no | no | yes | yes |
| Web service interface | no | no | yes | yes |

Standard, Plus and Pro versions come with a 1 year license that includes support and upgrades.

## Install
 1. Download the Seagull CMS from the link above and unzip the package.  Run the Seagull installer in the normal way, the package includes the framework and required modules.  http://example.com/setup.php
 1. Customise the database login details in  customInstallDefaults.ini if needed, it's set to 'root' and no password by default
 1. Login as admin / admin or whatever you set the password to be and choose 'CMS' from the navigation for options

*Bug alert*: If you rerun the setup.php script with a new database name, tables in your old db will be TRUNCATED!  

## Upgrades
Upgrades for the community version will be released periodically.  Bugfixes and enhancements will be prioritised for CMS module customers.
### upgrade 1.2 to 1.3
Please execute the provided script in <install-dir>/modules/cms/data

### upgrade 1.5 to 1.6
 * new installs
   * run the installer as normal, with latest customInstallDefault.ini file placed in sgl/etc
   * rebuild with sample data
 * svn  updates
   * Update your CMS and Seagull 0.6.x repos
   * Execute the provided script in <install-dir>/modules/cms/data
   * Add the rebuild task SGL\_Task\_CmsSetupCheck and rebuild with sample data

## Usage
### To create content
 * select 'Manage Content' from the admin menu
 * click on the 'New Content' combobox and select one of the sample content types
 * fill in the form and hit the 'create' button

### To edit content
 * select 'Manage Content' from the admin menu
 * for regular form editing, click on the content's edit link to the right and edit as normal
 * for inline editing, click on the content's title in the list
   * to edit the content, click in an area of the content, ie the title or a paragraph or an image
   * to save your changes, click outside of the content

### To create a new content type
 * select 'Content Types' from the user menu
 * click on the 'New Content Type' combobox and enter the content type name, and the number of attributes you want to add initially
 * enter names for each content type attribute, and choose the attribute type from the combobox
 * save when you're finished, or to add/remove additional content type attributes

### To edit an existing content type
 * select 'Content Types' from the user menu
 * click on a content type from the list to view its attributes
 * click 'edit' to edit it
 * click the delete button to delete an attribute, or add a new attribute at the bottom of the screen
 * save when you're finished

### To request content in a specific language
Content generated by the CMS is returned in the default language set in the framework Config, which is sometimes referred to as the fallback lang.

If you want to return your content (CMS content, navigation, categories) in any version other than the default, use the lang parameter in the request:

	http://localhost/cms/0.6-bugfix/www/index.php/cms/contentview/frmContentId/2/lang/zh-TW/

ISO 639 language codes are expected, so use 'en' rather than en-utf-8, etc.

For pretty URLs use Seagull routes to hide the parameters.

See [/Modules/Cms/i18n][7] for more details.


## Requirements
 * requires \>= PHP 5.2, MySQL \>= 4.1
 * requires a working install of latest Seagull (\>= 0.6.6), preferably with PHP installed as mod\_php
 * currently AJAX works well for all modern browsers
 * not compatible with the Publisher module, this must be uninstalled if you have it enabled  
 * depends on the Media and Translation modules being installed.  The Media module requires the GD extension for full image transformation functionality


## FAQ
*Q: Do I need the Zend Optimizer extension or something similar to get the obfuscated code to work?* [[BR]]
A: No, it will work in a standard PHP installation.

*Q: I think I found a problem, what should I do?* [[BR]]
A: Please report the issue to the forum or [Seagull mailing list][8].

[1]:	/Modules/Cms/Api.html
[2]:	/files/screencasts/Seagull-cms-multilang.mov
[3]:	/Modules/Cms/Api.html
[4]:	/Modules/Cms/Api.html
[5]:	http://demo.cms.seagullsystems.com/
[6]:	/wiki:Modules/Cms/Api/
[7]:	/Modules/Cms/i18n.html
[8]:	http://groups.google.co.uk/group/seagull_general?hl=en
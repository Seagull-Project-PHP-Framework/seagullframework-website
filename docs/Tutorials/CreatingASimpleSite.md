---
layout: page
title: Creating A Simple Site
permalink: /Tutorials/CreatingASimpleSite.html
---

<!-- Name: Tutorials/CreatingASimpleSite -->
<!-- Version: 28 -->
<!-- Last-Modified: 2008/04/28 11:24:20 -->
<!-- Author: demian -->

# Creating a simple site
* TOC
{:toc}

## Install Seagull
 * this tutorial will work with versions \>= 0.6.0 of Seagull 
 * to install Seagull follow the simple [Installation][1] instructions here.

## Create your own theme
Seagull themes are located in the seaugll/www/themes directory and are Flexy templates.  Basic distros are suppled with two themes:
 * default - the frontend html templates
 * default\_admin - the admin section html templates

For starters let's create a new frontend theme, to do this create a folder in the themes directory like so:


	seagull/www/themes/your_theme
Themes use pseudo inheritance so any template you do not duplicate and customise, will be taken from the default theme.  This means you can customise just the header, banner and footer templates, that make up the top and bottom of each screen, and end up with quite a customised looking site.

== Modify Headers and Footers == 
To create your own look and feel you need to first copy the images and css folders from the default theme into your theme.  So grab them from here:


	`-- www
	    |-- themes
	    |   |-- default
	    |   |   |-- css
	    |   |   |   |-- core.php
	    |   |   |   |-- style.php
	    |   |   |   |-- vars.php
	    |   |   `-- images
	    |   |       |-- *

and copy them to your\_theme



	`-- www
	    |-- themes
	    |   |-- your_theme

Then you need to copy at least 3 templates from the default module's theme folder into your custom theme.  So from here:

	|-- modules
	|   |-- default
	|   |   |-- templates
	|   |   |   |-- banner.html
	|   |   |   |-- footer.html
	|   |   |   |-- header.html

to here:

	`-- www
	|   |-- themes
	|   |   |-- your_theme
	|   |   |   |-- default
	|   |   |   |   |-- banner.html
	|   |   |   |   |-- footer.html
	|   |   |   |   |-- header.html


 * *banner.html* - this is where your masthead image would be defined, and navigation and perhaps a login link
 * *header.html* - contains everything from the first <html> tag until the end of the opening <body> tag and is useful for including custom style links or declarations
 * *footer.html* - copyright info and whatever you'd like to place at the bottom of each screen

## Activating the new theme
To activate the new theme:
  * login to the admin section
  * choose General -\> Configuration
  * towards the bottom of the general tab, find the key 'Default theme', and enter your theme name there and save it
  * logout and the theme will become active

 * *Note*: In order for the admin user (or any user that registered before you changed the default theme) to have the frontend displayed within this new activated theme, that user will have to log in and change his/her default theme setting in his/her user preferences.

## Installing some Modules
By default only the core modules are installed. Seagull offers you many more modules for specialised functionality, e.g. a CMS called Publisher, a module for managing FAQs etc.

To install such a module 
  * login to the admin section using the username and password you set during installation
  * choose _General_ -\> _Manage Modules_ from the admin menu
  * check the _show uninstalled modules_ checkbox on top of the list of installed modules
  * choose the module you want to install.

For sake of this tutorial we install the _publisher_ module. After clicking on the green plus sign you see it in the list of installed modules, including more info about the module and the navigation item added to the admin menu.

== Create a few Content Pages == 
To create pages like 'about us' or 'our products' you need to do the following
  * login to the admin section
  * choose _Publishing_ from the left hand menu
  * click _New Article_, then _Static HTML Article_.  A static article is like a permanent section in your site, like _about us_, or a privacy policy.  There are also html articles that can appear in a list and be categorised.
  * When creating your articles, remember you can cut and paste content from other sources, ie, other web pages or MS word documents.  Be sure to give meaningful titles to the articles.
  * Set the status of each article to _published_ by hitting the action link (twice) at the far right after saving your article

## Create Navigation
[[Image(navigation.jpg,right)]]
Next you need to create some navigation to lead viewers to your articles
  * from the left menu select _Navigation_
  * each element in the list represents a navigation tab, and they are categorised in two branches, "User menu" and "Admin menu", ie what navigation you see depends on which role you are using to view your Seagull installation
  * hit _New Section_
  * for the section title call your page "About us"
  * for the parent section select "User menu" so this navigation will show up for site users
  * the target will be set to static articles as this is the first type in the list, so you should see the names of the articles you created in the previous section in the _Static article title_ combobox
  * after selecting your article from the combobox, click the _Add an alias_ checkbox if you want a Search Engine Friendly URI created, this is recommended
  * hit the save button and confirm the navigation is present by viewing the site from the frontend (ie, click on the main Seagull logo, top left, to take you to the default page)
  * you can return to the admin backend by clicking the link 'Admin'

That's all there is to creating navigation. Un summary, for each navigation tab you want to create, give it a name, select the content it will link to, and in the _editing options_ tab, tick the _Publish_ checkbox to activate it, and ctrl-select all the user roles who you want to be able to access your page.  That's all there is to it.

## Conclusion
There you have it,  you have now created a basic site.  If you want more advanced features you can select some of the readymade modules, ie, FAQs, a registration page, a guestbook, etc, and create navigation from the previous step to link to them.

[1]:	/wiki:Installation/
---
layout: page
title: How to setup a Virtual Host in Apache
permalink: /Howto/Navigation/WorkingWithCategories.html
---

<!-- Name: Howto/Navigation/WorkingWithCategories -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/08/09 15:30:05 -->
<!-- Author: demian -->
# Working with Categories
* TOC
{:toc}

This tutorial is for how to add categories to a module.

## Getting started
  * Register your module. Otherwise you cannot add it to navigation.
  * Create new category
	* Go to Publisher -\> Categories -\> Add new Root Category
	* Add your Category: Enter Category Name and Permissions (role based)
	* notice the new created navigation id.
  * Create some sub categories 
  * Create the navigation block. 
	* You can copy it from e.g. CategoryNav.php
	* rename the class properly
	* add your id to `$menu->setStartId();` in `getBlockContent()`
  * Add your module to navigation. Otherwise you cannot add the category block
  * Add the category block to your module (navigation point)
  * Add the category block (CategoryNav or your new block) to your page:
	* in your `ModuleMgr` class add to function validate():
		  
	  `$input->javascriptSrc   = array('TreeMenu.js');`

	  Now the Block with rendered category tree should be visible.
	  The category block takes care of appending the rigth parameters the URL.

## SQL
add a column `category_id` to your modules table:

`ALTER TABLE ‘event’ ADD ‘category_id’ INT( 11 ) NOT NULL AFTER ‘event_id’ ;`

That's all you have to change in your db schema, as Seagull cares about nested set structures.

Don't forget to rebuild the Data Objects!

## Code inside your modules
To use categories inside your modules you have to include `/navigation/classes/CategoryMgr.php`:

	require_once SGL_MOD_DIR.'/navigation/classes/CategoryMgr.php';

### \_validation()
Get the current category by the navigation block:

	$input->currentCat  = (int)$req->get('frmCatID');

### generate select box
I made a simple function that returns an array of the sub categories:

	    function _generateCategoryArray($rootCatId)
	    {
	        $menu1 = & new MenuBuilder('SelectBox');
	        $menu1->setStartId($rootCatId);
	        $aHtmlOptions = $menu1->toHtml();
	        return $aHtmlOptions;
	    }

include this in the add method:

	$output->aCategories = $this->_generateCategoryArray($rootCatId);


and in the edit method i included:

	$output->aCategories = $this->_generateCategoryArray($rootCatId);
	$output->currentCat = $event->category_id;
where $event is my module's data object.

In the template:

	<select name="event[category_id]">{generateSelect(aCategories,currentCat):h}</select>



### Breadcrumbs (you are here:)

	$menu = & new MenuBuilder('SelectBox');
	$output->breadCrumbs = $menu->getBreadCrumbs($input->currentCat);

In the template:

	<strong>{translate(#You are here#)}:</strong> {breadCrumbs:h}
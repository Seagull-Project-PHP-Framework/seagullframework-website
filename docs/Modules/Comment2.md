<!-- Name: Modules/Comment2 -->
<!-- Version: 1 -->
<!-- Last-Modified: 2008/06/17 17:39:25 -->
<!-- Author: demian -->
# Comment2 Module
[[TOC]]
 * requires >= Seagull 0.6.5

## Overview
The comment2 module demonstrates an easy to to add commenting to any of your existing modules.

It offers several improvements over the existing module, namely
 * it's Ajax driven using jquery so no messy redirects are needed
 * Ajax submits and lists are very fast, making commenting more interactive

The module also includes a www folder with web resources, ie CSS, javascript and images, demonstrating how easy it is to build these into your modules
 
## Usage
This module is provided to demonstrate how easy it is to use jquery with Seagull.  When the module is installed, you'll get a navigation element "comments" which invokes the example manager supplied with the module.

The best way to follow the full Ajax workflow is, using the Zend debugger:
 1. load the comments screen in Firefox
 1. using the zend debug tool bar, select "next page"
 1. write a new comment and hit submit

This will start an interactive debug session where you can follow the workflow of the javascript calls.

## Going Forward
The Ajax usage anticipates SGL_AjaxProvider2 that will be part of Seagull 0.9.x, the main features are:
 * $input and $output objects are available to every action method
 * jquery is used for the javascript library, which is quite a bit more succinct and powerful than prototype/scriptaculous
 * there are improved hooks for cleaning up JSON sent back to the browser
 * a raiseMsg() method has been added to SGL_AjaxProvider, making it much easier to send error/warning/info message back to the user
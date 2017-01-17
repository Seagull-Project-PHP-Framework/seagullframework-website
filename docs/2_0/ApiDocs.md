---
layout: page
title: API Docs
permalink: 2_0/ApiDocs
---

<!-- Name: 2_0/ApiDocs -->
<!-- Version: 4 -->
<!-- Last-Modified: 2010/03/23 19:06:07 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# API Docs
* TOC
{:toc}

## Overview
The API docs can be easily generated as follows:

Install `PhpDocumentor`:


	$ pear install PhpDocumentor

From Seagull root:

	$ phpdoc -d lib -f README.markdown -t docs --pear on -ric README.markdown --title "Seagull 2.0 API Docs" --output "HTML:Smarty:HandS"

To view on a Mac:

	$ open docs/index.html

## Also In This Series

 - [Overview][1]
 - [Events and Plugins][2]
 - [Autoloading][3]
 - [API Docs][4]
 - [Unit Tests][5]


[1]:	/2_0/Overview.html
[2]:	/2_0/EventsAndPlugins.html
[3]:	/2_0/AutoLoading.html
[4]:	/2_0/ApiDocs.html
[5]:	/2_0/UnitTests.html
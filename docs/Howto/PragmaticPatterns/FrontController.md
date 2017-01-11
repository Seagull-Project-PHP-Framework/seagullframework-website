---
layout: page
title: Front Controller
permalink: /Howto/PragmaticPatterns/FrontController.html
---

<!-- Name: Howto/PragmaticPatterns/FrontController -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/04/02 17:07:49 -->
<!-- Author: demian -->

# Front Controller
* TOC
{:toc}

## Overview
The front controller is responsible for 
 * initialising the Seagull environment
 * initialising the Request object
 * building a filter chain 
 * and passing the request through the chain to its eventual target

## Initialising the Environment
In order to make the app setup as configurable as possible, a Task Runner is used.  Task objects can be added to a batch job and are executed in order, optionally receiving data as a parameter.

Here is an example batch job:


	    $batch = new SGL_TaskRunner();
	    $batch->addData($myData);
	    $batch->addTask(new SGL_Task_GoToMarket());
	    $batch->addTask(new SGL_Task_StayHome());
	    $batch->addTask(new SGL_Task_EatRoastBeef())
	    $batch->main();

Tasks are interchangable, reusable objects that perform discreet .. you guessed it .. tasks.

Setting up the Seagull environment involves establishing the application paths, setting constants, loading configuration, ensuring backwards compatibility if older versions of PHP are detected, etc.

The main job of the front controller however is building a filter chain specific to the current request.  If you want to initalise the app differently, or use a different filter chain or entirely different approach, write a custom front controller that extends SGL\_FrontController.

## Code Examples
Different examples of front controllers:
 * [source:/trunk/docs/developer/examples/controllers]
 * [source:/trunk/etc/sglBridge.php]

Various task runners:
 * [source:/trunk/www/setup.php]

## See Also
[[SubWiki(Howto/PragmaticPatterns)]]
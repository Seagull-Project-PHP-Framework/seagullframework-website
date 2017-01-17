---
layout: page
title: Unit Tests
permalink: 2_0/UnitTests
---

<!-- Name: 2_0/UnitTests -->
<!-- Version: 5 -->
<!-- Last-Modified: 2010/03/20 17:45:59 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Unit Tests
* TOC
{:toc}

## Install
Install `PHPunit`'s channel

	$ pear channel-discover pear.phpunit.de
Install the package

	$ pear install phpunit/PHPUnit

Ensure you are in the root of the sgl2 directory, and make sure the tests autoloader is setup correctly

	$ cd /path/to/sgl2

Run Seagull's test suite

	$ phpunit tests

## Setting up the tests autoloader
When you checkout sgl2 you will notice the `scripts` directory.  The `PHPunit` tests require a bootstrap autoloader to run correctly.

Execute the autoloader script (you must be running PHP 5.3.x)

	$ php scripts/autoload.php src/lib tests/out_bootstrap.php

Ensure that the `phpunit` "binary" is including the bootstrap before running.  Currently I am requiring the bootstrap from the `phpunit` script, which is not ideal.

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
---
layout: page
title: Developing Modules
permalink: /Tutorials/HelloWorld.html
---

<!-- Name: Tutorials/HelloWorld -->
<!-- Version: 36 -->
<!-- Last-Modified: 2008/03/04 19:30:55 -->
<!-- Author: spaycegirl -->
# 1. Creating A Simple Module to Display "Hello World"
> requires \>= Seagull 0.6.0

* TOC
{:toc}

## Introduction
Seagull includes a module generator tool, available under the 'General' section in the admin area, which will create a scaffolding of all the necessary files to build a module.  This tutorial, however, will go through creating the files manually to better explain the process.  

## Creating the Necessary Folders and Files
All functionality in Seagull is grouped together in modules, and modules themselves contain many resources. For the sake of this tutorial we will only create the bare minimum required to be able to print 'Hello World' on a web page.

In the modules directory, create a folder called _helloworld_, and in it, create folders called _classes_, _templates_ and _data_. You should have a structure like this: 

![][image-1]

	
	|-- [modules]
	|   |-- [helloworld]
	|   |   |-- [classes]
	|   |   |-- [data]
	|   |   `-- [templates]
The first file we are going to create is a class that will _manage_ the HelloWorld request, so let's call it _HelloWorldMgr.php_. In fact, all functionality which groups together a number of similar actions that display/modify data of a given type, occurs in classes called Managers. For example, if you want to create the functionality to add, edit and delete FAQs, the code will be managed in a file called `FaqMgr.php`, with _Mgr_ being the standard shorthand for _Manager_. If this sounds to you like a Controller, you're absolutely right, and at some point in the near future managers may be renamed to controllers.

Create the file HelloWorldMgr.php in the classes folder, and in it create a class definition for `HelloWorldMgr`, extending `SGL_Manager` 

	<?php
	class HelloWorldMgr extends SGL_Manager
	{
	    // ...
	}
	?>
If you are using MySQL as your database software, then create a file called data.default.my.sql in the data folder. Place this code in the file:

	INSERT INTO module VALUES ({SGL_NEXT_ID}, 1, 'helloworld', 'Hello World', 'A ''Hello World'' test module.', '', '', 'Alouicious Bird', NULL, 'BSD', 'alpha');
Finally, in the root of the module folder, create a file called _conf.ini_, each module has one of these and they are mandatory as they create a mapping between the manager name and the URI that requests it.  Your resulting file tree should look like this.


	|-- [modules]
	|   |-- [helloworld]
	|   |   |-- [classes]
	|   |   |   `-- HelloWorldMgr.php
	|   |   |-- [data]
	|   |   |   `-- data.default.my.sql
	|   |   |-- [templates]
	|   |   `-- conf.ini

In the ini file, enter the following line which creates a simple mapping:

	[HelloWorldMgr]

This simple entry gets around case sensitivity problems with class names in PHP4.

## Registering the Module
For security purposes, even if the module exists in your file system, the front controller will not load it if it is not registered.  To register a new module login to the admin section, and choose *General -\> Manage Modules* and then check the _show uninstalled modules_ checkbox.  Next click the _+_ sign under the _Action_ column.  For a module to be registered it just needs a record in the modules table, so you can adjust a module's state manually if required.

So let's register our helloworld module now: 
 * login as admin
 * check _show uninstalled modules_
 * hit the plus next to helloworld

You should get the message

	The helloworld module was successfully installed


## Requesting your new Manager
The next step is to call the new Manager from the browser, the format is very simple:


	http://your-site.com/seagull/index.php/moduleName/managerName/

so try

	http://your-site.com/seagull/index.php/helloworld/HelloWorldMgr/

If you get a standard Seagull page, with a header and footer, few blocks and not much else, then all is going well.  The URI format is quite flexible, so the following URIs will also work:

	http://your-site.com/seagull/index.php/helloworld/HelloWorld/

and 

	http://your-site.com/seagull/index.php/helloworld/helloworld/

This last format is the preferred one, with the _Mgr_ stripped and the class name lowercased.

Finally, if your module name is 'foo', and your class is FooMgr, and you have the same duplication as above, you can omit the manager name, ie the following works:

	http://your-site.com/seagull/index.php/helloworld/

Try it now.  In such cases you can consider HelloWorldMgr as the default manager for the module.

## Adding some Data
After all that preparation, it is time to generate the "Hello World!" output.  The most direct way of doing this is to add the following method to your HelloWorldMgr class:

	function display(&$output)
	{
	    print 'Hello World!';
	}

Every manager can have a display method, it's a catch-all that gets fired after the requested action has been invoked.  For our purposes we'll print out _Hello World_ directly in this method, as opposed to sending it to a template.

Refresh the browser and you should see the text _Hello World_ awkwardly appearing above the header, this is simply because we're printing directly to standard out, our next step will be to send the string to a template so it can be situated within the web page.

 * *Note*: depending on your version of PHP you might get a lot of errors to do with 'Cannot modify header information'.  In more recent PHP versions output buffering is enabled by default, but you can achieve the same effect by going to General -\> Configuration and set 'output buffering' to Yes.  Don't forget to save your changes.

## Creating the Template
In the templates directory now create a file called _helloWorld.html_, your module should now look like this:

	|-- [modules]
	|   |-- [helloworld]
	|   |   |-- [classes]
	|   |   |   `-- HelloWorldMgr.php
	|   |   |-- [data]
	|   |   |   `-- data.default.my.sql
	|   |   |-- [templates]
	|   |   |   `-- helloWorld.html
	|   |   `-- conf.ini

Within the file add a single variable, in `Flexy` all variables need to be surrounded by curly braces:

	{testVariable}

## Putting it all Together
In order for the template to get parsed and the variable substitution to take place, you need to do a few more things:

 1. tell the manager which template to parse
 1. assign the Hello World string to the $output variable

Your finished class should look like this:

	class HelloWorldMgr extends SGL_Manager
	{
	    function display(&$output)
	    {
	        $output->template = 'helloWorld.html';
	        $output->testVariable = 'Hello World!';
	    }
	}

And the finished results should look like this:

![][image-2]

## Download Code
You can download the code from this tutorial [here][1].

## Further Reading
Next time we will cover more techniques to get better flexibility out of your module:
 * [handling multiple actions][2]
 * validating request data
 * requiring authentication
 * checking for perms
 * displaying GUI in multiple languages
 * creating a custom layout
 * adding your own blocks
 * incorporating your module in the site navigation
 * using a Data Access layer
 * using Seagull libraries

[1]:	/images/Tutorials/HelloWorld/tutorial-1.tar.gz
[2]:	/Tutorials/WorkingWithActions.html

[image-1]:	/files/images/files.png
[image-2]:	/files/images/helloWorld.png
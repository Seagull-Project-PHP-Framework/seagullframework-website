---
layout: page
title: Developing Modules
permalink: /Tutorials/WorkingWithActions.html
---

<!-- Name: Tutorials/WorkingWithActions -->
<!-- Version: 20 -->
<!-- Last-Modified: 2007/04/05 10:00:39 -->
<!-- Author: bjorn -->
# 2. Working With Actions
>  requires \>= Seagull 0.6.0

* TOC
{:toc}

## Introduction
Now that you've managed to generate your first output, it's time to take a look at actions, or how you can perform multiple actions in one manager.

## GET and POST
We're going to try a GET action first as it's the simplest.  You've managed to create some output by requesting a specific URI, now we're going to change that output by clicking a link and invoking a different action.

## Creating a Link
A number of template helpers are available by default without the need to load any extra libs, these can be found in the [source:/trunk/lib/SGL/Output.php Output] library.  It provides methods which you can use in your templates for:
 * creating URIs
 * formatting dates
 * building comboboxes, sets of radio buttons and typical HTML controls
 * permission checks
 * translation

To create a simple link you need between zero and three parameters to the makeUrl() function.  Used by itself

	{makeUrl()}

it will create a link to the current page, in other words, it will detect the current action, manager and module, and supply them for you in the link.  Try it now in your template, do something like:

	<a href="{makeUrl()}">say good bye</a>

If you view the source that should have created something like

	<a href="http://localhost/seagull/www/index.php/helloworld/">say good bye</a>

The three optional parameters to makeUrl() are simply action name, manager name, module name, so the method has this prototype:

	{makeUrl(action,manager,module)}

Before we storm too far ahead let's review some conventions for Flexy templates, the default template engine used by Seagull.  Developers do have the choice however to run whatever their favourite engine is, currently supported are Flexy, Smarty and Savant.  The conventions are:

 1. all variables and function names must be surrounded by curly braces
 1. never use spaces between your variable arguments, Flexy doesn't like these

We'll review more as we get to it.  So for now, let's call an action in the new URI, for example _sayGoodBye_.  Put the following in your template:


	<a href="{makeUrl(#sayGoodBye#)}">say good bye</a>

What are the hashes, you ask?  Surround your args with hashes if they are string literals, in other words, if they are not variables.  If you view source, your output should now look like this:

	<a href="http://localhost/seagull/www/index.php/helloworld/action/sayGoodBye/">say good bye</a>

## Create the Action Mapping
Now that we have an action specified in our URI, the next step is to add a bit more code to our HelloWorldMgr class to listen for the action calls.

The first thing we need to do is add an action mapping in the class' constructor, this will tell Seagull which method or methods to invoke based on which action params are requested in the URI.  Using the class your created in the [last tutorial][1], create a constructor method like this:

	    function HelloWorldMgr()
	    {
	        parent::SGL_Manager();
	        $this->_aActionsMapping =  array(
	            'sayGoodBye'   => array('sayGoodBye'),
	        );
	    }

So a few things that should be mentioned about this code:
 * keep your constructor [PHP4 style][2] if you want your project to be  PHP4/5 compatible, but of course there's no harm in using a [PHP5-only constructor][3] if that's what you want.
 * no arguments are required for managers in the constructor
 * invoke the constructor of the parent class - in brief this is done to make resources available to your manager, eg a database connection, a config map, etc.

## Getting Request Variables
Seagull encourages developers to manage request variables conscientiously.  What you effectively end up is the exact opposite of the old register\_globals situation, that is, only variables explicitly declared are available to your program.  Whereas you might be used to doing things like

	SELECT foo FROM var where id = $_GET['id']

Seagull encourages a more careful approach.

 * *Tip*: all user input data can be considered tainted until it is assigned to the $input object

Every manager needs a validate method, and this is where, not surprisingly, you will be validating your input data.  The validate method takes two arguments, the request object and the input object, and it gives you a chance to test for the presence of request variables and assign them to the $input object which will be in scope for all later processing done by the framework.
 * the request object is an encapsulation of $\_REQUEST data, it provides methods for cleaning the data
 * the input object acts as a registry for data and resources used throughout the request/response cycle
 * you supply input to your action methods which, after processing, gets assigned to the output object

The concept is very simple, data is gathered from the request, and if it's deemed valid, it's forwarded to the action method indicated by the _action_ parameter in the request.  If it's not valid, in other words if the $validated property of your manager is set to false, the application controller forwards the request data straight to the display method, and no processing takes place.

With that preamble out of the way, here is our simple validate method:

	    function validate($req, &$input)
	    {
	        $this->validated = true;
	        $input->action   = $req->get('action');
	    }

Add this to HelloWorldMgr after the constructor.

## Setting a Default Action
The validate needs one more refinement, we must set a default action.  So if an action is not explicitly set in the URI, a default will be assigned to $input-\>action.  Please update your validate() method as follows:

	    function validate($req, &$input)
	    {
	        $this->validated = true;
	        $input->action   = ($req->get('action')) ? $req->get('action') : 'sayGoodBye';
	    }

Action names are case sensitive so be careful.  If this causes you a headache, adopt the convention of only using lowercase action names.

The request object is supplied as a parameter to the validate method, we get the value of the action variable with a $req-\>get() method, and we map it to the input object.  The application controller will use this value to forward the request to the relevant action method, in our case the _sayGoodBye_ method.  And, for the sake of simplicity, we don't do any validation on the data just yet, we just set the manager's state to validated.

## Creating the Action Method
With the mapping and validation in place, we are now ready to create the action method.  For starters let's just make sure we arrive at the right method with a simple echo 'foo', so add the following to your code:

	    function _cmd_sayGoodBye(&$input, &$output)
	    {
	        print 'foo';
	    }

Notice the action method is prefixed with the string __cmd__, this is simply to differentiate it from public and private methods.  Action methods are semi-private, so if you specify _bar_ as the action parameter in the request, the app controller will look for a \__cmd\_bar_ method in your manager.

As a side note, we have found that grouping numerous action methods together in one manager is very useful and efficient for prototyping apps, the fewer files you need to have open the faster you can work.  Managers can also contain private methods, typically for data fetching, or public methods, that other managers in the same module may need access to.

Enough with the theory, if you save the above code, refresh your browser and you should now get a nice 'foo' right above the header, indicating that everything is working as expected so far.

## Using Actions to Modify Data
Now that hopefully you're satisfied that you can link requests to action methods, let's complete the example and use action requests to toggle between a _hello_ and _goodbye world_ output from the framework.

For this we need two action methods, our existing \_\_cmd\_sayGoodBye()\_ and an additional \_\_cmd\_sayHello()\_ which we will set to be executed by default.  Please make the following small modifications to your HelloWorldMgr class so it resembles the following:

	<?php
	class HelloWorldMgr extends SGL_Manager
	{
	    function HelloWorldMgr()
	    {
	        parent::SGL_Manager();
	        $this->_aActionsMapping =  array(
	            'sayHello'    => array('sayHello'),
	            'sayGoodBye'    => array('sayGoodBye'),
	        );
	    }
	
	    function validate($req, &$input)
	    {
	        $this->validated = true;
	        $input->action   = ($req->get('action')) ? $req->get('action') : 'sayHello';
	    }
	
	    function display(&$output)
	    {
	        $output->template = 'helloWorld.html';
	        $output->testVariable = 'World!';
	    }
	
	    function _cmd_sayHello(&$input, &$output)
	    {
	        $output->word = 'Hello';
	    }
	
	    function _cmd_sayGoodBye(&$input, &$output)
	    {
	        $output->word = 'Goodbye';
	    }
	}
	?>

What we have done is the following:
 1. we now have two action methods, \_\_cmd\_sayHello()\_ and \_\_cmd\_sayGoodBye()\_
 1. each method has a corresponding mapping in the _$this-\>\_aActionsMapping_ hash in the constructor
 1. in _validate()_, a default action is set if none is detected from the request
 1. the hard-coded _Hello World_ is removed from _$output-\>testVariable_ and the new variable $output-\>word will contain the value to be changed by the request

Also, please make a small update to your template so it now looks like this:

	{word} {testVariable}
	<p><a href="{makeUrl(#sayHello#)}">say hello</a></p>
	<p><a href="{makeUrl(#sayGoodBye#)}">say good bye</a></p>

So now we have two action links, and two variables, the first will evaluate to _Hello_ or _Goodbye_, the second is just _Hello World_ from our previous example.  If you're wondering why there are no return statements in the action methods, it's because the _$input_ and _$output_ objects are [passed by reference][4], so there's no need to return values.

 * *Tip*: if you're using PHP5 exclusively, you don't need to indicate that arguments are passed to methods by reference, this happens by default

To run the example just load the manager in the browser and click on the hello and goodbye links and watch the output change.

	http://localhost/seagull/www/index.php/helloworld/

And the finished results should look like this:

![][image-1]

## Conclusion
In this tutorial you have learned how to create a basic Manager class, various actions methods, URI mapping and a simple template.  Stay tuned for future installments where will introduce more techniques for working with Seagull.

## Download Code
You can download the code from this tutorial [attachment:tutorial-2.tar.gz here].

#### Comment by bjorn on Thu Apr  5 04:00:39 2007
If you'd like to add id's or other information to the links it can be done this way in the template:

	<a href="{makeUrl(#edit#,#campaign#,#campaign#,##,#frmCampaignID|campaignId#,key)}">{translate(#Edit this campaign#)}</a>

In this example 'campaignId' has a value in the template and the link will be:

http://mydomain.com/index.php/campaign/action/edit/frmCampaignID/2/

[[AddComment]]

[1]:	/wiki:Tutorials/HelloWorld/
[2]:	http://php.net/manual/en/language.oop.constructor.php
[3]:	http://php.net/manual/en/language.oop5.decon.php
[4]:	http://php.net/manual/en/language.references.pass.php

[image-1]:	/files/images/goodbyeWorld.png
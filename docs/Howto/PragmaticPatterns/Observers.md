---
layout: page
title: Front Controller
permalink: /Howto/PragmaticPatterns/Observers.html
---

<!-- Name: Howto/PragmaticPatterns/Observers -->
<!-- Version: 4 -->
<!-- Last-Modified: 2007/02/14 15:47:04 -->
<!-- Author: openhaus -->

# Setting up Subject/Observer Relationships
* TOC
{:toc}

## Overview
Often in a program an input stimulus triggers a certain event, but as business logic grows, the number of events triggered grows.  The observer pattern is an ideal way to attach responses to events.  Take the simple example of a user registration which we'll call the 'registration event'.

A user registers for an insurance policy, upon invocation this event requires the following responses:
 * email sent to user confirming registration request
 * email sent to admin notifying new registrant
 * insertion of user record in a temporary table, awaiting confirmation
 * posting of user info to marketing partner gateway
 * increment of stats table 
 * and so on ...

Handling these responses procedurally quickly turns your code into a mess.  Furthermore, responses should be modular so they can be reused across a range of events.  Enter the observer pattern, the example here is taken from a [gitlink:/modules/user/classes/RegisterMgr.php Seagull registration].

## In Practice
Looking at the insert action of the RegisterMgr, a few steps are required to setup observers that listen for this event.


	Subject = the registration event,  User_AddUser, encapsulated in RegisterMgr::_cmd_insert();
	Observers = EmailConfirmation, AuthenticateUser, UpdateTracAuth

The insert action must create a subject object, also referred to as an observable object.  The class in question is:


	class User_AddUser extends SGL_Observable {}

By extending SGL\_Observable the User\_AddUser class is implementing _observable_ behaviour.  Note in PHP5 the _implements_ keyword would be more appropriate.

The observers are attached to the observable event, in this case the list of observers comes from a config file:


	$addUser = new User_AddUser($input, $output);
	$aObservers = explode(',', $this->conf['RegisterMgr']['observers']);
	foreach ($aObservers as $observer) {
	    $path = SGL_MOD_DIR . "/user/classes/observers/$observer.php";
	    if (is_file($path)) {
	        require_once $path;
	        $addUser->attach(new $observer());
	    }
	}

When all observers are attached, simply run the process:

	$addUser->run();

Once the business logic in the observable event has been executed, invoke the observers with the following line of code:


	$this->notify();

and all attached listeners (observers) are notified that the trigger event has fired.

## Convention
Currently all observers are stored within the module that requires them, in a folder called 'observers' in the classes directory.
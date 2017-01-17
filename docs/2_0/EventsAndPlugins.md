---
layout: page
title: Overview Seagull 2.0
permalink: 2_0/EventsAndPlugins
---

<!-- Name: 2_0/EventsAndPlugins -->
<!-- Version: 4 -->
<!-- Last-Modified: 2010/03/26 11:20:33 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Using the Event component
* TOC
{:toc}

## Intro
One of the uber-tools components is the event handling system.  This is a very lightweight set of classes you can use in your code to create event-based systems.  Using events in your code helps keep components loosely coupled, extensible and maintainable.  It's a great way to create a plugin system.

## Registering events or hooks in your code

Registering an event in your code simply involves assigning a unique string to given point in the execution flow.  If you use a front controller or single entry point into your code, you can register most events in this class.

Examples of event names could be `core.afterRouting` or `module.beforeSave`

## Attaching listeners to the events

Events can be associated with listeners.  That means every time the event fires, the listener or listeners will be invoked.  Let's use the term __plugin__ to describe any object listening to an event.

In your application setup or bootstrap class you'll want to register all the configured plugins with event hooks.

	$disp->addEventListener('core.afterRouting', new SGL2_Task_LoadController());
	$disp->addEventListener('core.afterRouting', new SGL2_Task_CreateSession());

## Listeners can be actions or filters

Your plugins can do one of 2 things, either:

 * invoke code that performs some action
 * invoke code that transforms some input and returns it

## Example of an Action plugin

This plugin creates a session and adds it to the registry


	class SGL2_Task_CreateSession extends Uber_Plugin_Abstract
	{
	    public function handleEvent(Uber_Event $e, $data = null)
	    {
	        SGL2_Registry::set('session', new SGL2_Session());
	    }
	}

## Example of a Filter plugin

This plugin capitalises some text


	class Xmpl_Plugin_Foo extends Uber_Plugin_Abstract
	{
	    public function handleEvent(Uber_Event $e, $data = null)
	    {
	        if (! is_null($data)) {
	            $txt = $data->get();
	            $newData = strtoupper($txt);
	            $data->set($newData);
	        }
	    }
	}

## Triggering events

Now that you have your events setup, and the relevant plugins listening to them, all that remains is to invoke the event.


	SGL2_Registry::get('dispatcher')->triggerEvent(
	    new SGL2_Event($this, 'core.afterRouting')
	);

We fetch the event dispatcher from the Registry, and simply trigger the event, with __$this__ as the first argument, in case the  plugin needs to know something about the caller.  With this single call, *all* plugins listening to the event will be invoked.

## Triggering events with parameters

Let's say you have a plugin that sets up a database.  You want to reuse it for all your projects so only the parameters needs to change.  Here is an example of how to do that.


	SGL2_Registry::get('dispatcher')->triggerEvent(
	    new SGL2_Event($this, 'core.beforeRouting', array(
	        'dbName' => SGL2_Config::get('db.name'),
	        'dbUser' => SGL2_Config::get('db.user'),
	        'dbPass' => SGL2_Config::get('db.pass'),
	)));

So the optional third argument to `SGL2_Event` is a dictionary of parameters.  Here's how the plugin reads them:


	class SGL2_Task_SetupDb extends Uber_Plugin_Abstract
	{
	    public function handleEvent(Uber_Event $e, $data = null)
	    {
	        $params = $e->getParameters();
	        $dbName = $params['dbName'];
	        $dbUser = $params['dbUser'];
	        ...

## Triggering events that perform filtering

If you want your plugin to transform data, just pass a context object that implements `getData` and `setData`



	$ctx = new Xmpl_Context();
	$ctx->set('my_prop', 'this is some text');
	$e = $disp->triggerEvent(new Xmpl_Event($this, 'core.anEvent'), $ctx);        

Note that the context object is the __2nd argument__ of the `triggerEvent` method.

See *Example of a Filter plugin* above to see how the plugin transforms the data.

## Cancelling events

Sometimes a number of listeners are registered to an event like `user.onLogin`.  But if login fails, you want the execution chain for those plugins to be halted.  All you have to do is cancel the event



	class Xmpl_Plugin_Cancel extends Uber_Plugin_Abstract
	{
	    public function handleEvent(Uber_Event $e, $data = null)
	    {
	        // do something
	        $userLogin = $e->getSubject();
	        if ($userLogin->failed()) {
	            $e->cancel();
	        }
	        //  then in subsequent listeners
	        if (!$e->isCancelled()) {
	            // record login datetime
	        }
	    }
	}

## Global events

You can easily add a single plugin to all events in the system, like a logger.

* tbd

## Callback listeners

Sometimes you will have an object that contains your business logic, but does not implement `Uber_Plugin_Abstract`. Simply use the callback listener.

## Related material

 * [http://java.schst.net/EventDispatcher][1]
 * [http://components.symfony-project.org/event-dispatcher/trunk/book/01-Event-Dispatcher ][2]

## Also In This Series

 - [Overview][3]
 - [Events and Plugins][4]
 - [Autoloading][5]
 - [API Docs][6]
 - [Unit Tests][7]

[1]:	http://java.schst.net/EventDispatcher
[2]:	http://components.symfony-project.org/event-dispatcher/trunk/book/01-Event-Dispatcher%20
[3]:	/2_0/Overview.html
[4]:	/2_0/EventsAndPlugins.html
[5]:	/2_0/AutoLoading.html
[6]:	/2_0/ApiDocs.html
[7]:	/2_0/UnitTests.html
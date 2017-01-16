<!-- Name: RFC/sgl2/Event_Handling -->
<!-- Version: 1 -->
<!-- Last-Modified: 2010/03/19 12:02:20 -->
<!-- Author: demian -->
# SGL2 Event Handling

Demian presented his Uber_Tools library some time ago which comes with an event handling mechanism. For SGL2 we need also a build in event handling process, for Core (Framework) events, as well as for module specific events.

I took a look at the Zend plugin mechanism which is the equivalent to an event mechanism. The plugin base class has a set of functions which can be overwritten. For me this seems to be too inflexible.

## SGL2 Base classes

SGL2_Event (abstract)[[BR]]
SGL2_Event_Listener (abstract)[[BR]]
SGL2_Event_Dispatcher[[BR]]

In SGL2_Registry::get(‘eventDispatcher’) stays the one dispatcher instance, which is responsible for dispatching all events.
Each module directory has another folder (may be under /lib, have to think how to get auto-loading working)


    /event
    /event/listener
    /event/listener/Authorise.php
    /event/MyEvent.php

So to trigger an event within your module, you do:


    $oEvent = new MyModule_Event_ MyEvent($data);
    SGL2_Registry::get(‘eventDispatcher’)->trigger($oEvent);

To subscribe to events, I would add a module config file events.ini:


    core.beforeDispatching = Authorise
    user.createUser = SendMail

To give listeners a priority I would allow to set a value 1-99 like this SendMail(2), which controls the order, event listeners are processed.
When setting up the event dispatcher, an event map of core and module specific events has to be build, with a list of attached listeners. This array should then be cached under /var. (so during runtime only one array is read. Perhaps we can store it in db as well)

The admin module should then come with a nice event manager, which allows to easily attach/de-attach listeners to/from events, so you can control event handling. (module ini file only gives default configuration)

*A word about filters/tasks*

SGL2 code currently has tasks and filters. The code base has lot of filters, which should be tasks by the way. (create session, etc.) In SGL1 each controller could define its own filter chain. That was the only way to hook in/change the global filter chain. This only worked if the controller is called. So to hook in globally (without even your module gets called), you had to change the global filter chain in config.
This is extremely inflexible, because it breaks the module design. “Throw in your module, and it works”. The framework i.e. should not know anything about Authentication & Authorization, although it may provide base classes. So by default, there is no filter for that.

The user module will implement this process and hook itself in. So the SGL1 way would be to change the global filter chain and add the new filter. Add another module, which needs filters in the global filter chain… Yes – worst case would be, the second module will overwrite the filters from first (user) module. It will not work without manually edit the filter chain.

So I would go the event driven way, and keep the front controller to setup the minimum in code, and attach the rest as event listeners. So I merge setup tasks to “setup-events”.

The front controller setups:[[BR]]
- The global config[[BR]]
- The request object[[BR]]
- The response object[[BR]]
- The event dispatcher[[BR]]
- The router object[[BR]]

The event “beforeRouting” will trigger event listeners to setup:[[BR]]
- Database[[BR]]
- Session[[BR]]

The event “afterRouting” will trigger event listeners to setup:[[BR]]
- Translation/Locale[[BR]]

*Open questions:*

- Is there a need to add events only for a specific controller?[[BR]]
- Any other issues I have not mentioned?[[BR]]
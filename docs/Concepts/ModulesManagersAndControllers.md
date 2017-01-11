---
layout: page
title: How to setup a Virtual Host in Apache
permalink: /Concepts/ModulesManagersAndControllers.html
---

<!-- Name: Concepts/ModulesManagersAndControllers -->
<!-- Version: 8 -->
<!-- Last-Modified: 2006/04/15 14:58:00 -->
<!-- Author: demian -->

# Modules, Managers and Controllers
* TOC
{:toc}

There are a number of logical groupings in Seagull that are helpful to understand before getting down to developing with the framework.

## Modules and Managers
The majority of work done by the developer when building as Seagull-based app is in implementing his/her own modules.  A module is a logical grouping of functionality.  In the [User module][1], for example, you will find code that manages
  * users
  * preferences
  * permissions
  * roles
and so on, in other words, all aspects of user management.  In [Struts][2] terms a module is referred to as a Catalog.

A module is made up of one or more managers.  A manager is simply a page controller object which groups together a number of related actions that can be performed on a single entity or business object, for example a User, a Transaction, a ShoppingCart, etc.  Typical actions that a manager controls are:
  * add
  * insert
  * edit
  * update
  * delete
  * display

although they can be named anything you like.  

In some other projects actions are referred to as commands, in still others as events.  All managers implement the [validate/process/display][3] workflow pattern.  In PHP5 this would be done by implementing the SGL\_Manager interface which has abstract definitions for validate(), process() and display() methods.  In PHP4, effectively the same result is achieved by all manager classes extending SGL\_Manager.  Managers are an example of the [Page Controller pattern][4] and are sometimes referred to as just 'controllers' in other projects.

## Controllers
### Application Controller
So all managers, or Page Controllers, inherit from SGL\_Manager which acts as the Application Controller.  The workflow set out in SGL\_Manager is a good generic choice for most web applications.  However you are not limited to implementing the validate/process/display workflow set out by this class.  You could easily create another application controller, and have your page controllers extend that.  

A good example would be in the case of building a REST server, where no template management is needed, no display() method, just XML to PHP transformation on the input stream, vice versa on the output, and data fetching.  All permission checks could be replaced by simple HTTP authentication (see http://del.icio.us/help/api/), no session would be required, so you would end up with a very lightweight app.

### Front Controller
Seagull also uses another higher level controller that helps simplify code responsibility, the Front Controller, found in the [source:/trunk/lib/SGL] directory.  This object is responsible for handling all requests from user agents and delegating responsibility to the Filter Chain, which is a simple list of input/output filters and the target Core Processor, the above mentioned App Controller.  Various filters in the chain build and clean the request object, resolve the relevant manager and action details from the URI, execute the domain logic and finally send the appropriate headers.

The Front Controller is also responsible for initialising the Seagull environment which involves setting up paths, parsing the config, etc.

Continue reading Concepts/ValidateProcessDisplay for a workflow overview.

## See Also
 * [Concepts/BasicWorkflow][5] (outdated)
 * [Concepts/ModelViewController][6] Seagull's MVC implementation explained (outdated)

[1]:	/Modules/User.html
[2]:	http://struts.apache.org/
[3]:	/Concepts/ValidateProcessDisplay.html
[4]:	http://www.martinfowler.com/eaaCatalog/pageController.html
[5]:	/Concepts/BasicWorkflow.html
[6]:	/Concepts/ModelViewController.html
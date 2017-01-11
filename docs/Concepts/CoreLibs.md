---
layout: page
title: Library Organisation in Seagull
permalink: /Concepts/CoreLibs.html
---

<!-- Name: Concepts/CoreLibs -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/04/02 02:28:19 -->
<!-- Author: demian -->

# Library Organisation in Seagull
* TOC
{:toc}

## Introduction

The library classes used in the Seagull framework basically fall into the following groups:
  * **Controllers** - workflow
  * **Core** - general
  * **Tasks** - mostly used to do setup tasks
  * **Filters** - modify the request and response objects as they pass through the filter chain
  * **PEAR libs** - general

Developing with Seagull involves building modules that include one or more manager classes to implement business logic.  Building managers is eased by using the module creator wizard and above-listed library resources.  For a typical project you will probably only ever use the Controllers, Core and PEAR libs.

The following sections explain the library groupings and describe the basically functionality of each class.  This will be of more interest to advanced devs, beginners need not worry as default task and filters are all you'll need for basic web apps.

### Controller Classes
The controller classes manage application workflow, several examples are provided but it's very easy to write your own.

|| *Class name* || *Availability* || *Methods* || *Description* ||
|| SGL\_FrontController || default ||  \* run || The FrontController is used to organise workflow for particular application types.  You will probably use similar controllers for each application you build, although workflow can be easily customised.  See the [unit test controller][1] versus the [Seagull][2] one for ideas on how to customise. ||
|| SGL\_Manager || default ||  \* validate  \* process  \* display[[BR]]  \* maintainState || The Manager class is an app controller, it sets out generic validate/process/display workflow.[[BR]] ||
|| SGL\_Wizard || default ||  \* validate[[BR]]  \* process[[BR]]  \* display[[BR]]  \* maintainState || The Wizard class is a specialisation of the Manager class, use it to create wizard workflows in your applications.[[BR]] ||


### Core Classes
The core classes are what you use in your managers to manipulate system, application and user resources.  Availability refers to whether you need to include the class or not, anything labelled as 'default' is available within the application scope, like default classes in, eg, java.lang.  'Requirable' libs need to be included.

|| *Class name* || *Availability* || *Methods* || *Description* ||
|| SGL || default || ||Provides a set of static utility methods used by most modules ||
|| SGL\_Request || default || ||Wraps all $\_GET $\_POST $\_FILES arrays into a Request object, provides a number of filtering methods ||
|| SGL\_Output || default || || The SGL\_Ouput class is known as a TemplateHelper in other contexts, it wraps methods used by the template engine to transform the data into, eg, html.[[BR]] ||
|| SGL\_Config || default || ||Config file parsing and handling, acts as a registry for config data ||
|| SGL\_Registry || default || ||Generic data storage object, referred to as $input.  Holds references to Url, Request and Config singletons ||
|| SGL\_DB || default || ||Mostly used as a DB resource singleton, also provides dsn building method ||
|| SGL\_ServiceLocator || default || ||Facilitates switching resources at runtime, eg, so a test database resource can be swapped in for the dev one ||
|| SGL\_Url || default || ||Provides URI related functionality, derived from Net\_URL and uses a  parser strategy ||
|| SGL\_HTTP || default || ||Provides HTTP functionality including redirects ||
|| SGL\_Session || default || ||Handles session management, most methods are static ||
|| SGL\_String || default || ||Various string helper methods ||
|| SGL\_Array || default || ||Provides array manipulation methods ||
|| SGL\_Date || default || ||Provides various date formatting methods ||
|| SGL\_Inflector || default || ||Performs transformations on resource names, ie, URIs, classes, methods, variables ||
|| SGL\_Delegator || default || ||PHP 4/5 compatible class for aggregating objects ||
|| SGL\_Util || default || ||Various utility methods ||
|| SGL\_Category || requirable || ||Wrapper to SGL\_NestedSet, used to manipulate Categories ||
|| SGL\_Emailer || requirable || ||Utility class that wraps PEAR::Mail, provides convenience methods for sending HTML emails  ||
|| SGL\_TaskRunner || requirable || ||Used for building and running a task list ||
|| SGL\_NestedSet || requirable || ||A lightweight wrapper to PEAR DB\_NestedSet that bypasses DB\_NestedSet for most methods ||
|| SGL\_Download || requirable || ||Wrapper around PEAR HTTP/Download class to workaround some limits of that class ||
|| SGL\_Item || requirable || ||Acts as a wrapped for content objects ||
|| SGL\_Sql || requirable || ||Provides SQL schema and data parsing/executing methods ||
|| SGL\_Install || requirable || ||Provides various static methods required for install routine ||

### Filters
All filters are optional, configurable, and can be ordered according to need.

The process tasks below implement the [Intercepting][3] [Filter][4] pattern, each filter must extend SGL\_DecorateProcess and acts as a filter pass on the Request object.


|| *Class name* || *Availability* || *Group* || *Description* ||
|| SGL\_Process\_Init || default || pre-process || ||
|| SGL\_Process\_AuthenticateRequest || default || pre-process || ||
|| SGL\_Process\_BuildHeaders || default || pre-process || ||
|| SGL\_Process\_CreateSession || default || pre-process || ||
|| SGL\_Process\_DetectBlackListing || default || pre-process || ||
|| SGL\_Process\_DetectDebug || default || pre-process || ||
|| SGL\_Process\_DiscoverClientOs || default || pre-process || ||
|| SGL\_Process\_ResolveManager || default || pre-process || ||
|| SGL\_Process\_SetupLangSupport || default || pre-process || ||
|| SGL\_Process\_SetupLocale || default || pre-process || ||
|| SGL\_Process\_SetupPerms || default || pre-process || ||
|| SGL\_MainProcess || default || main process || ||
|| SGL\_PostProcess || default || post-process || ||
|| SGL\_Process\_SetupNavigation || default || post-process || ||
|| SGL\_Process\_SetupBlocks || default || post-process || ||
|| SGL\_Process\_SetupWysiwyg || default || post-process || ||
|| SGL\_Process\_GetPerformanceInfo || default || post-process || ||


The install tasks below are simple units of work that can optionally take an argument, ie $data.  Each must extend SGL\_Task or a specialisation of it.

### Tasks
|| *Class name* || *Availability* || *Group* || *Description* ||
|| SGL\_EnvSummaryTask || default || install || ||
|| SGL\_Task\_GetFilesystemInfo || default || install || ||
|| SGL\_Task\_GetLoadedModules || default || install || ||
|| SGL\_Task\_GetPearInfo || default || install || ||
|| SGL\_Task\_GetPhpEnv || default || install || ||
|| SGL\_Task\_GetPhpIniValues || default || install || ||
|| SGL\_Task\_SetupPaths || default || install || ||
|| SGL\_Task\_SetupConstants || default || install || ||
|| SGL\_Task\_SetBaseUrl || default || install || ||
|| SGL\_Task\_CreateConfig || default || install || ||
|| SGL\_UpdateHtmlTask || default || install || ||
|| SGL\_Task\_CreateTables || default || install || ||
|| SGL\_Task\_LoadDefaultData || default || install || ||
|| SGL\_Task\_CreateConstraints || default || install || ||
|| SGL\_Task\_VerifyDbSetup || default || install || ||
|| SGL\_Task\_CreateFileSystem || default || install || ||
|| SGL\_Task\_CreateDataObjectEntities || default || install || ||
|| SGL\_Task\_SyncSequences || default || install || ||
|| SGL\_Task\_CreateAdminUser || default || install || ||
|| SGL\_Task\_InstallerCleanup || default || install || ||

### Interfaces and Abstract Classes
  * SGL\_Manager
  * SGL\_Observer
  * SGL\_Observable
  * SGL\_View
  * SGL\_Task
  * SGL\_OutputRendererStrategy
  * SGL\_ProcessRequest
  * SGL\_DecorateProcess

[1]:	gitlilnk:/trunk/etc/sglBridge.php
[2]:	gitlink:/trunk/lib/SGL/FrontController.php
[3]:	http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnpatterns/html/DesInterceptingFilter.asp
[4]:	http://java.sun.com/blueprints/corej2eepatterns/Patterns/InterceptingFilter.html
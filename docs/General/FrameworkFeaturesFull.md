---
layout: page
title: Full List of Framework Features
permalink: /General/FrameworkFeaturesFull.html
---

<!-- Name: General/FrameworkFeaturesFull -->
<!-- Version: 9 -->
<!-- Last-Modified: 2006/03/29 00:55:45 -->
<!-- Author: demian -->
<!-- Status: In Progress -->

# Framework Features
* TOC
{:toc}

## Minimal Requirements
Recently Seagull has increased its minimum requirement to PHP 4.3.0, although you're recommended to use 4.3.11 or later to take advantage of important security fixes within PHP. The framework itself does not depend on any non-standard extensions and will run on a base install of PHP.
Currently Seagull works fine with PHP 4.3.0 and above, PHP 5, MySQL 3.23+, with register\_globals set to on or off, under Linux, Windows and Mac. Running the code in a shared hosting environment is no problem.

## One Minute Install
Installing Seagull takes less than a minute and is easy to setup for users with little experience, for more information please see the [New Installation][1] section. Also a detailed installation wizard has been implemented in the 0.5.x branch that does extensive testing of your server environment and includes many new configuration options.

## Modularity
Seagull is an OOP application with an emphasis on modularity. The framework consists of:
- **base framework**: The framework itself consists of a set of base classes organised according to the MVC design pattern that take care of permissions, authentication, sessions, i/o and database abstraction layer. Developers familiar with Struts and JSP deployment will recognise the approach
- **modules**: Each generalised area of functionality comes in the form of a module, your business requirements may be matched to modules that already exist in framework. If a module doesn't exist please request it and our community of developers will take a crack. Better yet, have your developers build it and contribute it back to the project :-)
- **libraries**: Most task-specific functionality comes from libraries, quite often from [PEAR][2], that can be independently updated when upgrades/improvements are available
- **entities/entity managers**: each object in the application (Member, Group, Property, Document, Article, etc) is represented as an entity, developers are provided with tools to quickly prototype entities so that skeleton classes are created and updated automatically

## Quality Control
Seagull has a single-committer policy which has been enforced over the last 4 years of the project's lifetime, this has made it possible to enforce consistent design and quality across the codebase. All contributor features and bugfixes are submitted as patches which are subject to peer review and if deemed valid, applied by the project maintainer.

## Stability of Codebase
The utmost importance of a stable codebase for commercial deployments is well understood and the standard steps have been taken to maximise the reliablility of the codebase:
- A large part of the codebase is covered by unit and web tests which are easy to run thanks to the integrated testing framework
- The stable release branches, even-numbered, never introduce backwards-compatibility breaks - new releases serve only to fix reported bugs and add minor features

## Consistency and Coding Standards
All the code in Seagull follows the consistent style set out in the [coding standards document][3] included in the root of each distribution. The standard follows [PEAR coding standards][4] very closely which is essentially a reiteration of the K & R guidelines from the original **C Programming Language** (1978)

## Standards Compliant
Seagull supports XHTML 1.1, CSS 2.0, RSS 1.0, and 2.0, and conditional GET for caching RSS feeds on the client-side.

## Multi Platform/DB Vendor support
Borrowing from PHP's own multi-platform flexibility, Seagull operates almost identically on Windows and Unix platforms. Thanks to the extensive use of PEAR libraries it also offers connectivity to just about any database on the market\*, including:

- Mysql
- Postresql
- MS SQL Server\*
- Oracle
- MaxDB
- DB2

\* A schema for MS SQL is under development at the time of writing, PEAR::DB is used throughout.

## Multiple Input Sources
Input data can be accepted from a number of sources, currently supported are HTTP GET/POST, shell, email, XML-RPC. SOAP support is anticipated in the near future.

## Multiple Output Formats
Using the MVC design strategy all data processing is handled in the business logic tier which is independent from the presentation and persistence layers. Switching the output format from HTML to mobile devices, WebTV, etc is as simple as creating new output templates for your desired device, and basic data can be left as is. Built-in multiple device output is supported, drivers for html output are the default.

## Data Validation
All input data to the application is filtered and validated before any processing takes place. Filters appropriate to the input mechanism are invoked before subsequent validation tests are run. In the case of HTTP input, all request data is first stripped of javascript and leading/trailing whitespace. Next a validation pass is fired for all modules that implement the SGL\_Manager interface, here you can create your custom validation tests and use libraries like PEAR's Validate for string processing. If you use the default Flexy template library all output to the browser is escaped by default.

## Front Controller
All application requests are routed through a single point of entry, index.php by default, giving developers much greater control over user interaction and minimising code duplication.

## Highly Configurable
One of the key aspects of the Seagull framework is its configurability, just about every aspect of the software can be configured by the site admin, including but not limited to choice of template engine, the database server, url handling, customising the filesystem layout, php type whether version or CLI, CGI or apache module, choice of library integration, module configuration and so on.

## Third Party Integration
Seagull currently [integrates][5] easily with popular open source projects like [FUDforum][6] and [Gallery][7]. It also provides an XML-RPC gateway and tools for rapid integration of third party services

## Skinnable Interface
The HTML presentation layer is taken care of by global and local templates so it is very easy to change the corporate logo, page header/footer, colour scheme, as well as the adding and deleting of sidebar columns. CSS stylesheets are used throughout so it is possible to substantially modify the look and feel of your project just by changing a single file. Currently you can use either the [Flexy][8] or [Smarty][9] template engines.

## Caching And Performance
Using PEAR's [Cache\_Lite][10] many aspects of output throughout the framework can be optionally cached. If you want to use Seagull for a heavy traffic site, it is recommended you use a compiler cache. There are several recommended open source and commercial products:

- [APC][11]
- [Turck MMCache][12]
- [ionCube PHP Accelerator][13]
- [Zend Optimiser][14]

## Multi-Language Support
From version 0.5.5 the framework is integrated with PEAR's [Translation2][15] library which offers extensive support for creating content in multiple languages. All aspects of the application can now be made available in multiple translations:

- The user interface
- Web site content
- Site navigation

## Localisation
See the [Localisation][16] section in the wiki.

## Authentication and Authorisation
Seagull uses standard PHP sessions which propagate persistence of user data using cookies by default. Both database and file-based persistence are supported. The PHP engine automatically detects whether the client returns session cookies, if not the session is propagated in the URL. Anti session-hijacking measures are in place to ensure the user session can not be compromised.
Seagull works identically whether or not end users have cookies enabled in their browsers.
Any module in the application can be set to require authentication by setting the 'requiresAuth' flag to true on a per-screen basis. Once users are authenticated, fine grained permissions can be controlled by testing for role membership.

## Specialised SQL Data Structures
Seagull uses nested set data structures almost throughout the framework which provide greater flexibility than the more typical adjacency list (id/parent\_id) model and allow for things like reordering nodes at n levels

## Developer Friendly
The Seagull framework aims to be developer friendly, with this in mind the following features are available:

- extensive feedback on what your code is doing through customisable debug levels
	- logging of debug messages to file, screen, database
	- windows developers are recommended to download [MSYS][17], the Unix command-line emulator. With this tool you can easily tail the log files and pinpoint bugs
- uses `DB_Dataobject` for generating application entities: the developer only has to define entity tables in database, `DB_Dataobject` does the rest
- the framework is divided into entities, manager classes and templates plus SGL and PEAR libraries, so development tasks can easily be shared and progressed in parallel
- All manager classes follow the Validate, Process, Display model so once you've developed one it's quick to recycle your work and build new modules. See the [diagrams section][18] for a visual summary of the framework workflow, or see the [Basic Workflow][19] section

## Designer Friendly
The [templating system][20] used by Seagull is based on standard HTML pages, so designers can use popular HTML tools such as Dreamweaver, Netscape Composer, or Frontpage to edit templates. Dynamic values are represented like` {this}`, methods like this `{method(arg)}` and are simple for designers to differentiate from static content.

Using the [Publisher Module][21], article authors can use the [htmlarea][22] WYSIWYG interface which allows for rich Dreamweaver-like editing functionality from within a web browser (IE 5 and up / Mozilla 1.3+, Firefox).

## Not Developed In Isolation
Considerable time has been spent studying existing PHP CMS/framework solutions in an attempt to learn from current milestones already achieved. Projects studied include, in no particular order:

- [ez Publish][23]
- [Ampoliros][24]
- [PHlexDB][25]
- [Binary Cloud][26]
- [Horde][27]
- [Logicreate][28]
- [bitflux CMS][29]
- [Geeklog][30]
- [Drupal][31]
- [Blueshoes][32]
- [pMachine][33]
- [Mamboserver][34]
- [Cake PHP][35]
- [Sitellite][36]
- [Django][37]

## Open Source Advantage
Seagull is licensed under the [BSD license][38] and offers the benefits of Open Source/Free Software:

- developers have complete access to the source code so no hidden surprises
- the codebase is scrutinised by many eyes so bugs are fixed more quickly. The project often receives 5-10 patches/day from its community of 250+ active users
- _release_ _early and_ _often_ - following the development style pioneered by Torvalds the project has consistently put out releases at least once a month for the past 4 years and currently maintains stable and development branches in svn

## Web Based Editing
All content and document management is done via the web browser, allowing you to create articles with a Dreamweaver-like interface, and upload and manage various asset types via a web forms. Currently integrated WYSIWYG editors include [FCK][39], [htmlArea][40] and [Xinha][41].

## Included Modules
Seagull has been designed with an open component architecture, so you can create and incorporate your own modules easily. Some modules available for free usage are:

- **Publisher** a content/document management solution that allows non-developers to build websites rapidly. It includes a category builder and media and document management.
- **User Manager** a module for managing users and groups, and site registration/my account
- **RSS** support for importing feeds and exporting data
- **Newsletter** A WYSIWYG newsletter designer that lets you send mailouts to your users on a group by group basis
- **Messaging** an internal messaging system like those found in many forums that lets registered users send emails to each other. All communications can be copied to the user's registered email address if desired
- **Navigation** a module that allows you to build your site navigation through a web GUI and customise its look & feel by choosing from the available stylesheets
- **Documentor** for quickly creating documentation where a table of contents links to various items and sub-items

## Content Life Cycle
Articles can be scheduled for publishing and expiration, which means you can set content to go live at a chosen date in the future, for example a press release, then have it retire automatically after a set period. Date constraints are in the 'create article' window.
end mainContent start left column

[1]:	/Installation.html
[2]:	http://pear.php.net/
[3]:	gitlink:/trunk/CODING_STANDARDS.txt
[4]:	http://pear.php.net/manual/en/standards.php
[5]:	/Integration.html
[6]:	/Integration/FUDforum.html
[7]:	/Integration/Gallery.html
[8]:	http://pear.php.net/package/HTML_Template_Flexy/
[9]:	http://smarty.php.net/
[10]:	http://pear.php.net/package/Cache_Lite
[11]:	http://apc.communityconnect.com/
[12]:	http://turck-mmcache.sourceforge.net/
[13]:	http://www.php-accelerator.co.uk/
[14]:	http://www.zend.com/store/products/zend-performance-suite.php
[15]:	http://pear.php.net/package/Translation2
[16]:	/Howto/Internationalisation.html
[17]:	http://www.mingw.org/msys.shtml
[18]:	/General/SeagullDiagrams.html
[19]:	/Concepts/ValidateProcessDisplay.html
[20]:	/Howto/Templates/WorkingWithTemplates.html
[21]:	/Modules/Publisher.html
[22]:	http://www.htmlarea.com/
[23]:	http://developer.ez.no/
[24]:	http://www.ampoliros.com/en/
[25]:	http://wiki.phlexdb.org/index.php?page=HomePage
[26]:	http://www.binarycloud.com/
[27]:	http://www.horde.org/
[28]:	http://www.logicreate.com/index.php/html/logicreate/lcapps.html
[29]:	http://slides.bitflux.ch/oscms/
[30]:	http://www.geeklog.net/
[31]:	http://www.drupal.org/node.php?title=drupal%2Bhandbook
[32]:	http://www.blueshoes.org/
[33]:	http://www.pmachine.com/
[34]:	http://www.mamboserver.com/
[35]:	http://cakephp.org/
[36]:	http://www.sitellite.org/
[37]:	http://www.djangoproject.com/
[38]:	http://opensource.org/licenses/bsd-license.php
[39]:	http://www.fckeditor.net/
[40]:	http://www.htmlarea.com/
[41]:	http://xinha.python-hosting.com/
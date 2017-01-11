<!-- Name: Howto/Navigation -->
<!-- Version: 11 -->
<!-- Last-Modified: 2007/06/27 16:38:05 -->
<!-- Author: lyric -->

# Working with Navigation
* TOC
{:toc}

## Adding a tab to the navigation

  * login as admin where you'll be forward to the "Manage Modules" screen
  * go to the Navigation section
  * click on the '`New page`' button
  * in the `title` field put 'your title'
  * the field '`page`':
	* In this section you can create navigation that references 2 types of content in the system, static or dynamic. Static content is a static HTML page entered through the Publisher module, using the article type '`Static Html Article`'.
	* To reference dynamic content, you call any of the modules in the system by selecting the caller page for that module. For example to call the FAQ module, first set the '`page type`' to '`dynamic pages`', then select '`faq.php`' from the list.
  * you may add additional parameters (do not add the ?)
  * tick the '`check to activate`' checkbox
  * in the '`can view`' combo box select the user or group you want to see this item
  * click '`save`'
  * finished

## How to Create Traditional Vertical, Left-hand Navigation
Seagull supports vertical, left-hand navigation, it's just a matter of drawing up the relevant stylesheet. We can have infinitely nested vertical menus, just as we by default have horizontal menus. Just need to tweak the css. Anyone interested might want to check out http://css.maxdesign.com.au/listamatic2/index.htm for some examples.

## Creating your own Navigation System
In Seagull a driver/renderer-pair is used to create the navigation system. The default driver is 'SimpleDriver'.
The default renderer is 'SimpleRenderer' which creates an unordered html list that is transformed by one of the several available stylesheets to give either tabs, dropdowns, etc menu styles.
To change the rendered layout you can create your own template and replace 'SimpleRenderer' by 'TemplateRenderer'. 

Alternatively, you can create your own navigation system, for example using PEAR's HTML\_Menu class or your own.  Please observe the following guidelines:

  * create your new navigation class in _/modules/navigation/classes_
  * any files where the last 3 letters are 'Nav' will be listed on the Config screen, eg, MyCustomNav.php
  * either use the 'section' table or your own if desired
  * in the global configuration file, _/etc/<hostname>.default.conf.ini_, specify the class name of your driver under the [navigation] driver key
  * make sure your class implements a render() method which uses the signature below, it's important that argument variable names are the same as they are used later on in controller.  You should *not* supply values for the parameters, they are references set by the method (see example http://uk.php.net/headers\_sent).
	  

	$nav = & new $navClass;
	$nav-\>render($sectionId, $html);
	// $sectionId - integer: id of currently selected section
	// $html - string: html to build menu

## Creating Nav Data to be Loaded with Custom Modules
It's possible to store definitions of your navigation data so it can be loaded when users install your module.  Navigation definitions are basically an array of data, and must be in a file called navigation.php located in your target module's data directory.  Here is an [source:/modules/publisher/data/navigation.php example].

There are 2 types of nodes:

  * SGL\_NODE\_ADMIN node – are added to the navigation tree with a Parent Section of 'Admin menu'
  * SGL\_NODE\_USER node – are added to the navigation tree with a Parent Section of 'User menu'

Children nodes (subsection) can be added to the above node types using the a third node type. 

  * SGL\_NODE\_GROUP node – are subsections of the node inserted previously

Using this logic you can create node groups that correspond to modules, with a SGL\_NODE\_ADMIN or  SGL\_NODE\_USER as the designated parent, and all children designated as SGL\_NODE\_GROUP nodes.

The navigation definitions are parsed by the [source:/branches/0.6-bugfix/lib/SGL/Task/Install.php#L827 SGL\_Task\_BuildNavigation] task. See instructions in the example file for using the SGL\_NODE\_ADMIN, SGL\_NODE\_GROUP and SGL\_NODE\_USER constants.

## Working with Nested Sets
The Seagull section table that the navigation module uses is built with nested sets, this is a more sophisticated way of storing a node hierarchy in a database, that allows you to do things like reordering nodes at a given hierarchy depth.  For more info on nested sets, see:

 * [Storing Hierarchical Data in a Database by Gijs Van Tulder][1]
 * [Trees in SQL by Joe Celko ][2]

## See Also

[[SubWiki(Howto/Navigation)]]

[[AddComment]]

[1]:	http://www.sitepoint.com/article/hierarchical-data-database
[2]:	http://www.intelligententerprise.com/001020/celko.jhtml;jsessionid=PHJPZUZQLLB52QSNDLRCKHSCJUNN2JVN?_requestid=880821
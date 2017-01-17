<!-- Name: Integration/Serendipity -->
<!-- Version: 9 -->
<!-- Last-Modified: 2006/11/30 15:53:13 -->
<!-- Author: demian -->
# Integrating the Serendipity Blog
[Serendipity][1] is widely recognised as being one of the best blogs available in PHP, perhaps in any language.  Now you can use it in your Seagull installation, here's how:

## Installation
 * install s9y in seagull/www/serendipity
 * configure and setup the app as normal with the following exceptions:
   * In Appearance and Options -\> Is serendipity embedded?  select 'yes'
   * set Paths -\> Index file  to '../index.php/blog/'
 * login to s9y as admin user and select 'Configure Plugins'
 * set all 'Sidebar Plugins' to 'hidden' (remember admin link is serendipity/serendipity\_admin.php)
 * add a couple of test articles
 * register 'blog' module in seagull (at the time of writing this means manually add a record to the 'module' table, this will be automated shortly)
 * modify seagull/www/serendipity/templates/default/index.tpl and comment out <div id="serendipity_banner">, your own blog heading can be set in seagull/modules/blog/conf.ini

## Customising
A standard Seagull Flexy template is used to wrap the s9y output, so you can customise the look and feel.

## Integrating s9y plugins into Seagull Blocks
One of the most visible actions when using the 'embed' option is that the plugin panes are not generated. And HTML headers/footers are also not provided by s9y in this case.

So you have to include them in your BlogMgr.php file, to have them visible in your embedded setup.

Include the file, serendipity\_config.inc.php, and use this snippet:


	<?php
	serendipity_plugin_api::generate_plugins('left','div');
	serendipity_plugin_api::generate_plugins('right','div');
	?>

## Improvements
This integration could be improved, see Garvin's comments here:

http://www.seagullproject.org/forum/index.php?t=msg&th=154

--Demian

[1]:	http://www.s9y.org/
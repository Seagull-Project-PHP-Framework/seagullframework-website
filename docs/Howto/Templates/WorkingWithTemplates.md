<!-- Name: Howto/Templates/WorkingWithTemplates -->
<!-- Version: 27 -->
<!-- Last-Modified: 2008/04/21 11:25:00 -->
<!-- Author: demian -->
# How to use Templates
[[TOC]]

## Intro
Seagull by default uses the [Flexy][1] template engine, though it is possible to swap in your preferred engine, especially if you are going to create your own theme.

## Main Concepts
  * Seagull follows a well-established tradition of minimalising the amount of business logic that is dealt with in the presentation layer - ideally it should be none or very little.  By keeping the data transformation in the business logic layer (the Model of Model View Controller), data can be sent to the presentation layer (the View of MVC) and marked up according to any number of themes (a look and feel scheme), and targetted for a range of user agents (browser, PDA, etc.).  Currently Seagull supplies two themes which, using Flexy, are tranformed into XHTML output for Web browsers.
  * A theme is comprised of a collection of folders, each of which contains the HTML templates for the module it represents.
  * Flexy compiles all HTML templates into PHP scripts that are never touched by the developer.

## Simple example template syntax
Simple template syntax with Flexy, this construct iterates through a collection of user objects:


	<tr class="{switchRowClass()}" flexy:foreach="results,key,valueObj">
	    <td align="center"><input type="checkbox" name="frmDelete[]" value="{valueObj.id}"></td>
	    <td>{valueObj.id}</td>
	    <td>{valueObj.username}</td>
	    <td>{valueObj.link_gid.name}</td>
	    <td>{valueObj.email}</td>
	</tr>


## Creating your own Theme
In order to create your own theme the easiest approach is to create a new directory in the 'themes' folder and copy the templates for the default module into your new folder.  

  * Create a new themes folder:


	seagull/www/themes/myTheme
  * Copy the default module's templates from:


	seagull/modules/default/templates

into your new theme directory:


	seagull/www/themes/myTheme/default

 * Also copy the _css_ and _images_ directories into your theme dir as these are not affected by seagulls automatic fallback to the default theme.

Remember you don't have to include the whole _default_ directory, only the files you plan on changing.

The most visible changes then can be implemented by simply changing the header and footer templates, and modifying the stylesheet.  These default templates exist in the 'default' templates folder:
  * master.html: the main template, including the others.
  * header.html: contains all the HTML from the open <html> tag, the <head> tag, up until where the <body> tag starts.
  * banner.html: contains all the HTML from the open <body> tag until the end of the page header info, ie. the area that you want to display at the top of all your pages.
  * footer.html: everything from the ' Powered by Seagull Framework' downwards, ie., any footer info you have.

## Enabling Your New Theme
The default theme, ie, what anonymous user browsing your site see, is a config setting in the 'general' section.

## Conventions
 * you can include templates within templates


	\<flexy:include src="banner.html" /\>

 * you can loop through arrays within HTML tags, ie.


	\<tr flexy:foreach="aPagedData[data],key,aValue"\>

 * call functions


	<th>{translate(#Role#)}</th>

 * call in site config hash and conditionals


	{if:conf[OrgMgr][enabled]}

See Flexy docs for full details.

Also, Seagull uses a convention of sending form objects to PHP as associative arrays:

	<tr>
	    <td class="fieldName">{translate(#First Name#)}</td>
	    <td align="left"><input name="user[first_name]" type="text" value="{user.first_name}" /></td>
	</tr>
	<tr>
	    <td class="fieldName">{translate(#Last Name#)}</td>
	    <td align="left"><input name="user[last_name]" type="text" value="{user.last_name}" /></td>
	</tr>

then casting them to objects in the script:

	$input->user = (object)$req->get('user');

## Theme Inheritance
It's important to mention that Flexy allows your themes to observe an inheritance model, in other words, if you only want to change the headers and footers, just define those templates in your new theme, and they will override the ones set in 'default'.  Because none of the other templates are present in your theme, the ones from 'default' will be used.

The following code in FlexyStrategy.php shows the theme inheritance rules for Flexy templates, the first element in the list has the highest precedence:


	$options = array(
	                       // the current module's templates dir from the custom theme
	'templateDir'       => SGL_THEME_DIR . '/' . $data->theme . '/' . $data->moduleName . PATH_SEPARATOR .
	
	                       // the default template dir from the custom theme
	                       SGL_THEME_DIR . '/' . $data->theme . '/default'. PATH_SEPARATOR .
	
	                       // the current module's templates dir from the default theme
	                       SGL_MOD_DIR . '/'. $data->moduleName . '/templates' . PATH_SEPARATOR .
	
	                       // the default template dir from the default theme
	                       SGL_MOD_DIR . '/default/templates',


## Setting the theme on a per-manager basis
As of Seagull 0.6.3 you can set the theme per manager with



	[FooMgr]
	requiresAuth     = false
	showUntranslated = false
	theme = my_theme


## Modifying Translated Strings
Any string that is translated in the GUI, in the format


	<th width="5%">{translate(#disable#)}</th>

can also be modified by specifying a PHP function as the second argument, for example:


	<th width="5%">{translate(#disable#,#ucfirst#)}</th>



## Extensibility
You can define your own custom methods for use in templates, see Howto/Templates/Flexy/Plugins.  Don't forget to consult [Alan K's complete test suite for Flexy][2].

NB: thanks to Harry who's put together a quite complete list of [PHP template engines][3].

## Debugging
A very useful trick for seeing which output was sent to the template is the following:

 * to see the whole output object, can be placed anywhere in the template


	{t:r}

 * to examine a particular scalar, array or object variable, place after variable is initialised, ie, in a foreach loop:


	{myObject:r}


## Using Variables in Language Strings
New in the 0.6 version is the *printf method* that can be called in any template.

Usage examples:

	{printf(#User %s registered successfully. The password sent to email %s#,user[name],user[email])}
or the same approach can be used with the *translation method*.  Given the key


	'user registered successfully' => 'User %s registered successfully. The password sent to email %s'
or

	'user registered successfully' => 'User %1 registered successfully. The password sent to email %2'
or - the best and easiest to read method for translators

	$words['user %name registered successfully. The password sent to email %email'] =
	    'User %name registered successfully. The password sent to email %email';
you would call:

	{translate(#user %name registered successfully. The password sent to email %email#,#vprintf#,user)}
where *user* is an array of arguments you want to be replaced in your translation string e.g. $user['name'], $user['email']

## Javascript
Flexy doesn't step into `<script>` blocks. You can find a workaround at [http://blog.iworks.at/?/archives/2\_Javascript\_\_Flexy.html] 

You can use the `<flexy:toJavascript>` tag as discussed in the [Flexy manual][4].
Unfortunately you cannot pass flexy functions (like `translate()`) to `flexy:toJavascript`. In this case you have to use the following workaround if you want to translate error alerts:


	<div id="alertPleaseWait" class="hide">{translate(#Please wait while document uploads#)}</div>
	<script type='text/javascript'>
	    /* alert messages: */
	    var alertPleaseWait = document.getElementById('alertPleaseWait').innerHTML;
	 </script>

## Problems and Solutions
### PHP5 and overloading (Flexy)
If you use PHP5 you may be tempted to do neat things with objects using overloading...


	PHP5 class
	
	class ModernObject {
	    private $mySecret = array();
	
	    public function __get($key) {
	        if (isset($this->mySecret[$key])) {
	            return $this->mySecret[$key];
	        }
	    }
	
	    public function __set($key, $value) {
	        $this->mySecret[$key] = $value;
	    }
	
	    public function get($key) {
	        if (isset($this->mySecret[$key])) {
	            return $this->mySecret[$key];
	        }
	    }
	}
	
	
	
	PageController action
	
	function _cmd_list(&$input, &$output) {
	
	    $mo = new ModernObject();
	
	    //  works via ModernObject::__set()
	    $mo->objOne = new BusinessFormatter();
	    $mo->objTwo = new FunnyLibrary();
	
	    $output->mo = $mo;
	}

Now let's have a look at what we can do with our shiny new class...[[BR]]

PHP5 lets you do things like...

	$mo->get('objOne')->methodOfObjOne();

Flexy not...

	{mo.get(#objOne#).methodOfObjOne()}
	<!-- results in a fatal error: unexpected something: () -->

Hah, overloading to the rescue you might think, since accessing object attributes always worked... 

	{mo.objOne.anyMethod()}

... but unfortunately it doesn't, in fact anyMethod() will never be called. So why?[[BR]]

Looking into our compiled template we find code like this (word-wrapped):

	<?php 
	
	if ($this->options['strict']
	|| (isset($t->mo->objOne) && method_exists($t->mo->objOne, 'anyMethod')))
	    echo $t->mo->objOne->anyMethod();
	
	?>

That's sort of expectable you may say but the key point is that isset($t-\>mo-\>objOne) results in _false_ due to the fact that ModernObject has no real public attribute 'objOne', thus prevents anyMethod() from getting invoked.

To make things work we need to provide a third method in class ModernObject:

	public function __isset($key) {
	    return isset($this->mySecret[$key]);
	}

Last downside of this method is that !__isset() (and !__unset()) aren't available until PHP 5.1.0.

[1]:	http://pear.php.net/manual/en/package.html.html-template-flexy.php
[2]:	http://cvs.php.net/pear/HTML_Template_Flexy/tests/templates/
[3]:	http://wact.sourceforge.net/index.php/PhpTemplateEngines
[4]:	http://pear.php.net/manual/en/package.html.html-template-flexy.attribute.tojavascript.php
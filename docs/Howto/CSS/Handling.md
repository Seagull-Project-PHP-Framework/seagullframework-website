<!-- Name: Howto/CSS/Handling -->
<!-- Version: 3 -->
<!-- Last-Modified: 2007/02/14 15:53:08 -->
<!-- Author: demian -->
<!-- Status: Original -->

# How Seagull handles CSS
* TOC
{:toc}

## Intro

Seagull CSS stylesheets are written as PHP files.  That is, rather than having a file named _stylesheet.css_, you have a file named _stylesheet.php_.

This offers several advantages over a normal CSS file:
  * Ability to do PHP variable substitutions that can be used in the actual css code.
  * Ability to modify simple colour scheme variables, ie, primary/secondary/tertiary colours, and quickly effect an overall colour scheme change.
  * Ability to sniff the browser-agent string and customize CSS code (if you desire).
  * Provides these advantages without adding processing overhead or increasing bandwidth utilization (see explanation below).

Effectively the stylesheet.php behaves just like a typical stylesheet.css because of added logic for checking 'Last-Modified' headers.  All files served by Apache have a 'Last-Modified' timestamp header.  This information is echoed back to Apache on subsequent requests by the client browser.  For subsequent requests, the php script verifies that the css file hasn't been modified since the last time it provided the file to the client, and simply responds with the 'HTTP/1.x 304 Not Modified' header (which tells the client browser to use the already cached copy).  If the stylesheet has been modified, the new version of the file is served to the client.

## Overview

The base seagull stylesheet is split into smaller pieces. The base stylesheet is:
  * style.php, the container
  * vars.php, where php variables are defined
  * core.php, the main css code
  * $navigation.php, the navigation stylesheet 

Plus you can load a specific stylesheet, for only the module you are actually using.

## Defaults, eye candy

We have centered and borderless tables as default, so you don't have to pollute the html code with border="0", align="center" ,cell{padding,spacing} stuff everywhere.

Then we have some helpers like the error class that set the colour to red which could save you from the style="color: red" or style="color: #ff0000" ugliness. We have three classes full, wide and narrow which sets: 100%, 90% and 60% so you can save some width="foo" and use them. Images are borderless by default.

At last, in the eye candy camp we have the "simulate a button" effect in CSS, you could see it in action in the login/logout button.

## Navigation menu stylesheets
Seagull permits you to flawlessly add navigation menus stylesheets. Navigation menus are coded as standard unordered lists. The code is pretty straightforward, you can see as an example SglDefault\_Twolevel.css and SglDefault\_Multilevel.css for horizontal menus and SglListamaticSubtle.css for a vertical one. The only helper Seagull provides is a .current class to the current page path elements.

## Module specific CSS

You can load CSS for a specific module by creating a file named after the module in www/themes/default/css. For example if you wanted to load CSS only for the Faq module you would create the file faq.php in www/themes/default/css.
 
 * *Note:* as of version 0.6.2 of Seagull you can use SGL\_Output#addCssFile('myFile.css') to dynamically add a CSS file on a manager or action-specific basis



		function _cmd_list(&$input, &$output)
		{
		    SGL::logMessage(null, PEAR_LOG_DEBUG);
		
		    $output->addCssFile('foomgr-myaction.css');
		}

 * *Note:* as of version 0.6.2 of Seagull you can reinstate the dynamically loaded CSS file if you supply it as a post param in your forms.  eg:



	<form id="foo">
	    <fieldset class="hide">
	        <input type="hidden" name="redirMod" value="default" />
	        <input type="hidden" name="redirMgr" value="service" />
	        <input type="hidden" name="redirTpl" value="serviceDetail.html" />
	        <input type="hidden" name="contentId" value="{contentId}" />
	        <input type="hidden" name="cssFile" value="foomgr-myaction.css" />
	    </fieldset>
	</form>

As long as the param name is 'cssFile' it will get automatically re-added to $output if validation fails.


## Customization

With the CSS modularity introduced in Seagull 0.5.1, maintaining a mid customized tree is really easy. If you only need color changes you can easily swap the default vars.php with yours, the same works for the navigation stylesheet. If you need an higher level of customization but you don't want to have to resync with each seagull change you can easily require from style.php each stylesheet you need. Have fun!

## Coding standards
Coding standards are pretty simple:
  * indent with four spaces
  * no empty lines between code of the same block
  * no trailing whitespaces
  * Camel for php variables name

i.e.:

	$primaryTextLight      = '#ffffff'; // white
	
	#sgl-header-right a {
	    padding: 0 5px;
	    text-decoration: none;
	    color: <?php echo $primaryTextLight ?>;
	}
	#sgl-header-right a:hover {
	    text-decoration: underline;
	}

[[AddComment]]
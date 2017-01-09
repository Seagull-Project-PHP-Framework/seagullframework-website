<!-- Name: Howto/Templates/Smarty -->
<!-- Version: 12 -->
<!-- Last-Modified: 2007/12/02 21:51:35 -->
<!-- Author: demian -->
# Using Smarty as your Template Engine
[[TOC]]

## Overview
Thanks to Malaney J. Hill we now have the [Smarty](http://smarty.php.net/) template engine integrated into Seagull.  To use simply download the latest Smarty package, unzip and install in seagull/lib/other/ removing the version number.  You should have


    /path/to/seagull/lib/other/Smarty

Then in Config select 'Smarty' from the 'Template Engine' dropdown.  This will use the templates from the Smarty theme:

    /path/to/seagull/www/smarty

Another idea, very easy to implement, would be to use the pearified.com version of Smarty (http://pearified.com/index.php?package=Smarty) which would allow for easier updates and maintenance.  Doing so would require modification of the Smarty include paths in 


    /path/to/seagull/lib/SGL/HtmlSmartyRendererStrategy.php

or as of Seagull 0.6.0 in 


    /path/to/seagull/lib/SGL/HtmlRenderer/SmartyStrategy.php

## Tips
If you want to get full error messages, comment out the following lines in lib/SGL/HtmlRenderer/SmartyStrategy.php :


    SGL::setNoticeBehaviour(SGL_NOTICES_DISABLED);
    SGL::setNoticeBehaviour(SGL_NOTICES_ENABLED);


## Using Smarty without changing the default renderer

It could be useful if: 

 * You have an existing application using Smarty and you want to convert it to a Seagull module without changing the default template engine to Smarty
 * You want to make your module using Smarty because you feel more comfortable using Smarty than the default template engine

1. Make sure that the file masterSmarty.html exists in www/themes/default directory and that it contains:


    <flexy:include src="header.html" />
    <flexy:include src="banner.html" />
    <flexy:include src="blocksLeft.html" />
    
    <div id="content">
    {outputBody(#smarty#)}
    </div>
    
    <flexy:include src="blocksRight.html" />
    <flexy:include src="footer.html" />

2. Place this line in the constructor of your module manager:


    $this->masterTemplate     = 'masterSmarty.html';

3. Put Smarty templates for your module in


    /path/to/seagull/smarty/yourmodule/

4. Select the template in your module manager as usual with


    $output->template = 'yourtemplate.html';

5. Variables assigned to the output object with


    $output->yourVariable = 'Hello World!';

can be used inside the Smarty templates via


    {$result->yourVariable}

The methods of the output object can be called like this


    {$result->makeUrl("youraction","yourmanager", "yourmodule")}


And that's all. Now you can use Smarty templates inside your module without changing the default theme manager.

## Assigning values directly to Smarty

When porting an existing Smarty-based application to Seagull it saved me a lot of time to assign output values directly to the Smarty instance instead of adding them to the Seagull output object.


    $smarty->assign('yourVariable', 'Hello Wold!');

This is the usual way how Smarty-based applications do their output. Therefore neither the templates nor the existing code fragments have to be changed for using them in a Seagull manager.

To be able to do this you need access to the Smarty instance. You can obtain the instance from the class SGL_Smarty which is part of the output renderer strategy for Smarty.


    require_once SGL_LIB_DIR . '/SGL/HtmlRenderer/SmartyStrategy.php';
    $smarty = & SGL_Smarty::singleton();

I'm not entirely sure if this undermines other concepts or features of Seagull. But for porting existing applications it may be a great help. For new applications I suggest using the Seagull output object. Both output techniques can be used in parallel when working with Smarty.

## See Also
 * [Smarty documentation](http://smarty.php.net/docs.php)
 * [source:trunk/etc/flexy2SmartyRunner.php]
 * [source:trunk/etc/Flexy2Smarty.php]

[[AddComment]]
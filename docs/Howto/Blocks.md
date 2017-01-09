<!-- Name: Howto/Blocks -->
<!-- Version: 26 -->
<!-- Last-Modified: 2007/07/18 10:35:11 -->
<!-- Author: demian -->
= Working with Blocks = 
[[TOC]]
## What is a block?
A block is a snippet of code that gets called from your template files.  All the templates that make up a page are laid out in the [browser:/branches/0.6-bugfix/modules/default/templates/master.html master template].  As you can see by browsing this directory, there are a number of master templates you can use, however most developers will create their own to suit their needs.  Examples of templates that call blocks are [browser:/branches/0.6-bugfix/modules/default/templates/blocksLeft.html blocksLeft.html] and [browser:/branches/0.6-bugfix/modules/default/templates/blocksRight.html blocksRight.html].

Seagull uses blocks to provide a flexible way of including dynamic content, as in the “boxes” in the right and left hand columns, navigation menu or breadcrumbs.  Block can be constrained to sections, (login as admin and choose Navigation), and limited to members of specific roles.  You can reorder blocks and disable them when you need to.  As a matter of convention, if a block is associated with the functionality of a module, it is stored in that module, eg 'latest articles' block can be found in the publisher module, in blocks directory.

## Activating an existing block
In order to create a new block to appear in the left or right columns of your Seagull project, you need to do the following:

  1. Log in as admin and select the *Blocks -> Manage Blocks*
  1. By hitting 'New Block' you can add a block.
  1. Fill in the relevant details for the name and title of your block. You can choose any of the existing Block Class Names for now.
  1. The title and body class fields allows you to designate CSS classes to your block - if you leave these blank the global block CSS classes will be applied.
  1. By hitting the 'publish' tab, you can choose the sections, roles and position where the block will appear in.
  1. Some blocks let you add some block specific parameters, you will get an extra tab if this is the case.
  1. Hit 'submit' and the block details will be saved. If you checked the "activate" box your new block will be visible now for the roles you chose and in the sections you specified.

See [An Example of Activating an Existing Block](/wiki:Howto/Blocks#Anexampleofactivatinganexistingblock/)

 
Some blocks you may want to try that are provided with Seagull but not activated by default:
  * LoginBlock
  * CalendarBlock


## Creating your own blocks
  To create your own custom block, there are several steps that must be followed. (_These instructions were written referencing 0.6.x_)
  
### Class Creation
  1. First, you need to create a class for your block. For this example, we'll be using a block called "PhotoGallery".
    1. Go to ''modules/<module name>/blocks'' and create a new block class, where <module name> is the module of which this block will be a member. 
    1. The class filename should be the name of your block, and the class name should be ''<module name>_Block_<block class name>''
    1. You must have an init() method, which returns the output of a getBlockContent() method. The getBlockContent() method can return an HTML or text string directly, as in the Sample1 block, or it can call another method to retrieve its content, as we'll see in the example.
    ''Note that when the blocks are being loaded, they are passed several arguments that can be used in your block. (See Block Arguments for more information)''

The easiest way to get this structure right is to copy one of the existing classes, say [browser:/branches/0.6-bugfix/modules/default/blocks/Sample1.php Sample1.php]. However, this sample leaves out many of the possible features of block programming, so let's go through those.

### Block Arguments
When the blocks are being loaded by the [browser:/branches/0.6-bugfix/lib/SGL/BlockLoader.php BlockLoader] during processing, they are passed several arguments that can be used in your block. 

  1. The $output object (passed by reference; the object that is being built for display)
  1. The $block_id (passed by value; simply the ID of the block as stored in the DB)
  1. The $aParams array (passed by reference; if your block has any additional parameters, see _Block Additional Parameters_)
    
### Block Additional Parameters
When creating blocks, you can specify additional parameters the block should have access to in a file called _<block name>.ini_.
When the block is loaded, they will be pulled from the file and used in the block's creation, and also serialized and stored in the database. At that point these parameters will be editable from the Admin interface; you will see a _Block Parameters_ tab when editing this block there.

Generally there should be a [details] section, where you can enter the descriptive name, description, author, date created, version, and copyright, if any.

Any other sections will be additional parameters for your block, which can be accessed by calling _$aParams['sectionname']_ in your block code. These sections should take type, value, label, and description.

  * Type lets you choose whether you want a text field, dropdown, checkbox, or radio button form control in the Admin area. 
  * Value is the default value for this parameter.
  * Label will be the text displayed on the Block Parameters tab in the Admin area.
  * Description will be shown in the tooltip text

#### Example
_Note, the template parameters created in this example are for demonstration purposes; the template and template path don't necessarily need to be kept in parameters._

    PhotoGallery.ini
    
    [details]
    name        = "Photo Gallery Block"
    description = "Show random photos from a database"
    author      = "Clay Hinson"
    created     = "2006-09-15"
    version     = "0.5"
    copyright   =
    
    [blockTemplate]
    type        = "text"
    value       = "photoGallery.html"
    label       = "Template file for block"
    description = "File that should be loaded for the block content"
    
    [blockTemplatePath]
    type        = "text"
    value       = "default"
    label       = "Template file path"
    description = "Path to search for the template file"
    

### Custom Block Code Example

Our example block, PhotoGallery,  is in the default module, so the filename will be _modules/default/blocks/PhotoGallery.php_. The classname is _Default_Block_PhotoGallery_.


    
    class Default_Block_PhotoGallery
    {
        function init(&$output, $block_id, &$aParams)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
           
            return $this->getBlockContent($output, &$aParams);
        }
    
        function getBlockContent(&$output, &$aParams)
        {
            // set up the block output object
            $blockOutput            = new SGL_Output();
            $blockOutput->webRoot   = $output->webRoot;
            $blockOutput->imagesDir = $output->imagesDir;
            $blockOutput->theme     = $output->theme;
    
            [... block php code - build the photo gallery html ...]
    
            return $this->process($blockOutput, &$aParams);
        }
        
        function process(&$output, &$aParams)
        {
    	    
            // set template path and file
            $output->moduleName     = $aParams['blockTemplatePath'];
            $output->masterTemplate = $aParams['blockTemplate'];
    
            // output the HTML
            $view = new SGL_HtmlSimpleView($output);
            return $view->render();
        }
    }

### Create a Block Template
Your block not only needs a class, but also a template if you are going to be building the HTML dynamically. This file is stored in _/www/themes/<mytheme>/<module name>/<block name>.html_ for our example block. As shown above, the block is told the location of this file in the block parameters, so you can technically store it anywhere in your own theme or in the default theme.


    {foreach:resultValues,key,aValue}
    <div class="photo">
    <a href="{webRoot}/themes/{theme}/images/photos/{aValue[image_name]}" target="_blank">
    <img src="{webRoot}/themes/{theme}/images/photos/thumbnails/{aValue[image_name]}" height="{aValue[thumb_height]}" width="{aValue[thumb_width]}" border=0></a>
    <div class="caption small">{aValue[caption]:h}</div>
    </div>
    {end:}

### Using the Block
Now that you have created your block's class and optional parameters file, you can follow the steps in [Activating an existing block](/wiki:Howto/Blocks#Activatinganexistingblock/) above, or follow the example below.  

== An example of activating an existing block == 
Let's try and implement a Breadcrumbs block, changing position and layout:

We follow the steps above for adding a new block:

  1. click on add new block under the Manage blocks section.
  1. Let's name it breadcrumbstest and choose the Navigation_Block_Breadcrumbs class.
  1. Under the publishing tab change the position to MainBreadcrumb.
  1. Check the “activate” box and click submit.


We have created a block that uses the Breadcrumbs class included in the Navigation module, and we have placed it under the MainBreadcrumb “slot”.

You can customise block positioning by defining slot names in ary.blocksNames.php.

We can choose the positioning of the MainBreadcrumb slot, for example in your banner.html template file, by adding the following lines:



    {foreach:blocksMainBreadcrumb,key,valueObj}
        {valueObj.content:h}
    {end:}

We can customize the breadcrumbs html template for our theme:

  1. create a navigation folder in your current theme if you don't have one already.
  1. copy the Breadcrumbs.html file from modules/navigation/templates and copy it in your navigation folder.
  1. Make the changes you need.


## Adding a block to the top or bottom of the page
Simply create a block and assign it to 'top' or 'bottom' and then in your templates add the code below.



    {foreach:blocksBottom,key,valueObj}
        <div class="block">
            <div class="header">
                <h2>{valueObj.title}</h2>
            </div>
            <div class="content">
                {valueObj.content:h}
            </div>
        </div>
    {end:}

_Note: if you want a block to not be displayed at all if its content is empty, rewrite the block template as follows:_

    {foreach:blocksBottom,key,valueObj}
        {if:valueObj.content}
            <div class="block">
                <div class="header">
                    <h2>{valueObj.title}</h2>
                </div>
                <div class="content">
                    {valueObj.content:h}
                </div>
             </div>
        {end:}
    {end:}

## Creating custom block positions
 * *since Seagull 0.6.3*
By default all blocks created expect to be assigned to set positions within the page layout.  The position template can be found in 


    seagull/lib/data/ary.blocksNames.php

The block positions are called by the master template, which by default is located in 


    seagull/modules/default/templates/master.html

In order to create your own master layout, simply create your own theme, and in a subfolder called 'default' create your own file called master.html, eg


    seagull/www/themes/my_theme/default/master.html

You can override the block layout positions and create your own just by setting a global variable.  Here's an example:


    $GLOBALS['_SGL']['aBlocksNames'] = array(
        'Right'=> 'Right',
        'Left' => 'Left',
        'Top' => 'Top',
        'Bottom' => 'Bottom',
        'BodyTop' => 'BodyTop',
        'AdminCategory' => 'AdminCategory',
        'AdminNav' => 'AdminNav',
        'AdminBreadcrumbs' => 'AdminBreadcrumbs',
        'MainNav' => 'MainNav',
        'SearchBar' => 'SearchBar',
        'MainBreadcrumb' => 'MainBreadcrumb',
        );

In order for this global array to be loaded on every request either define it in a custom config file in Config:


    $conf['path']['pathToCustomConfigFile']

or define it in a init.php file in the root of any of your custom modules, ie


    seagull/modules/my_module/init.php





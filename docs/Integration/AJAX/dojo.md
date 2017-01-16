<!-- Name: Integration/AJAX/dojo -->
<!-- Version: 15 -->
<!-- Last-Modified: 2009/08/19 04:32:08 -->
<!-- Author: malber -->

# dojo Toolkit
[[TOC]]

## What is Dojo?

Dojo is the Open Source JavaScript toolkit that helps you build serious applications in less time. It fills in the gaps where JavaScript and browsers don't go quite far enough, and gives you powerful, portable, lightweight, and tested tools for constructing dynamic interfaces. Dojo lets you prototype interactive widgets quickly, animate transitions, and build Ajax requests with the most powerful and easiest to use abstractions available. These capabilities are built on top of a lightweight packaging system, so you never have to figure out which order to request script files in again. Dojo's package system and optional build tools help you develop quickly and optimize transparently.

Dojo also packs an easy to use widget system. From prototype to deployment, Dojo widgets are HTML and CSS all the way. Best of all, since Dojo is portable JavaScript to the core, your widgets can be portable between HTML, SVG, and whatever else comes down the pike. The web is changing, and Dojo can help you stay ahead.

Dojo makes professional web development better, easier, and faster. In that order.

## Standard Installation

  * Download - http://dojotoolkit.org/download/
  * Decompress in <install-root>/www/js
  * Rename folder the decompressed folder (dojo-0.x.x) to dojo

## The Package System and Custom Builds
*A Dojo custom build speeds performance by doing the following:*
  * First, it groups together modules into layers. A layer, which is one big .js file, loads faster than the individual .js modules that comprise it
  * Second, it interns external non-JavaScript files. This is most important for Dijit templates, which are kept in a separate HTML file. Interning pulls the entire file in and assigns it to a string.
  * Third, it smooshes the layer down with ShrinkSafe. ShrinkSafe removes unneeded whitepsace and comments, and compacts variable names down to smaller ones. This file downloads and parses faster than the original.
  * Finally, it copies all non-layered scripts to the appropriate places. While this doesn't speed anything up, it ensures that all Dojo modules can be loaded, even if not present in a layer. If you use a particular module only once or twice, keeping it out of the layers makes those layers load faster.

### Step by Step
  1. Download a source build of Dojo, which you can obtain at [http://download.dojotoolkit.org/current-stable/]. The source builds are suffixed with "-src".
  1. Decompress into a work directory, i.e. _/tmp/dojo-release-1.3.2-src_
  1. Change directory to _/tmp/dojo-release-1.3.2-src/util/buildscripts_
  1. Assuming a Linux system, run the following: _./build.sh profile=standard optimize=shrinksafe.keepLines version=1.3.2 cssOptimize=comments.keepLines cssImportIgnore=../dijit.css action=release mini=true copyTests=false_
  1. When the build is complete there will be a _/tmp/dojo-release-1.3.2-src/release_ directory, containing a _dojo_ directory.
  1. Copy _dojo_ into _<install-root>/www/js_  

By using the _standard_ profile this will generate the standard Dojo package (see Standard Installation). To create a custom package, use _util/buildscripts/profiles/standardCustomBase.profile.js_ as a template to create a custom profile, ie. _fooCustom.profile.js_. The following will include some basic form widgets in the dojo.js, this will reduce the number of file requests, speeding up a page's load time.


	dependencies = {
	    layers: [
	        {
	            name: "dojo.js",
	            customBase: true,
	            dependencies: [ 
	                    "dijit.form.Form",
	                    "dijit.form.Button",
	                    "dijit.form.Checkbox",
	                "dijit.form.Menu",
	                    "dijit.form.ValidationTextBox", 
	                "dijit.form.NumberTextBox", 
	                "dijit.form.CurrencyTextBox", 
	                "dijit.form.FilteringSelect",
	                "dijit.form.DateTextBox", 
	                "dijit.form.TimeTextBox", 
	                "dijit.Dialog", 
	                "dijit.Tree",
	                "dijit.ProgressBar",
	                "dijit.layout.ContentPane", 
	                "dijit.layout.LayoutContainer", 
	                "dijit.layout.StackContainer",
	                "dijit.layout.TabContainer"
	             ]
	        },
  * Run the following: _./build.sh profile=fooCustom optimize=shrinksafe.keepLines version=1.3.2 cssOptimize=comments.keepLines cssImportIgnore=../dijit.css action=release mini=true copyTests=false_

## Integration
### Templates
#### Dojo CSS
For [source:trunk/modules/default/templates/header.html]

	
	{makeCssOptimizerLink():h}
	 
	{if:isDojoEnabled}
	    <!-- Dojo -->
	    <link rel="stylesheet" type="text/css" media="screen" href="{webRoot}/js/dojo/dojo/resources/dojo.css" />
	    <link rel="stylesheet" type="text/css" media="screen" href="{webRoot}/js/dojo/dijit/themes/{dojoTheme}/{dojoTheme}.css" />
	    <!-- custom overrides of dojo css -->
	    <!-- <link rel="stylesheet" type="text/css" media="screen" href="{webRoot}/themes/{theme}/css/dojo.css" />  -->
	{end:}


For [source:trunk/modules/default/templates/admin\_header.html]

	
	{makeCssOptimizerLink(##,#core.php,block.php,blockStyle.php,tools.css#,#vars.php#):h}
	
	{if:isDojoEnabled}
	    <!-- Dojo -->
	    <link rel="stylesheet" type="text/css" media="screen" href="{webRoot}/js/dojo/dojo/resources/dojo.css" />
	    <link rel="stylesheet" type="text/css" media="screen" href="{webRoot}/js/dojo/dijit/themes/{dojoTheme}/{dojoTheme}.css" />
	    <!-- custom overrides of dojo css -->
	    <!-- <link rel="stylesheet" type="text/css" media="screen" href="{webRoot}/themes/{theme}/css/dojo.css" />  -->
	{end:}

#### Dojo Main Files
Just before the </head> tag of both the [source:trunk/modules/default/templates/admin\_header.html] and [source:trunk/modules/default/templates/header.html], add the following:

	{if:isDojoEnabled}
	        {if:conf[debug][production]}
	            <script type="text/javascript" src="{webRoot}/js/dojo/dojo/dojo.js"
	                djConfig="isDebug:false, parseOnLoad: true">
	            </script>
	        {else:}
	            <script type="text/javascript" src="{webRoot}/js/dojo/dojo/dojo.js"
	                djConfig="isDebug:true, parseOnLoad: true">
	            </script>
	        {end:}
	
	        <script type="text/javascript">
	        // <![CDATA[ 
	           dojo.require("dijit.dijit"); // optimize: load dijit layer
	           <!-- this needs to be the latest file included -->
	           <!-- dojo.require("dojo.parser");    // scan page for widgets and instantiate them -->
	        // ]]> 
	        </script>
	{end:}

#### The <body> Tag
For [source:trunk/modules/default/templates/header.html]

	{if:isDojoEnabled}
	<body class="{dojoTheme}" id="page-home">
	{else:}
	<body id="page-home">
	{end:}


For [source:trunk/modules/default/templates/admin\_header.html] 

	{if:isDojoEnabled}
	    <body class="{dojoTheme}">
	{else:}
	    <body>
	{end:}

### Manager
Enable the inclusion of the Dojo code for the managers that require it:

	$output->isDojoEnabled = true;

And set the Dojo theme

	$output->dojoTheme = 'tundra'

## Also See
  * [wiki:Howto/AJAX/dojo]
  * [The Package System and Custom Builds][1]
  * [Dojo Build System][2]
  * [Dojo-Mini: Optimization Tricks with the Dojo Toolkit][3]
  * [The Dojo Build System Presentation][4]
  * [Dojo Javascripts Swiss Army Knife][5]

[1]:	http://dojotoolkit.org/book/dojo-book-0-9/part-4-meta-dojo/package-system-and-custom-builds
[2]:	http://docs.dojocampus.org/build/index
[3]:	http://www.sitepen.com/blog/2008/04/02/dojo-mini-optimization-tricks-with-the-dojo-toolkit/
[4]:	http://www.slideshare.net/klipstein/the-dojo-build-system-presentation
[5]:	http://www.slideshare.net/cb1kenobi/dojo-javascripts-swiss-army-knife-7152009
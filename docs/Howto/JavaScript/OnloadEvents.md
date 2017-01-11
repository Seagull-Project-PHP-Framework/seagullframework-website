<!-- Name: Howto/JavaScript/OnloadEvents -->
<!-- Version: 8 -->
<!-- Last-Modified: 2007/01/12 11:38:36 -->
<!-- Author: jcasanova -->

# Managing onload and onreadyDOM events

Using javascript you will often want to run some scripts when the page is loaded. For ages, developpers have done this by adding a handler on the window.onload event:

	/* Some HTML */
	<body onload="init()">

or

	/* In Javascript */
	window.onload = function() {
	    init();
	}

You can add as many onload event handlers in your code (Managers) by calling the $output-\>addOnLoadEvent() as follows

	$output->addOnLoadEvent("init()");

The problem is that the onload event fires only after all page content has loaded (included images, iframes and any other binary content). If you have a lot of images in your page, you can notice a lag before the page is active.

### 0.6.2 onReadyDOM feature
In most situations what you really need to start your Rich Web Applications is the DOM to be fully available, because your scripts will be interacting with DOM nodes, i.e. elements of the page. So we implemented Dean Edwards [window.onload solution][1] into Seagull so that you just need to pass a second (optional) parameter to the $output-\>addOnLoadEvent() method and your script will be executed as soon as the DOM is ready.

*From your Manager*

Add in the display method

	function display(&$output) {
	    $output->addOnLoadEvent("init('param')", true)
	}
Notice that the function is passed as a string with double quotes, and any param of the js function (if required) is passed in single quotes.
This second parameter has been set to false by default for BC reasons. In further releases, it will be set to true.

*From your javascript code*

Call the sgl.ready() method anywhere, it will be executed as soon as the DOM is ready. You can have several sgl.ready() if needed.

	sgl.ready(function() {
	    init('param');
	});
Notice that you have to pass sgl.ready() an anonymous (lambda) function. If you don't do so, chances are you get an error as the function will be executed as soon as javascript parses this line!

### 0.6.2 other features
To enhance javascript integration within Seagull framework we also added $output-\>addOnUnloadEvent():


	$output->addOnUnloadEvent("foo()")

You may require some event handlers to happen for every page of your site. Some options have been added to the main configuration file so you can add:

	$conf['site']['globalJavascriptOnload']     = '';
	$conf['site']['globalJavascriptOnUnload']   = '';
	$conf['site']['globalJavascriptOnReadyDom'] = '';

## See Also
[[SubWiki(Howto/JavaScript)]]

[[AddComment]]

[1]:	http://dean.edwards.name/weblog/2006/06/again/
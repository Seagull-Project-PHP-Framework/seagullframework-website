<!-- Name: Howto/AJAX/AjaxIntegration -->
<!-- Version: 8 -->
<!-- Last-Modified: 2007/11/08 15:40:24 -->
<!-- Author: lyric -->

# Ajax Integration
* TOC
{:toc}

 * *Note: requires \>= Seagull 0.6.2*

Web 2.0 is all about improving the user experience in your web applications. This is where AJAX comes into play. Behind the buzz there is a reality where you can add nice visual effects, asynchronous communication, and more over interaction on the client side within your applications.

In this "how to" we'll give you some hints on how to use the tools that Seagull framework provides to help you integrate AJAX seamlessly within your applications.

You're free to use any javascript library/framework you like or even code by hand; Seagull provides the [Prototype][1] and [Scriptaculous][2] libraries as they offer a nice set of tools while still remaining light in comparison to big frameworks such dojo.

*Important note:* In a first attempt to provide an AJAX integration we used the PEAR HTML\_AJAX library and its ajaxServer.php approach. This ajaxServer will not be provided anymore in further releases of Seagull (starting next 0.7 development branch) so please consider using the new AjaxProvider approach as described below.

## Step 1 - Require the Prototype library

As said previously the Prototype library is provided in Seagull. Its simple and efficient API has convinced us to use it.

Request the prototype.js file in your manager's display method


	$output->addJavascriptFile('js/scriptaculous/lib/prototype.js');

## Step 2 - Create the method in your <moduleName>AjaxProvider

Add your method in the <moduleName>AjaxProvider ([how to create and work with AjaxProviders][3]) you already created.


	function fooBar()
	{
	    $this->responseFormat = SGL_RESPONSEFORMAT_HTML;
	
	    return 'foo';
	}

Depending on the data your method will return you can send an appropriate content-type header by setting the AjaxProvider-\>responseFormat. You may choose among:

 * SGL\_RESPONSEFORMAT\_JSON
 * SGL\_RESPONSEFORMAT\_PLAIN
 * SGL\_RESPONSEFORMAT\_JAVASCRIPT
 * SGL\_RESPONSEFORMAT\_HTML

When the AJAX response is processed, the appropriate header is sent.

*Note:* The responseFormat is set to SGL\_RESPONSEFORMAT\_HTML by default. You may override this value in your <moduleName>AjaxProvider constructor or even for each method. With SGL\_RESPONSEFORMAT\_JSON the data (any type) is converted to JSON format. 
## Step 3 - Execute an AJAX request from the client side

Now we want to run the fooBar method when the user clicks on a link.

Let's look at the javascript code that will be responsible of running the request and managing the response.


	<script type="text/javascript">
	function doAjaxMethodFooBar() {
	    new Ajax.Request( makeUrl({module: "foo", action: "fooBar"}), {
	        method: 'post',
	        asynchronous:true,
	        parameters: '',
	        onSuccess: function(transport) {
	            alert(transport.responseText);
	        });
	}
	</script>
	
	<html>
	...
	    <a href="#" onclick="doAjaxMethodFooBar(); return false;">click on this link to trigger the ajax method</a>
	...
	</html>

If you are new to Prototype library, you can find a good documentation on how to use the [Ajax.Request API][4]. 

## Sending the request

In this simple example we just want to run the fooBar method asynchronously. To run the AJAX request we just need to set the appropriate url, in this case !http://www.example.com/seagull/www/index.php/foo/foo/action/fooBar.


	new Ajax.Request( makeUrl({module: "foo", action: "fooBar"}), {
	    options
	    });

We set some params such method:'post' and asynchronous:true for better readability of what does our request even if these are default values.

The request headers are now analysed when the SGL\_Request object parses $\_REQUEST and if an AJAX call is detected, a much lighter filter chain of tasks is executed (see [information about AJAX requests][5]). If the method corresponding to the action param in the url is found in the <moduleName>AjaxProvider class, it will be executed.

*Note:* There is currently only one AjaxProvider per module. So you don't need to specify any managerName to the javascript makeUrl() method.

Let's stop here for a second. We defined a method that will invoke an Ajax call from our client side script. Clicking on the link will trigger this method and the response will be sent to the browser. Now what happens with the response? Well it's up to you. Maybe you just want to run some code on the server side. You then probably want to notify the user that the operation performed well or alert them that a problem occurred.

## Processing the response

In this simple example we just want to alert the response of our fooBar() method. You can pass several callbacks to the Ajax.Request. The obvious one is the onSuccess that will be triggered if the response was successfully received.

*Be carefull,* you have to deal with possible logical/fatal errors that happen in your AJAX method; onSuccess does not mean your method went fine. It just means that the AJAX request was correctly sent and the response received.

So everything went fine, our fooBar() method was run successfully and returned a string 'foo'. Our callback is invoked with the XMLHttpRequest object as a first parameter. As a convention we name this first parameter 'transport'. The response of the AJAX request is held within the transport.responseText property. We just alert the response in the client browser.


	onSuccess: function(transport) {
	    alert(transport.responseText);
	}


That's all folks. It is really simple to integrate AJAX within your applications. Read some tutorials to find more detail and advanced examples of how to pass parameters to your AjaxProvider methods, output HTML code or deal with JSON encoded responses.

[1]:	http://www.prototypejs.org
[2]:	http://script.aculo.us
[3]:	/wiki:Howto/AJAX/AjaxProvider/
[4]:	http://prototypejs.org/api/ajax/request
[5]:	/wiki:Howto/AJAX/AjaxRequests/
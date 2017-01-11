<!-- Name: Howto/AJAX/AjaxRequests -->
<!-- Version: 2 -->
<!-- Last-Modified: 2007/05/26 09:58:52 -->
<!-- Author: demian -->

# Ajax Requests and Responses
* TOC
{:toc}

## Requests
The default Front Controller provided by the Seagull framework detects a range of request types, currently:
 * Browser
 * Ajax
 * CLI
 * AMF

Depending on the request type, the relevant Request child class is loaded.  Ajax requests are currently detected with the following logic:


	if (isset($_SERVER['HTTP_X_REQUESTED_WITH']) &&
	          $_SERVER['HTTP_X_REQUESTED_WITH'] == 'XMLHttpRequest') { // }

This works fine for (as tested) the Prototype and HTML\_Ajax libraries.  Leave a note if you have experience with
 * jQuery
 * Dojo
 * anything else

## Responses
The response from an Ajax request can be returned in any of the following formats:


	define('SGL_RESPONSEFORMAT_JSON', 1);
	define('SGL_RESPONSEFORMAT_PLAIN', 2);
	define('SGL_RESPONSEFORMAT_JAVASCRIPT', 3);
	define('SGL_RESPONSEFORMAT_HTML', 4);

The response type can be set on a per method basis.  In your ajax provider, use this convention:


	$this->responseFormat = SGL_RESPONSEFORMAT_HTML;


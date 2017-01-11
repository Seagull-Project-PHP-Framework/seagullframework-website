<!-- Name: Howto/WorkingWithTheRequestObject -->
<!-- Version: 4 -->
<!-- Last-Modified: 2007/02/14 22:44:46 -->
<!-- Author: openhaus -->

# Working with the Request Object
* TOC
{:toc}

## Intro
A new request object is created for each script execution, ie, every time SGL\_FrontController::run() is invoked.  You can obtain a reference to the request object from anywhere in the code as follows:


	$req = SGL_Request::singleton();

or if the $input registry is in scope, you can also get it from there:

	$input->getRequest();

The request object makes request data available to the application. The object performs minimal filtering and sanity-checking on the data.

In the managers, the validate() workflow method allows developers to implement custom logic to determine which request data is valid.  Data deemed valid is copied from the request object to the $input object.

The request is made up of PHP's $\_REQUEST vars, which are then submitted to pre-processing:
  * if magic quotes are enabled, any slashes are removed
  * if the $allowTags flag is set to the default, false, any html is stripped
  * if HTML is allowed, javascript is stripped
  * whitespace at the beginning and end of the strings is removed
  * ASCII zeros are stripped

See [source:trunk/lib/SGL/Request.php] to get an idea of which methods are available.

## Best Practices
You should never use a raw value from PHP's $\_REQUEST array, always call expected vars using the following convention to take advantage of the above processing:


	$input->myVar = $req->get('myVar');

If you don't want HTML automatically stripped out, use:


	$input->aDataItemValue = $req->get('frmFieldName', $allowTags = true);

All values obtained should be mapped to the input object as shown.

## URI Parsing
As the request object is a singleton, it is created just once per script invocation.  During this initial creation, some of the request data is extracted from the calling URI.  Seagull allows developers to select a number of parsing strategies to be run on the URI, the results of which are aggregated to the request data.  As a minimum, Seagull style URI data will be extracted, as will standard querystring info, and certain URI aliases will be resolved, see [wiki:Howto/UriManagement] for more info.

As a result of URI parsing the following keys get added to the request object:


	[frontScriptName] => index.php
	[moduleName] => currentModule
	[managerName] => currentManagerClass

The frontScriptName variable is where the filename that acts as a single point of entry, or front controller, is stored.  Developers can name this file as they wish, or indeed get rid of it altogether using [source:/trunk/etc/htaccess-cleanUrl.dist mod\_rewrite] and setting the config key *frontScriptName* to false.

## URI Caching
As parsing can be expensive, URI mappings are cached so each unique URI will not be parsed more than once during the cache lifetime.

## Cool URIs don't change
Interesting W3C material worth reading on the subject of [good URL construction][1].

[[AddComment]]

[1]:	http://www.w3.org/Provider/Style/URI
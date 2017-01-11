<!-- Name: Howto/HttpRedirects -->
<!-- Version: 5 -->
<!-- Last-Modified: 2006/11/30 15:45:20 -->
<!-- Author: demian -->

# HTTP Redirects
## Overview
The `SGL_HTTP::redirect()` method is used for page forwarding/redirecting and is essentially a wrapper for PHP's `header('Location')` functionality.  Calling redirects is similar to creating URLs in that you can provide no arguments, or one, or many, progressively defining a more precise target URL along the way.

The following will redirect you to the same page, and call the current action, or is there is no action, the default:

	SGL_HTTP::redirect();

Redirects to other method of the current module

	SGL_HTTP::redirect(array('action' => 'myaction'));

Redirects to another module/manager

	SGL_HTTP::redirect(array('moduleName' => 'foo', 'managerName' => 'bar'));

Redirects to an external site

	SGL_HTTP::redirect('http://example.com');

And finally, the following will redirect you to a specific action with a given module's class, and pass appropriate params.

	$options = array(
	    'moduleName' => 'publisher',
	    'managerName' => 'article',
	    'action' => 'edit',
	    'frmArticleId' => 23
	);
	SGL_HTTP::redirect($options);

The `SGL_HTTP::redirect()` method also
  * makes sure the redirect URL is absolute according to RFC 2616;
  * adds session info if necessary;
  * perpetuates `https` if necessary.

## Adding custom redirects in your modules
By default most modules call redirects after data modifications by specifying _redirectToDefault_ in the manager's action mapping array.  This maps to SGL\_Manager::\_cmd\_redirectToDefault which is the parent class of all modules.

Occasionally you will need to customise the redirects, in which case you can use an approach similar to the following:


	    function _cmd_redirectToDefault(&$input, &$output)
	    {
	        SGL::logMessage(null, PEAR_LOG_DEBUG);
	
	        //  if no errors have occured, redirect
	        if (!SGL_Error::count()) {
	            SGL_HTTP::redirect(array('frmCatID' => $this->_redirectCatId));
	
	        //  else display error with blank template
	        } else {
	            $output->template = 'docBlank.html';
	        }
	    }

It is very important to test for errors first, as they must be captured before redirects so they can be output to the screen, if so configured.

[[AddComment]]
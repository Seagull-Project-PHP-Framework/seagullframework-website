<!-- Name: Standards/CorrectUseOfGetAndPost -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/08/18 13:28:43 -->
<!-- Author: demian -->
# Correct Use of GET and POST
## Overview
Many devs use HTTP GET and POST requests in the wrong places, for a good writeup of the differences and when to use what, please see http://www.cs.tut.fi/~jkorpela/forms/methods.html.

To protect against [CSRF attacks](http://www.squarefree.com/securitytips/web-developers.html#CSRF) and to adhere to the HTTP RFC rules on [Safe and Idempotent Methods](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.1),

 * Check that all requests that create, modify or delete resources use the HTTP POST method.
 * Use real server-side confirmation for deletion of wiki pages and attachments, instead of the JavaScript confirmation dialog.

In other words, GET must only be used for idempotent processing, all other requests must be done with POST.

*Idempotent*: no lasting observable effect on the state of the world

Idempotent processing means that a form submission causes no changes anywhere except on the user's screen (or, more generally speaking, in the user agent's state). Thus, it is basically for retrieving data



    GET:  a search form
    POST: updating a user record
    GET:  tweaking filter params to return property results
    POST: logging into a website
    GET:  button to go to certain page

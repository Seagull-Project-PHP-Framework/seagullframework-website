---
layout: page
title: Front Controller
permalink: /Howto/PragmaticPatterns/InterceptingFilter.html
---

<!-- Name: Howto/PragmaticPatterns/InterceptingFilter -->
<!-- Version: 7 -->
<!-- Last-Modified: 2006/11/15 17:21:13 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Intercepting Filter
* TOC
{:toc}

## Overview
Seagull uses the [Intercepting][1] [Filter][2] pattern which allows developers to setup a custom chain of pre and post processing filters on a per request basis.

As the data model is built up, it passes through the filter chain before reaching its ultimate target, the so-called Core Processor, which in most cases is the Application Controller.  What this means is you can filter and transform any data coming into your core business process, then filter the results that are returned before they are sent back to the client.

Typically data being sent to the core business process is the request object.  In Seagull's case this is also true, however data deemed valid from the request is mapped to an input object which is passed through the filter chain.  After the target process has executed, a response in the form of an output object is sent back through the filter chain for post processing, and to eventually be returned to the client.

Here's an example to clarify:

	    $request
	        $input
	            //  pre-process on $input
	            SGL_Task_StripMagicQuotes()
	            SGL_Task_ResolveManager()
	            SGL_Task_AuthenticateRequest()
	
	                // core process
	                $myMgr->validate()
	                $myMgr->process()
	                $myMgr->display()
	
	            // post process on $output
	            SGL_Task_BuildView()
	            SGL_Task_BuildHeaders()
	
	        $output
	    echo $output->data;

Note the inverted pyramid, more clearly described in this UML:

![][image-1]

## Customising the Filter Chain
Quite understandably the developer may want to customise the filters on a per-request basis.  You may want to 

 * output your data as XML
 * run it through an HTML to PDF converter

A generic filter chain is provided in Seagull by default, but is very easy to customise with one line of configuration.

In a module's config, add something like the following to setup your own chain, this example is for building a REST server:


	[RestMgr]
	requiresAuth    = false
	filterChain     = " SGL_Task_Init,
	                    SGL_Task_ResolveManager,
	                    EG_Filter_PhpToXmlSerializer,
	                    EG_Filter_XmlToPhpUnserializer,
	                    EG_Filter_BuildHeaders,
	                    EG_CoreProcessor"

Note that the target, in this case EG\_CoreProcessor, must always be the last item in the list.

## Make your Filters Portable
To build your own filter chain follow the example in [gitlink:/branches/0.6-bugfix/lib/SGL/FrontController.php]. Here are some guidelines to keep your code clean and reusable:
 
If you are using libs specific to your own project, for example "My Important Client", create your own namespaced lib directory somewhere on the filesystem, within Seagull if you like, eg:


	seagull/lib/MIC/

and put your filters in an obvious place:

	seagull/lib/MIC/Filters/MyFooTransform.php
	seagull/lib/MIC/Filters/MyBarTransform.php

Add the path to this directory to your Seagull configuration, in Configuration -\> General.

Finally, modify your module's `conf.ini` file to reflect the filters' class names and correct order:

	[MyMgr]
	requiresAuth    = false
	filterChain     = " SGL_Task_Init,
	                    SGL_Task_ResolveManager,
	                    MIC_Filters_MyFooTransform,
	                    MIC_Filters_MyBarTransform,
	                    EG_CoreProcessor"

The [gitlink:/branches/0.6-bugfix/lib/SGL/FilterChain.php Filter chain object] is the one that does the dynamic evaluation of your filter chain.

## Where is this used in Seagull
Currently the export module uses the custom filter chain approach to set configurable headers, in this case the text/html Content-type headers, to produce an RSS feed.  See the [gitlink:/branches/0.6-bugfix/modules/export/conf.ini conf.ini] for that module to get an idea for how few tasks are loaded, and how the ordering is important.

## How Other Frameworks Intercept Requests
I've just been reading up on Django, probably the top-rated Python web framework right now, it seems they haven't heard of intercepting filters, view their solution here: 

http://www.djangoproject.com/documentation/middleware/

I do like some of the filters they reference, would be nice to see in Seagull.

## See Also
[Howto/PragmaticPatterns][3]

[1]:	http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnpatterns/html/DesInterceptingFilter.asp
[2]:	http://java.sun.com/blueprints/corej2eepatterns/Patterns/InterceptingFilter.html
[3]:	/Howto/PragmaticPatterns.html

[image-1]:	/images/intercepting-filter.gif
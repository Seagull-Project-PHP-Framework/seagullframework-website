---
layout: page
title: Validate Process Display workflow
permalink: /Concepts/ValidateProcessDisplay.html
---

<!-- Name: Concepts/ValidateProcessDisplay -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/04/01 23:32:25 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Validate Process Display workflow

The validate/process/display workflow used in Seagull simply means that all data that passes through the system must be processed by the following methods:

  * *validate*: the raw $\_REQUEST array is passed to this method (wrapped in the SGL\_Request object), validations performed, and acceptable data mapped to an $input object
  * *process*
	* if valid: if all data is valid, the $input object is passed to the process method, which redirects to the relevant action method derived from URI params.
	  * once data has been manipulated, it is mapped to an $output object
	* if invalid: if one or more validations have failed, the request is deemed to be invalid, and the data passed directly to the display() method with appropriate error messages, ie, to be presented back to the user for correction.  The process method is bypassed.
  * *display*: this method offers the opportunity to add some post-processing to the data that all actions have in common. For example, if you have 5 methods that all need to build combobox data for forms, here is the place to do it once for all of them.

Once the above operations have been performed, responsibility passes back to the Core Processor which, if you use SGL\_MainProcess means the relevant view information is extracted from the $output object, the rendering engine determined from the config, and the data formatted according to the target client device and passed back through the filter chain for post-processing.  

An example would be:
 * a contact form is posted with field data deemed to be valid
 * business logic executed to dispatch the details via email and save the request in the database
 * a confirmation message built for displaying to the user
 * the emailSent.html template specified
 * the HTML renderer instantiated, using the Flexy template engine as specified in the config
 * the template rendered with vars populated from $output data, or loaded from the cache if it exists
 * text/html headers generated and response sent to the browser client
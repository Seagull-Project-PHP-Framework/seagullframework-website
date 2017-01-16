<!-- Name: Integration/flexible-js-formatting -->
<!-- Version: 3 -->
<!-- Last-Modified: 2009/03/04 10:56:35 -->
<!-- Author: demian -->
# Fast, flexible JavaScript libraries for formatting and parsing numbers and dates (LGPL)
 * The latest code can be found at: http://code.google.com/p/flexible-js-formatting/

[[TOC]]

## Overview
This project consists of two libraries that dynamically build a formatting function for number and date formatting. The date formatting is very similar to PHP's functionality; it also permits parsing dates. The number formatting is similar to MS Excel formatting. The technique of building a function to implement each formatting instruction leads to cleaner code that is richer in functionality and much faster than other popular libraries, as proven by benchmarks.

_By the way, this library depends on the Prototype library, so you will also need to reference that in your page. If you don’t, you won’t get an error, but nothing will happen._

## Input Masks
Input masks are guides to help users enter data in the correct format. 

Load the Javascript libraries in the Add and Edit methods:

    function _cmd_add(&$input, &$output)
    {
        $output->addJavascriptFile(array('js/scriptaculous/lib/prototype.js','js/flexible-js-formatting/html-form-input-mask.js'));
        $output->addOnLoadEvent("Xaprb.InputMask.setupElementMasks();");
    }

Then in a template use the library to format a phone number field:

    <input type="text" name="foo[telephone]" value="{foo.telephone}" maxlength="13" class="text input_mask mask_phone" />
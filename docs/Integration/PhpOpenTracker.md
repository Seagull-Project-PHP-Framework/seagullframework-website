<!-- Name: Integration/PhpOpenTracker -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/11/30 15:52:59 -->
<!-- Author: demian -->
# Integrating phpOpenTracker
## Overview
[phpOpenTracker](http://www.phpopentracker.de) is a framework solution for the analysis of website traffic and visitor analysis.

Download the package, install it (with PEAR), configure it.

To log all visits with all page views add the following code to, eg, [browse:/lib/core/SGL_FrontController.php] in the *run* method after HTML data is echoed out.


    $pageName = $req->getModuleName() . ": " . $req->getManagerName();
    if (isset($_SERVER["QUERY_STRING"]) && !empty($_SERVER["QUERY_STRING"]] {
        $args = explode("&", $_SERVER["QUERY_STRING"]);
        foreach ($args as $arg) {
            if (!empty($arg] {
                $pageName .= ": $arg";
            }
        }
    }
    
    phpOpenTracker::log (
        array(
            'client_id' => THIS_SITES_CLIENT_ID,
            'document' => $pageName,
        )
    );

Modify $pageName as you need it.

Caution: Logging has a *not unsignificant* impact on the page execution time!

[[AddComment]]
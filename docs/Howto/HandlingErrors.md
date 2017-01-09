<!-- Name: Howto/HandlingErrors -->
<!-- Version: 5 -->
<!-- Last-Modified: 2006/11/30 15:38:13 -->
<!-- Author: demian -->
# Handing Errors
## Overview
Seagull provides some flexible tools and a number of configuration options to help you handle errors in your application.

 * All raised errors are PEAR error objects
 * The advantage of returning an error object rather than just false is it can store information regarding file and line number where the error occured, an error code, and messages for system and public users
 * Output is sent to logs and screen depending on configuration  
 * Admin is notified by email if error is greater than preset threshold in Config
 * Output to screen is suppressed if 'production' key in config is set to 'true'
 * Errors can be logged to a variety of targets thanks to PEAR::Log, use the Configuation screen to choose between file or DB logging.
 * use the SGL_Error object to consult the error stack, [basic methods](http://api.seagullproject.org/SGL/SGL_Error.html) are provided to let your query and manipulate the stack
 * To raise an error use SGL::raiseError(String $errorMsg, Constant SGL_ERROR_CONST [, Constant PEAR_ERROR_DIE ]).


        SGL::raiseError('There was a problem inserting the record', SGL_ERROR_NOAFFECTEDROWS);

 * Example of available error constants:


        define('SGL_ERROR_DBFAILURE',           -111);
        define('SGL_ERROR_DBTRANSACTIONFAILURE',-112);
        define('SGL_ERROR_BANNEDUSER',          -113);
        define('SGL_ERROR_NOFILE',              -114);
        define('SGL_ERROR_INVALIDFILEPERMS',    -115);
        define('SGL_ERROR_INVALIDSESSION',      -116);
        define('SGL_ERROR_INVALIDPOST',         -117);
        define('SGL_ERROR_INVALIDTRANSLATION',  -118);
        define('SGL_ERROR_FILEUNWRITABLE',      -119);

  * To raise a fatal error that will halt program execution, use the following format:


        SGL::raiseError('Cannot connect to DB, check your credentials, exiting ...',
            SGL_ERROR_DBFAILURE, PEAR_ERROR_DIE);

[[AddComment]]
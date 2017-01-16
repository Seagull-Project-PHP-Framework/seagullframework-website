<!-- Name: Howto/DebuggingIdeas -->
<!-- Version: 11 -->
<!-- Last-Modified: 2007/09/17 23:45:09 -->
<!-- Author: demian -->
<!-- Status: Original -->

# Debugging Ideas
* TOC
{:toc}

## Intro
Seagull uses the class [source:/trunk/lib/SGL/ErrorHandler.php] to override PHP's default error handling.  Errors basically fall into two categories, those that are thrown by the system and those thrown by the developer.  Beyond this distinction, an error can be a PHP error, ie, originating from the PHP engine or thrown by the developer with PHP's trigger\_error() function, or a PEAR error.

## Errors
### Which Types of Errors Handled
Seagull's error handing allows you to customise the output of any error type by defining thresholds in the application configuration.  In the event of an error the following happens:

  * error output is hidden from the screen unless a parse error occurs in which case the script is not executed, therefore the error handler cannot come into play
  * if logging is enabled in Configuration, errors up to the threshold you define are logged to the target of your choice, ie, file, db, etc. 
  * errors that reach the SGL\_EMAIL\_ADMIN\_THRESHOLD are emailed to the administrator.  It is a good idea to set this to PEAR\_LOG\_WARNING for a live site, although PEAR\_LOG\_EMERG is fine for development.  The email to admin grabs a bunch of useful extra info: last query, client details, etc 

### Which Code Does the Work
In terms of the code,  PHP errors are handled by [source:/trunk/lib/SGL/ErrorHandler.php] which basically uses set\_error\_handler() and PEAR errors are handled by the pearErrorHandler() fn, in the SGL\_Task\_SetupPearErrorCallback() task, which is the callback referenced by 


	PEAR::setErrorHandling(PEAR_ERROR_CALLBACK, 'pearErrorHandler');


The error severity is dynamically passed to the logs, with the severity level mapped to PEAR::Log's level with a simple PHP/PEAR mapping, see the *ErrorHandler* constructor for details.

If you're on a linux system, you can uncomment the highlight\_file fn in the ErrorHandler class, unfortunately this coredumps on windows.

### Making the Most of File Logging
Any linux user will automatically tail a log file interactively to catch errors/feedback as they happen, however, this is not automatic for windows users.  All is not lost, there are many unix emulation tools windows users can take advantage of, some more complex than others, to get the same effect.  [MSYS][1] is an ideal lightweight package, a couple MB download, painless install, and gives you identical unix tail functionality.



	tail -f [path-to-seagull]/var/log/php_log.txt


Other windows programs simulating tail:
[mTAIL][2]

To enable logging go the the Configuration screen, choose the 'Logs' section and select enable.  The *debug* priority gives the most feedback.

### How to Handle Notices
By default, reporting of E\_NOTICE level errors is enabled.  Seagull is coded to E\_ALL and it's a good idea to get your classes to work to this level.  If you want to suppress the notice errors simply comment the 'notice' element of the errorType array in [source:/trunk/lib/SGL/ErrorHandler.php].

Additionally, if you know you have a block of code that throws notices, you can wrap it as follows to keep a notice-free output:


	SGL::setNoticeBehaviour(SGL_NOTICES_DISABLED);
	
	//  code that throws notices
	
	SGL::setNoticeBehaviour(SGL_NOTICES_ENABLED);



### Developer Triggered Errors
To throw errors, developers are encouraged to use Seagull's static method:



	SGL::raiseError('message text', [SGL_ERROR_CONST], [PEAR_ERROR_DIE])


The first parameter is the message you wish to raise, the optional second parameter is any of the Seagull error constants defined in constants.php, and the optional 3rd parameter, PEAR\_ERROR\_DIE, will halt program execution.  If the 3rd argument is used, the 1st arg message is sent to the screen regardless of whether debug is enabled or not.

Many developers familiar with DB\_DataObject will notice similarities here, the method signature is the same, functionality almost identical, but the implementation is simpler.

#### SGL Error Constants


	        //  error codes to use with SGL::raiseError()
	        //  start at -100 in order not to conflict with PEAR::DB error codes
	        define('SGL_ERROR_INVALIDARGS',         -101);  // wrong args to function
	        define('SGL_ERROR_INVALIDCONFIG',       -102);  // something wrong with the config
	        define('SGL_ERROR_NODATA',              -103);  // no data available
	        define('SGL_ERROR_NOCLASS',             -104);  // no class exists
	        define('SGL_ERROR_NOMETHOD',            -105);  // no method exists
	        define('SGL_ERROR_NOAFFECTEDROWS',      -106);  // no rows where affected by update/insert/delete
	        define('SGL_ERROR_NOTSUPPORTED'  ,      -107);  // limit queries on unsuppored databases
	        define('SGL_ERROR_INVALIDCALL',         -108);  // overload getter/setter failure
	        define('SGL_ERROR_INVALIDAUTH',         -109);
	        define('SGL_ERROR_EMAILFAILURE',        -110);
	        define('SGL_ERROR_DBFAILURE',           -111);
	        define('SGL_ERROR_DBTRANSACTIONFAILURE',-112);
	        define('SGL_ERROR_BANNEDUSER',          -113);
	        define('SGL_ERROR_NOFILE',              -114);
	        define('SGL_ERROR_INVALIDFILEPERMS',    -115);
	        define('SGL_ERROR_INVALIDSESSION',      -116);
	        define('SGL_ERROR_INVALIDPOST',         -117);
	        define('SGL_ERROR_INVALIDTRANSLATION',  -118);
	        define('SGL_ERROR_FILEUNWRITABLE',      -119);
	        define('SGL_ERROR_INVALIDMETHODPERMS',  -120);
	        define('SGL_ERROR_INVALIDREQUEST',      -121);
	        define('SGL_ERROR_INVALIDTYPE',         -122);
	        define('SGL_ERROR_RECURSION',           -123);

 
## Debugging

### Using the Zend Debugger
One of the best ways to track down problems is by using a debugger like what's supplied with the Zend IDE.  One catch is that you often need to access pages requiring authentication, so you have to start a session, login, then navigate back to the page where the problem is.  An easy way to get around this is set the Configuration option 'Enable authentication' to no.  This allows you to access any resource in the framework as if you were an admin user, without having to login.

The Zend debugger extension binaries for your webserver can be downloaded from here: http://downloads.zend.com/pdt/server-debugger/

Installing is pretty straight-forward (from the readme.txt in one of the archives above):

	Zend Debugger installation instructions
	---------------------------------------
	
	1. Locate ZendDebugger.so or ZendDebugger.dll file that is compiled for the
	   correct version of PHP (4.3.x, 4.4.x, 5.0.x, 5.1.x, 5.2.x) in the 
	   appropriate directory.
	
	2. Add the following line to the php.ini file:
	   Linux and Mac OS X: zend_extension=/full/path/to/ZendDebugger.so
	   Windows:            zend_extension_ts=/full/path/to/ZendDebugger.dll
	
	3. Add the following lines to the php.ini file:
	   zend_debugger.allow_hosts=<ip_addresses>
	   zend_debugger.expose_remotely=always 
	
	4. Place dummy.php file in the document root directory.
	
	5. Restart web server.

### Debug sesssions
 * debug sessions are available in \>= Seagull 0.6.3
Developers can also invoke a privileged debug session - this option is available on production sites.  The concept is simple, as long as the developer knows the secret key and passes it into the session, debug info will be available to him/her.  This can be quite handy when certain errors only occur when dealing with live data.

To setup a debug session take the following steps:

 1. in the Config -\> Debug screen, set 'enable debug session' to true
 1. in Config -\> General set a secret key that will be used to invoke the session, under 'admin key'
 1. log out
 1. start the privileged session by calling a URL like

	http://example.com/index.php?adminKey=my\_secret\_key
 1. any variables or data your expose in the following conditional will be displayed

	if (SGL\_Config::get('debug.sessionDebugAllowed')) {
		echo 'this is debug';
	}

### Debugging Db\_DataObject
As most developers familiar with the Db\_DataObject package will know, there is a built-in mechanism for displaying debug info for the class.  In the Db\_DataObject config array, which is set towards the bottom of init.php, there is a key passed named 'debug' which, by default, is set to zero.  Db\_DataObject recognises 5 levels of debug info, 5 being the most verbose - change this setting to send debug info to the screen.

### Debugging CLI Apps
Mr. Beaver has some invaluable advice here, read about it : http://greg.chiaraquartet.net/archives/16-Incredible-Zend-ZDE-debugging-trick-for-debugging-CLI-apps.html

Example:


	export  QUERY_STRING="start_debug=1&debug_port=10000&debug_fastfile=1&debug_host=192.168.60.1%2C172.16.247.1%2C192.168.2.3%2C127.0.0.1
	&send_sess_end=1&debug_no_cache=1158079848619&debug_stop=1&debug_url=1&debug_start_session=1&no_remote=1"


### Debugging XML-RPC Apps
Similar idea, see the commented out line in this file:
http://trac.seagullproject.org/browser/trunk/modules/default/xmlrpc\_conf.ini

### Debugging Ajax Apps
There are basically two options:

#### Using Zend
From the Zend toolbar available in Firefox, choose Debug Menu -\> Next page.  The fire an Ajax request by clicking in the browser appropriately - the debug session will start.

#### Using Unit Tests
 * requires \>= Seagull 0.6.3
Within the unit test you can create a sub-request that is of type Ajax, ie so SGL\_Request\_Ajax is called and the Ajax-specific filter chain, if it has been defined, it called.  Use the following convention:


	$req = SGL_Request::singleton(true, SGL_REQUEST_AJAX);
	$req->set('moduleName', 'foo');
	$req->set('action', 'getPersonById');
	$req->set('usrId', 1);
	SGL_Config::set('debug.authorisationEnabled', false);
	SGL_FrontController::run();


### Examining global NameSpace
Often it quite helpful to examine the Seagull NameSpace to see what your variables are doing.  Some of framework-related variables can be found in $GLOBALS['\_SGL'], the best way to see what you've got is:


	print_r($GLOBALS['_SGL']);

optionally contained with PRE tags, to improve formatting. Also useful is the ability to examine the request object, then verify against the input object to see if you've successfully mapped the desired, filtered data as expected.  To do this you can echo out the request object at the beginning of any validate() method:



	$req->debug();


or echo the input object end the end of any validate method, to see what's getting passed back to the controller. 



	print_r($input);

### Using the Debug Block
As of 0.6.1, in Config you can enable [debug][enableDebugBlock], and the next time you run the Seagull rebuild action in the maintenance section, a debug block will appear in the left hand column.  This block allows you to run rebuilds easily without having to navigate to the relevant admin section.  Currently it is only enabled for admins.


### Verifying Input


	print '<pre>'; print_r($input); print '</pre>';


add to the end of your validate() method, This will allow it so you can see which vars are successfully being assigned to the input object from the request.

### Unit Tests
Seagull comes with a test runner app that's built on top of SimpleTest 

[source:/trunk/tests]

For more info on unit testing see [wiki:Standards/UnitTesting]

#### Testing tools

There are different tools for testing an PHP application, e.g.:

  * [SimpleTest][3]
  * [PEAR::PHPUnit][4] and this [PEAR::PHPUnit homepage][5]
  * [phpt][6]
	 
#### Tutorials / Articles about (Unit)Testing:

  * Printed Article in [german PHP Magazin][7] (03/2003)
  * [SimpleTest Doku][8]: see Tutorials node
  * [http://www.sebastian-bergmann.de/talks/2004-05-04-PHPUnit.pdf]: Some material of the author of PEAR::PHPUnit
  * [Tutorial of PHPUnit][9] at the [Pear Manual][10]

### UAT Testing
 * http://www.openqa.org/selenium/

There is also an Eclipse plugin called Solex (http://solex.sourceforge.net/) with the same functionality but the ability to apply extraction and replacement rules.

[[AddComment]]

[1]:	http://www.mingw.org/msys.shtml
[2]:	http://ophilipp.free.fr/op_tail.htm
[3]:	http://simpletest.sourceforge.net/
[4]:	http://pear.php.net/package/PHPUnit
[5]:	http://www.sebastian-bergmann.de/en/phpunit.php
[6]:	http://qa.php.net/write-test.php
[7]:	http://www.phpmag.de/itr/ausgaben/psecom,id,131,nodeid,60.html
[8]:	http://simpletest.sourceforge.net/
[9]:	http://pear.php.net/manual/en/packages.php.phpunit.intro.php
[10]:	http://pear.php.net/manual/en/
<!-- Name: Howto/WorkingWithCliOrHttpRequests -->
<!-- Version: 8 -->
<!-- Last-Modified: 2007/05/21 17:45:50 -->
<!-- Author: demian -->
<!-- Status: Original -->

# Working with CLI or HTTP Requests
* TOC
{:toc}

## Overview
Seagull detects the type of application request and deals with it accordingly.  If the request is from a browser or web-enabled device, HTTP initialisation will be invoked.  If it is commandline, the CLI initialisation is run.

## HTTP Requests
With HTTP requests the URI is parsed according to the parsing strategies loaded, and the relevant data is extracted, this is explained in greater detail [here][1].

## CLI Requests
If Seagull detects a CLI request, a similar initialisation takes place, however fewer parameters are involved.  Typically you would invoke a CLI run of Seagull as follows:


	$ php seagull/www/index.php --moduleName=foo --managerName=bar --action=myAction

Arbitrary arguments must be in the form of name =\> value pairs. The 
value being preceded by '--' and the value should follow '='.

Example 

	$ php seagull/www/index.php --moduleName=foo --managerName=bar --action=myAction --customVariable=true --fileName=/path/to/file.xml

If arbitrary arguments are not formatted properly, an error "CLI parameters invalid" will occur.

You can then access your arguments through the Request Object as noted
here [SGL\_Request Object][2].

### Running CLI jobs as Apache User
By default you will probably write cronjobs to invoke Seagull in CLI mode as the current user.  However if the same Seagull installation is used in web mode, ie requested by web browsers, you'll get a conflict of file ownership for files created by the webserver, ie what's in the var directory.  You can easily overcome this by running your CLI scripts as the Apache user as well.  Given that Apache is being run as www-data (it could also be apache or nobody) use this convention:



	su - www-data -c "php /home/seagull/www/index.php  --moduleName=foo --managerName=bar"


#### Comment by mwattier on Thu Nov 30 19:08:09 2006
As of version 0.6.1, authentication is not handled for CLI requests. Your module configuration for requiresAuth must be false in conf.ini [see ticket 1324][3]
Example

	[ImportMgr]
	requiresAuth=false

[1]:	/Howto/WorkingWithTheRequestObject.html
[2]:	/wiki:Howto/WorkingWithTheRequestObject/
[3]:	http://trac.seagullproject.org/ticket/1324
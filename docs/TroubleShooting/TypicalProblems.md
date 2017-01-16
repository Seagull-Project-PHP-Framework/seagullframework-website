<!-- Name: TroubleShooting/TypicalProblems -->
<!-- Version: 27 -->
<!-- Last-Modified: 2008/11/20 14:46:53 -->
<!-- Author: jonas -->
# Typical Problems


* I inherit from RegisterMgr class to make my own register form. It worked well on 0.55 but now in 0.6 I get: Warning: md5() expects parameter 1 to be string, object given in /lib/SGL/DB.php on line 190. *

Remove 

    SGL_DB::setConnection($this->dbh);
line in _cmd_insert method

Check the new RegisterMgr class to see the changes

----

*Setting custom default module I get Fatal error: Call to a member function validate()*

When setting the default manager in the configuration, you must use the short name of the manager. For example:

 * FaqMgr     = faq
 * DefaultMgr = default

_Note: short names should be lower cased_

----

* All the blocks on my site have disappeared! *

Make sure the blocksEnables setting in your <domain>.conf.php is set to 1. E.g.:

blocksEnabled=1

----

*I'm getting FATAL pass by reference errors*

Are you using PHP 5.0.5? This is a known buggy version, please upgrade (or downgrade).

----

*I changed my site from PHP 4.x to PHP 5.x (or vice versa) and now I get an error with db_dataobject entities*

You have to regenerate the db_dataobjects specifically for the PHP version.  This can be invoked with the following request:


    http://localhost/seagull/www/index.php/default/maintenance/action/dbgen/

You will need to disable the authentication key first to be able to do this, in seagull/var/<server>.conf.php:


    $conf['debug']['authenticationEnabled'] = '0';

----

*I have accidently set config values that stop my site from running*

This can happen quite easily, best approach is to reset to a default config.  To do this, request 'setup.php' instead of the default index.php.  You will be prompted for a password.  If you don't remember what you set, simply delete the file 'INSTALL_COMPLETE.php' from the seagull/var directory.  During the setup, select 'use existing database' and 'preserve your existing data' to preserve your existing data.

----

*I get the message "The specified method, list does not exist"*

Check the specified module's conf.ini file located in modules/<modulename>/conf.ini and ensure that each manager in the module/classes directory is listed

You must also ensure that you have setup your method in your manager's action mapping

If you are unfamiliar with action mappings please read here [Tutorials/WorkingWithActions](http://trac.seagullproject.org/wiki/Tutorials/WorkingWithActions#CreatetheActionMapping)

----


*I get the message "No input file specified"*

This means you're runing PHP as a CGI instead of the recommend apache module configuration, mod_php.  You can get around some of the limitations of this mode by updating your Seagull config file.  Locate seagull/var/<servername>.conf.php and find the line:


    $conf['site']['frontScriptName'] = 'index.php';

and change it to


    $conf['site']['frontScriptName'] = 'index.php?';

----

* I get the error message 'Use of undefined constant ""__FUNCTION__"" - assumed '""__FUNCTION__""*''

This means you are using a version of PHP older than 4.3.0, Seagull will run fine in this case however you may want to disable error reporting to screen, this is done by going into /etc/<servername>.default.conf.ini and setting the production key to 'true':


    [debug]
    production = true

----

* Args to Flexy methods must NOT be separated by spaces before or after the commas *

Incidentally, my problem was compounded by an HTML_Template_Flexy fatal
error in the HTML (unexpected character 0x20).  I had placed a space between
parameters in the generateSelect() parameter list (for readability).
Although I might complain about Flexy's pickiness, I can't complain about
the specificity of the error detection - it was specific to the character
and the cause.

----

* My pages seem to be rendering slowly *

The first time you request a page its template gets compiled, similar to JSP, the subsequent request will be a lot faster.  Additionally you can enable caching which speeds up pages significantly: logon as admin, go to 'Configuration', and set 'Caching enabled' to yes.

----

* MS IExplorer cannot download PDF files *

A well known bug in MS IExplorer could create problems when downloading PDF files, especially if the files are dynamically created.
Using this code: 

    $dl = &new SGL_Download();
    $dl->setData($myPdfData);
    $dl->setContentType('application/pdf');
    $dl->setContentTransferEncoding('binary');
    $dl->setContentDisposition(HTTP_DOWNLOAD_INLINE, 'myfile.pdf');
    $dl->setAcceptRanges('none');
    $dl->setContentTransferEncoding('binary');
    $error = $dl->send();

IExplorer could give you an error after opening the Acrobat Reader Plugin.

A good solution is to change the ContentDisposition parameter to *HTTP_DOWNLOAD_ATTACHMENT* instead: IExplorer will open a new window and will ask you to choose from 'open' or 'save' the file.

Choosing 'open' the PDF will be downloaded and presented without problem.

----

* I can not login with Safari or Internet Explorer (IE) *

Safari and IE seem to ignore Cookies from domain names with underscores. So if you cannot login into Seagull using these browsers and your domain is something like 


    developer_portal.mybox

try to remove the underscore(s):


    developerportal.mybox


----

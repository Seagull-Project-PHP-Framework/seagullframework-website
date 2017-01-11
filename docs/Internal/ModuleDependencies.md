<!-- Name: Internal/ModuleDependencies -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/02/09 02:40:00 -->
<!-- Author: aj -->

# Modules dependencies - Seagull 0.4.0 CVS 2004/11/15

*TODO: Create DSM (http://www.lattix.com/technology/whatisdsm.htm)*

This page describes the dependencies beetwen Seagull modules.

[[TOC]]

Of course, all the modules needs the SGL Core. So we can determinate that a basic Seagull install would be :
  * *SGL Core*
  * *default* module
  * *user* module
  * *messaging* module

The *maintenance* module is too an important one, and should be kept.


## SGL Core
#### Mandatory Seagull Modules
  * default
  * user

#### Optional Seagull Modules
  * block _($conf['site']['blocksEnabled'])_
  * navigation _($conf['navigation']['enabled'])_

#### External Modules
  * _lib/pear/Benchmark/Timer.php_
  * _lib/pear/Cache/Lite.php_
  * _lib/pear/Date.php_
  * _lib/pear/DB.php_
  * _lib/pear/DB/NestedSet.php_
  * _lib/pear/HTML/Template/Flexy.php_
  * _lib/pear/Log.php_
  * _lib/pear/Pager/Pager.php_
  * _lib/pear/PEAR.php_
  * _lib/pear/System.php_
#### Tables used
  * block
  * block\_assignment
  * category
  * item
  * item\_type
  * item\_type\_mapping
  * item\_addition
  * log\_table (only used for Log\_sqlite class, when logging to an sqlite table)
  * session
  * table\_lock (used by modules for SGL\_NestedSet)
  * user


## block module
#### External Modules
  * _lib/pear/HTML/QuickForm.php_
#### Tables used
  * block
  * block\_assignment
  * section

*Each block in _modules\block\classes\blocks_ has its own dependencies (eg.: navigation - _DirectoryNav.php_ requires _modules/navigation/classes/MenuBuilder.php_)*


## contactus module
====  Mandatory Seagull Modules  ==== 
  * messaging

====  External Modules  ==== 
  * _lib/pear/Validate.php_
#### Tables used
  * contact\_us
  * usr


## default module
#### External Modules
  * _lib/pear/Config.php_
  * _lib/pear/Validate.php_
#### Tables used
  * module
  * permission

=\> TODO : Handle correctly '$conf['navigation']['driver']', '$conf['navigation']['stylesheet']' and '$conf['navigation']['enabled']'
=\> In DefaultMgr, '\_showNews' action possible only if 'publisher' module is installed ?


## documentor module
#### Mandatory Seagull Modules
  * navigation
  * publisher


## faq module
#### Tables used
  * faq


## guestbook module
#### Tables used
  * guestbook


## maintenance module
#### Mandatory Seagull Modules
  * default

#### External Modules
  * _lib/pear/Config.php_
  * _lib/pear/System.php_
  * _lib/pear/DB/DataObject/Generator.php_ 
#### Tables used
  * sequence


## messaging module
#### External Modules
  * _lib/pear/Mail.php_
  * _lib/pear/Mail/mime.php_
  * _lib/pear/Net/UserAgent/Detect.php_
#### Tables used
  * contact
  * instant\_message
  * usr


## navigation module
#### Mandatory Seagull Modules
  * user
  * publisher

#### External Modules
  * _lib/pear/Cache/Lite.php_
  * _lib/pear/Config.php_
#### Tables used
  * item
  * item\_addition
  * item\_type
  * item\_type\_mapping
  * section
  * table\_lock (SGL\_NestedSet)

*Only files in _module/navigation/classes/menu_/ need 'publisher' module *


## newsletter module
#### Mandatory Seagull Modules
  * user

#### External Modules
  * _lib/pear/Mail.php_
  * _lib/pear/Validate.php_
  * _lib/pear/Mail/mime.php_


## publisher module
#### Mandatory Seagull Modules
  * navigation
  * user

#### External Modules
  * _lib/pear/Text/Statistics.php_
  * _lib/pear/HTML/TreeMenu.php_
  * _lib/pear/HTML/Tree.php_
  * _lib/pear/System.php_
  * _lib/other/Zip.php_
#### Tables used
  * category
  * document
  * document\_type
  * item
  * item\_addition
  * item\_type
  * item\_type\_mapping
  * usr


## randommsg module
#### Tables used
  * rndmsg\_message


## user module
#### Mandatory Seagull Modules
  * messaging
  * default

#### External Modules
  * _lib/pear/Validate.php_
  * _lib/pear/Text/Password.php_
  * _lib/pear/System.php_
#### Tables used
  * item
  * login
  * module
  * org\_preference
  * organisation
  * organisation\_type
  * permission.php
  * preference
  * role
  * role\_permission
  * table\_lock (SGL\_NestedSet)
  * user\_permission
  * user\_preference
  * usr
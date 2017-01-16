<!-- Name: Howto/UsingConfigFiles -->
<!-- Version: 17 -->
<!-- Last-Modified: 2007/03/29 15:37:25 -->
<!-- Author: demian -->
<!-- Status: Original -->

# Config files
* TOC
{:toc}

## Intro
Seagull uses a number of configuration files to supply information to the application, the two main types are
 * the system or global config file

	seagull/var/<your-hostname>\_<port-number>.conf.php
 * module config files, one per module

	seagull/modules/$module/conf.ini

The system config file is created during installation and is based on a template located in 


	seagull/etc/default.conf.dist.ini

The hostname of the machine you're running is prepended to your config file, so if you machine is called

*www.example.com*

your config file will be called

*www.example.com.conf.php*

if you use port 8080 as 

*www.example.com:8080*

your config file will be called

*www.example.com\_8080.conf.php*

This way you can run your code on a number of machines, and for each one a server-specific config file will be loaded.

## Config File Formats
The default data format used is a PHP array in a file with a PHP extension, however you can specify whatever data format you like.  Currently supported are:
 * ini format
 * xml
 * PHP array

## Reading the global config file
To read the global config file, use the convention:


	$c = &SGL_Config::singleton();
	$conf = $c->getAll();

From this point on whenever you load the config singleton you'll get a reference to already loaded data, the file will not be re-read unnecessarily.

The Config object has a reference to a ParamHandler object which is a file parsing strategy based on the file extension read in the $configFile argument.  No intervention is required by the developer as file type handling happens automatically.

## Reading Additional Config files, eg Module Config
In this case you won't want an existing reference to the global config data, so create a new config instance and specify the path to the file you want to load:


	$myConf = new SGL_Config();
	$data = $myConf->load('/path/to/file')

If you then want to modify the $data, first associate it with the config object:


	$myConf->replace($data);

set some keys:


	$myConf->set('tuples', array('version' => SGL_SEAGULL_VERSION));
	$myConf->set('foo', array('bar' => $myValue));

and finally save the object:


	$ok = $myConf->save('/path/to/file');

## Config caching
Since version 0.6.2 all module config files are cached in the var directory.

 * *only the cached file is read*: If you take the config file from the default module, ie seagull/modules/default/conf.ini, it will automatically be cached in seagull/var/config/default.ini.  _Always_ reference the original config file for read and write operations, the caching and updating will be handled automatically.

 * *clearing the cache when you make changes*: If you manually alter a value in a module's conf.ini file, you will need to use "_Maintenance -\> Delete cached configuration files_" to update the config cache.  If you use the GUI to edit a config value, cache updating will be handled for you.


For the module config files, the ini format because it extremely fast to parse with PHP's parse\_ini\_file(), notably faster than xml, and there is generally no sensitive info associated with conf.ini files.

See the [php manual][1] for an example ini-file.

## Loading, Saving and Merging Config Data
To modify the global config, read its contents into a hash:

	$c = &SGL_Config::singleton();
	$data = $c->load($configFile)

You can also read multiple config files, and merge the results into a single config object:


	$c->merge($data);

set a few more keys:


	$c->set('tuples', array('version' => SGL_SEAGULL_VERSION));
	$c->set('foo', array('bar' => $myValue));

and finally save the object:


	$ok = $c->save('/path/to/file');


As with loading, the file extension you specify determines the format of the saved data.

If you want a single data source to override the existing data in the content object, use the replace method:


	$c->replace($data);

## Accessing config data
Config variables can be called as follows.

Within any manager, $this-\>conf is inherited from the SGL\_Manager parent class:

	$c = &SGL_Config::singleton();
	$this->conf = $c->getAll();

so call specific keys like:

	if ($this->conf['banIpEnabled']['enabled']) { // do something }

or


	if ($c->get(array('banIpEnabled' => 'enabled'))) { // do something }


To set a value *never* use the array referenced by $this-\>conf, it is a copy of config values for read-only purposes.  Instead use:


	$c->set('tuples', array('version' => SGL_SEAGULL_VERSION));

## Safely testing Boolean Values
If you need to test a simple boolean in the config you can save a step by using empty():



	if (!empty($this->conf['FaqMgr']['commentsEnabled'])) { .. }

is the same as


	if (isset($this->conf['FaqMgr']['commentsEnabled']) && $this->conf['FaqMgr']['commentsEnabled'] == true) { .. }

So in cases where the key doesn't exist, you won't get a notice with the empty() test.

## Module Config
As stated above, all the modules have a config file located at


	seagull/modules/$module/conf.ini

This config file is ini file format by default, and is merged with the system config when a module is loaded.  Its value can be called in the same manner as above.  Use this file to set module-specific config info.

### Editing modules' config files using the browser
As of version 0.6.0 you can edit the config files via the browser, simply login as admin which takes you to the module list screen by default, and click on a module's title to edit that module's config file.

## Loading custom Config files
As of version 0.6.1 of Seagull, you can load an additional config file from a custom location.  Simply specify the path in the global config.  If the value for $conf[path][pathToCustomConfigFile] is non-empty, the SGL\_Task\_LoadCustomConfig task will attempt to load the file.  This can be useful for setting constants for your custom roles, etc.


## Example
The $conf array returned typically looks like this:


	Array
	(
	    [db] => Array
	        (
	            [type] => mysql_SGL
	            [host] => localhost
	            [protocol] => unix
	            [socket] => 
	            [port] => 3306
	            [user] => root
	            [pass] => 
	            [name] => seagull_bugfix
	            [postConnect] => 
	            [prefix] => not implemented yet
	        )
	
	    [site] => Array
	        (
	            [outputUrlHandler] => SGL_UrlParser_SefStrategy
	            [name] => Seagull Framework
	            [showLogo] => logo.gif
	            [compression] => 0
	            [outputBuffering] => 0
	            [banIpEnabled] => 0
	            [denyList] => 
	            [allowList] => 
	            [tidyhtml] => 0
	            [blocksEnabled] => 1
	            [safeDelete] => 1
	            [frontScriptName] => index.php
	            [defaultModule] => default
	            [defaultManager] => default
	            [defaultArticleViewType] => 1
	            [defaultParams] => 
	            [templateEngine] => flexy
	            [wysiwygEditor] => tinyfck
	            [extendedLocale] => 0
	            [localeCategory] => 'LC_ALL'
	            [adminGuiTheme] => default_admin
	            [defaultTheme] => default
	            [serverTimeOffset] => UTC
	            [baseUrl] => http://localhost/seagull/branches/0.6-bugfix/www
	            [description] => Coming soon to a webserver near you.
	            [keywords] => seagull, php, framework, cms, content management
	        )
	
	    [cookie] => Array
	        (
	            [path] => /
	            [domain] => 
	            [secure] => 0
	            [name] => SGLSESSID
	        )
	
	    [session] => Array
	        (
	            [maxLifetime] => 0
	            [extended] => 0
	            [singleUser] => 0
	            [handler] => file
	            [allowedInUri] => 1
	        )
	
	    [cache] => Array
	        (
	            [enabled] => 0
	            [lifetime] => 86400
	        )
	
	    [debug] => Array
	        (
	            [authorisationEnabled] => 1
	            [sessionDebugAllowed] => 1
	            [customErrorHandler] => 1
	            [production] => 0
	            [showBacktrace] => 0
	            [profiling] => 0
	            [emailAdminThreshold] => PEAR_LOG_EMERG
	            [showBugReporterLink] => 1
	            [showUntranslated] => 1
	        )
	
	    [translation] => Array
	        (
	            [addMissingTrans] => 0
	            [fallbackLang] => en_iso_8859_15
	            [container] => file
	            [installedLanguages] => pt_br_iso_8859_1,zh_tw,zh,zh_utf_8,zh_tw_utf_8,cs_iso_8859_2,en_iso_8859_15,fr_iso_8859_1,fr_utf_8,de_iso_8859_1,it_iso_8859_1,no_iso_8859_1,pl_iso_8859_2,pt_iso_8859_1,ru_win1251,es_iso_8859_1,es_utf_8,sv_iso_8859_1,tr_iso_8859_9,tr_utf_8
	        )
	
	    [navigation] => Array
	        (
	            [enabled] => 1
	            [renderer] => SimpleRenderer
	            [driver] => SimpleDriver
	            [stylesheet] => SglDefault_TwoLevel
	        )
	
	    [log] => Array
	        (
	            [enabled] => 0
	            [type] => file
	            [name] => var/log/php_log.txt
	            [priority] => PEAR_LOG_EMERG
	            [ident] => Seagull
	            [paramsUsername] => 
	            [paramsPassword] => 
	        )
	
	    [mta] => Array
	        (
	            [backend] => mail
	            [sendmailPath] => /usr/sbin/sendmail
	            [sendmailArgs] => -t -i
	            [smtpHost] => 127.0.0.1
	            [smtpLocalHost] => seagull.phpkitchen.com
	            [smtpPort] => 25
	            [smtpAuth] => 0
	            [smtpUsername] => 
	            [smtpPassword] => 
	        )
	
	    [email] => Array
	        (
	            [admin] => demian@muse23.com
	            [support] => demian@muse23.com
	            [info] => demian@muse23.com
	        )
	
	    [popup] => Array
	        (
	            [winHeight] => 500
	            [winWidth] => 600
	        )
	
	    [censor] => Array
	        (
	            [mode] => 0
	            [replaceString] => *censored*
	            [badWords] => your,bad,words,here
	        )
	
	    [p3p] => Array
	        (
	            [policies] => 1
	            [policyLocation] => 
	            [compactPolicy] => CUR ADM OUR NOR STA NID
	        )
	
	    [tuples] => Array
	        (
	            [version] => 0.6.0RC2
	        )
	
	    [path] => Array
	        (
	            [installRoot] => /var/www/html/seagull/branches/0.6-bugfix
	            [webRoot] => /var/www/html/seagull/branches/0.6-bugfix/www
	        )
	
	    [table] => Array
	        (
	            [block] => block
	            [block_role] => block_role
	            [block_assignment] => block_assignment
	            [contact_us] => contact_us
	            [module] => module
	            [sequence] => sequence
	            [translation] => translation
	            [uri_alias] => uri_alias
	            [faq] => faq
	            [guestbook] => guestbook
	            [instant_message] => instant_message
	            [contact] => contact
	            [section] => section
	            [newsletter] => newsletter
	            [category] => category
	            [document] => document
	            [document_type] => document_type
	            [item] => item
	            [item_addition] => item_addition
	            [item_type] => item_type
	            [item_type_mapping] => item_type_mapping
	            [rndmsg_message] => rndmsg_message
	            [login] => login
	            [organisation] => organisation
	            [organisation_type] => organisation_type
	            [org_preference] => org_preference
	            [permission] => permission
	            [preference] => preference
	            [role] => role
	            [role_permission] => role_permission
	            [user] => usr
	            [user_permission] => user_permission
	            [user_preference] => user_preference
	            [user_session] => user_session
	        )
	
	    [ConfigMgr] => Array
	        (
	            [requiresAuth] => 1
	            [adminGuiAllowed] => 1
	        )
	
	    [DefaultMgr] => Array
	        (
	            [requiresAuth] => 
	        )
	
	    [ModuleConfigMgr] => Array
	        (
	            [requiresAuth] => 
	        )
	
	    [ModuleMgr] => Array
	        (
	            [requiresAuth] => 1
	            [adminGuiAllowed] => 1
	        )
	
	    [BugMgr] => Array
	        (
	            [requiresAuth] => 
	        )
	
	    [MaintenanceMgr] => Array
	        (
	            [requiresAuth] => 1
	            [adminGuiAllowed] => 1
	        )
	
	    [PearMgr] => Array
	        (
	            [requiresAuth] => 1
	            [adminGuiAllowed] => 1
	        )
	
	    [ModuleGenerationMgr] => Array
	        (
	            [requiresAuth] => 1
	            [adminGuiAllowed] => 1
	        )
	
	    [TranslationMgr] => Array
	        (
	            [requiresAuth] => 1
	            [adminGuiAllowed] => 1
	        )
	
	    [localConfig] => Array
	        (
	            [moduleName] => default
	        )
	
	)

[1]:	http://www.php.net/manual/en/function.parse-ini-file.php
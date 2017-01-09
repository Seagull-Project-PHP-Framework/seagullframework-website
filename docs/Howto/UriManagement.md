<!-- Name: Howto/UriManagement -->
<!-- Version: 16 -->
<!-- Last-Modified: 2007/10/18 11:12:13 -->
<!-- Author: demian -->
# URI Management
[[TOC]]
## Intro
This page shows how to use the the front controller functionality in Seagull, and how to input and output URIs are dealt with.

The aim of the Front Controller functionality is to enable Seagull to use search-engine-friendly URIs that are
  * easy to bookmark;
  * compatible with the widest range of search engine spiders;
  * easy to drill down to application functionality if required;
  * trackable by traffic analysis programs such as Webalizer.

For some background on clean URIs see here: http://www.port80software.com/support/articles/nextgenerationurls

The discussion breaks down into
 * input URIs - GET requests that are parsed by Seagull
 * output URIs - URI strings that make up the hyperlinks embedded in the generated HTML pages

## Input URIs
### Format
The URI format that Seagull expects is:

http://example.com/index.php/user/login/action/list/foo/bar/

|| *scheme* || http ||
|| *domain* || example.com
|| *front controller scriptname* || index.php
|| *module name* || user
|| *manager name* || login
|| *action name* || list
|| *URI params* || foo=bar


So the following URI:

http://localhost/seagull/www/index.php/user/login/action/list/foo/bar/baz/quux/

would be parsed by the SGL_Request object as:


    Array
    (
        [frontScriptName] => index.php
        [moduleName] => user
        [managerName] => login
        [action] => list
        [foo] => bar
        [baz] => quux
        [SGLSESSID] => 501d9d8b4452f84421fc214dd3fe7979
    )

and stored as a request singleton.  For more info on the Request object [see here](/wiki:Howto/WorkingWithTheRequestObject/).

The following variations of this format are supported:


    http://localhost/foo/
    http://localhost/foo/bar/
    http://localhost/foo/bar/key/value/
    http://localhost/foo/bar/action/list/
    http://localhost/foo/bar/action/list/key/value/

### URI Parsing Strategies
When the SGL_Request object is first initialised, it loads an array of configurable strategies that are used to parse the current request.  Each parsing strategy will attempt to match expected patterns in the URI.  By default 3 strategies are always invoked, but devs can customise the way URIs are parsed by loading and reordering the strategies.  The code that does the job looks like this:


        require_once dirname(__FILE__) . '/UrlParser/SimpleStrategy.php';
        require_once dirname(__FILE__) . '/UrlParser/AliasStrategy.php';
        require_once dirname(__FILE__) . '/UrlParser/ClassicStrategy.php';
    
        $aStrats = array(
            new SGL_UrlParser_ClassicStrategy(),
            new SGL_UrlParser_AliasStrategy(),
            new SGL_UrlParser_SefStrategy(),
            );
        $uri = new SGL_URL($_SERVER['PHP_SELF'], true, $aStrats);

And the default strategies are:
 * classic - for standard URIs like http://example.com/index.php?moduleName=foo&managerName=bar&baz=quux
 * alias - for user-defined aliases stored in the database, eg: http://example.com/this-is-my-uri/
 * SEF - for the Seagull Search Engine Friendly format, eg: http://localhost/foo/bar/action/list/key/value/

The resulting $uri object is cached so potentially expensive parsing is minimised.

### SGL_Url and SGL_Request APIs
After parsing, a summary of the most relevant request data is stored in the SGL_Request object:


    $aQueryData = $uri->getQueryData();
    $this->aProps = array_merge($_GET, $_FILES, $aQueryData, $_POST);

and is made available through basic [getters and setters](http://api.seagullproject.org/SGL/SGL_Request.html). 

The more detailed information stored in the URI object is still available and is accessible through the URI API, see the [documentation](http://api.seagullproject.org/SGL/SGL_URL.html) for more info.

### Problems Caused Setting Cookie Paths

When using SEF URIs and you are setting cookies in javascript you MUST explicitly set cookie path otherwise current URI will be used (the problem doesn't happen when using standard URIs, because browser strips everything after final / character).
e.g. 

classic URI: http://computer.name/seagull/www/tdbMgr.php?element_id=86&toId=13&treeParent=566
cookie path will be http://computer.name/seagull/www/

Seagull URI: http://computer.name/seagull/www/index.php/tdb/element_id/86/toId/13/treeParent/566/
cookie path will be http://computer.name/seagull/www/index.php/tdb/element_id/86/toId/13/treeParent/566/

so use something like this to set path
document.cookie = 'foo=bar ; path=/'

### Server Config Requirements
The way the URIs are implemented, no modifications to your server config are required whatsover.  By the same token, you don't need to be running Apache, the system should work equally well for IIS, etc. 

### Flexibility
There is quite a bit of flexibility in the format:

  * the front controller scriptname is user-configurable; in addition, you could use Apache's ForceType directive to remove the requirement for a file extension, ie.:


    <Files article>
      ForceType application/x-httpd-php
    </Files>

which would allow you to use a URI like

http://example.com/obidos/user/login/action/list/foo/bar

or

http://example.com/seagull/user/login/action/list/foo/bar

  * front controller scriptname can be removed altogether if you're using Apache and have mod_rewrite compiled in, to do this:
    * edit the global config file, seagull/var/<servername>.conf.php, and set the following: 


    $conf['site']['frontScriptName'] = false;

    * copy [browser:/branches/0.6-bugfix/etc/htaccess-cleanUrl.dist] to your seagull/www directory and rename to ".htaccess"
  * the module and manager components of the URIs are not case sensitive.  The following URIs therefore are all acceptable, however it is recommended you use all lowercase whenever possible:


    http://example.com/index.php/UsEr/LoGiN/action/list/foo/bar
    http://example.com/index.php/USER/login/action/list/foo/bar

  * the 'Mgr' suffix in the class names is optional - since it is used in the vast majority of module classes, it would otherwise appear redundant in the URI.  So the following are both acceptable:


    http://example.com/index.php/user/loginMgr/action/list/foo/bar
    http://example.com/index.php/user/login/action/list/foo/bar


  * the module or manager name is optional if they are identical.  In other words, many modules have a default manager class which has the same name (at least in the URI) as the module it resides in.  This would create redundant URIs like the following:


    http://example.com/index.php/faq/faq/action/list/foo/bar
    http://example.com/index.php/user/user/action/list/foo/bar


Therefore it is recommended not to explicitly call the default manager class name as it is already implied in the module name.

  * the action parameter is also optional: in cases where it is not present, the default action set in the manager class will be called.  Therefore the following two URIs are equivalent:


    http://example.com/index.php/user/login/action/list/foo/bar
    http://example.com/index.php/user/login/foo/bar


## Output URIs
### Generating URIs in the Templates
Use the makeUrl() method from the Output object which is available in the template scope.  Here is an example:


    This is a <a href="{makeUrl(#edit#)}foo/{bar}">link</a>

The *makeUrl()* method accepts 3 optional literal arguments in order of importance: action, managerName, moduleName.  For any value that is not supplied, the current value will be supplied.  In other words, if you're in a template for the FAQ manager, and you specify no params to *makeUrl()*, the current values will be supplied, ie.:



    action = list
    managerName = FaqMgr
    moduleName = faq


You could use this format to send only querystring params to the script.  The *makeUrl()* method also supplies the following functionality:

  * scheme type will be detected and perpetuated, ie. if you're in `https` mode, the link generated will also be in `https`;
  * if you're browsing with cookies disabled, session info will be automatically added to the URI.

When iterating through resultsets, use the following format:

    This is a <a href=
    "{makeUrl(#edit#,#user#,#user#,aPagedData[data],#frmUserID|usr_id||frmEmail|email#,key)}">complex link</a>

Whereas this causes the parameters frmUserID and frmEmail to be set to the following values:
 * frmUsrID = aPagedData[data][key][usr_id]
 * frmEmail = aPagedData[data][key][email]

The args are as follows:
 1. action
 1. manager
 1. module
 1. array of arrays or objects representing data collection
 1. `frmVariableName1|dataFieldName1||frmVariableName2|dataFieldName2`
 1. array index

If you're not interating through an array of object results, pass in a single object like this:


    {makeUrl(#profil#,#klienci#,#klienci#,##,#frmUserID|$oNews->usr_usr_id#)}

### Outputting the current URL in the Templates
If you're generating anchors on a page or something similar, there are cases where you need the current url output to the template [getCurrentUrl() is available from version 0.6.1 onwards].  To generate this use:


    <a href="{getCurrentUrl()}">foo</a>

or in the case of comment anchors, something like:


    <a href="{getCurrentUrl()}#comment{increment(key)}">#{increment(key)}</a>


### Generating URIs in a module
If for some reason you need to generate an URI in a module, use SGL_Output::makeUrl(). The arguments are the same as above, so the code here will be:

    SGL_Output::makeUrl('profil','klienci','klienci',array(),'frmUserID|'.$oNews->usr_usr_id);

### Generating URIs in JavaScript
You can create URIs in JavaScript, the format is like this:


    var myUri = makeUrl({'module':'mymodule', 'action':'myaction', 'param2': 'foo bar'});
or

    var myUri = makeUrl({'module':'mymodule', 'manager':'mymanager', 'action':'myaction', 'param2': 'foo bar'});
module and action params are mantadory, you can also specify a manager, other 'name':'value' pairs are added to the end of uri

[[AddComment]]
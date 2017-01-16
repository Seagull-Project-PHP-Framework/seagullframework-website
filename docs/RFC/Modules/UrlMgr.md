<!-- Name: RFC/Modules/UrlMgr -->
<!-- Version: 9 -->
<!-- Last-Modified: 2006/01/11 18:48:44 -->
<!-- Author: demian -->
pls see http://trac.seagullproject.org/wiki/RFC/Modules/UriHandling

UrlMgr is the module that tries to implement URL Aliasing and URL Redirection for SGL.

This is draft just to explain the concept.

You can see a working version at: [http://www.itromania.ro/urlmgr/www/index.php/src-article]

The 'Src Article' contains links that refer to the same page but with pageTitle, pageDescription, pageKeywords changed page.

*Description:*
The magic takes place in Url.php where we check if the requested URL
    

    $reqUrl = $this->protocol.'://'.$this->host.$this->path;

is found in db or not. If it is then we initialize the $pageTitle, $pageDescription and $pageKeywords variables (as they were passed in the URL) with the data from db and they are available like:

    $input->pageTitle = $req->get('pageTitle') 
This variables can be ignored or overwritten by the user. In this way we can set different keywords for different URLs that refer to the same content (good for SEO) . If 'redirect' is set we redirect to the specified address. If you put in the 'redirect' field a full URL (!http://...) then you'll make a redirect, else just an alias.

If the URL is not found in db then the process continues the normal flow with the original passed URL.

This is a very clean and simple  version of  handling  URL aliases. It can be used regardless of URL parsing schema and it assures compatibility to older and future versions.

What is good to know that we compare the protocol,host and path of the URL so we can have:
www.dom.com/nice-article
nice.article.dom.com/article
article.dom.com/nice
refer to the same page.

URL Mgr is using cache so it has minimal impact on exec time.


*Change log:*

1.3 - 04/01/2006
 * This is just a cleaned up version of the code.
   More info: [http://marc.theaimsgroup.com/?l=seagull-general&m=113510323005926&w=2]

1.2 - 20/12/2005
 * On static article delete, check if we have a reference to it in URL db and delete from there too. (By the reference field)
 * If the URL is not in db and you are 'admin' then show a page 'This page does not exist. Do you want to create it'. If yes, an empty static article will be created and added to the url db. This is a wiki like comportment. (This is supported by the URL Block too)

1.1 - 16/12/2005
 * Moved the UrlMgr code as /lib/SGL/UrlParserAliasStrategy.php (Thanks for the examples with the aliases stored as an array)
 * Added the block that allow you to add a link or an article at the current URL
 * Added the 'reference' field so we know what aliases to what items are they related
 * If you add an alias like /this/is/a/nice/path it will be put into categories : this -> is -> a -> nice -> path (one step closer to the navigation tree and site map page)
 * Possibility to add URLs in navigation bar.
1.0 - 29/11/2005
 * Initial features



*What I want to do next:*
 * Create navigational category tree.
 * Generate a 'Site Map' page.
 * At the 'This page does not exist. Do you want to create it' page, add the possibility to add other items beside static articles. Ex: faq, products, news, etc.

*To Do:*
 * Make UrlMgr compatible with URL passed SessionId
 * Make UrlMgr compatible with the host:port format

Please add you comments bellow.

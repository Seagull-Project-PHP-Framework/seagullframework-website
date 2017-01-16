<!-- Name: RFC/Modules/UriHandling -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/05/26 10:40:46 -->
<!-- Author: demian -->
# URI Handling
This RFC takes into account the ideas from Rares work [here](/wiki:RFC/Modules/UrlMgr/) which was not implemented due to code compatibility issues.

Much of the functionality has since been implemented, however there are a number of features outstanding:

## Aliases
 * the alias functionality does not currently support URIs of the type example.com/foo/bar/baz or better yet example.com/foo/bar/baz.html
 * aliases should be used to implement 401, 403 and 404 type HTTP errors
 * "page not found, create a new page here" would not be difficult to integrate

## URI Creation
We are currently stuck with the makeLink() function available to Flexy templates and also for use within Manager scope.  It is clumsy to use and long-winded.  One much simpler solution would be directly passing string literals to the function:


    This is a <a href="{makeLink(#/user/account/action/edit#)}">link</a>

At least the following distinctions would be needed, here are some suggestions:


    This is a <a href="{makeLink(#sgl:user/account/action/edit#)}">link</a>
    This is a <a href="{makeLink(#sgl:publisher/article/action/edit/frmArticleId/$article_id#)}">link</a>
    This is a <a href="{makeLink(#external:http://example.com#)}">link</a>
    This is a <a href="{makeLink(#alias:my-custom-alias-link#)}">link</a>
    This is a <a href="{makeLink(#alias:path/to/my/resource#)}">link</a>
    This is a <a href="{makeLink(#img:myImage.png#)}">link</a>

To maintain compatibility with the huge amount of templates that use the current makeLink() format, a new method could be created, perhaps makeLink2() ?

----
more suggestions:


    
    name/$1/$2/$3 => module/manager/year/$1/month/$2/day/$3
    	1	=> \d{4}
    	2	=> \d\d
    	3	=> \d\d



    [CalendarMgr]
    ...
    [aliases]
    "aliasName/$1/$2/$3" = "module/manager/year/$1/month/$2/day/$3"

should be used printf for aliasing. I mean 


    aliasName/%1/%2/%3 => module/manager/year/%1/month/%2/day/%3



    name/(\d*)/(\d*)/(\d*) => module/manager/year/$1/month/$2/day/$3




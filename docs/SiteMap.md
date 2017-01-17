<!-- Name: SiteMap -->
<!-- Version: 5 -->
<!-- Last-Modified: 2007/05/10 09:37:06 -->
<!-- Author: demian -->

# Sitemap Builder
* TOC
{:toc}

## Protocol

Sitemap extension for Export module of Seagull serves the purpose of exporting your sitemap in xml format readable by search engines. The protocol is adopted/supported by all major players in search industry. For details about it please visit [http://www.sitemaps.org/protocol.php][1].


## Generating Sitemaps

First of all you have to install export module in admin interface of your installation (it could be done trough Admin-\>General-\>Manage Modules), it depends on Publisher module so install it first if you don\`t have it in place. Once it is successfully finished, just copy the files attached to this page over your seagull install directory. Everything should be in place now.

You can test whether everything works as intended by simply checking <yourdomain>/sitemap.php. If you get some output & its not some kind of error message, it works. If you check the source of the page it should be xml.
 

You probably noticed that a new www/sitemap.php file was created. This is required by search engines because they dont accept sitemap file to be in sub-directory, so the generator should be placed in wwwroot directory of your installation, to be able to describe the content of whole site. It simply invokes Sitemap manager.

The generator uses strategy pattern to generate the URLs for your site. Some of strategies are already implemented such as:

 * *Navigation* : generates links from publicly accessible navigation sections
 * *Article* : generates links for your published articles
 * *News* :  generates links for your published news
 * *Staticurls* : enables you to define static url you want to be indexed, basically to hardcode them by hand (could be handy for exceptions, which we know every rule has:)

You may want to change priority & changefreq each strategy assigns to generated urls. It could be done via overriding inherited member variables in each strategy with same names.

The generator supports multilingual sitemap generation, it can be turned on/off, configurable trough respective conf.ini.

The generated sitemap is cached, global cache options are used.

## Extending Sitemaps

You can easily extend your sitemap generator by creating a new child of SGL\_Sitemap\_Strategy class, and adding it to strategies used in modules/export/conf.ini in Sitemap section. Look for examples of doing this in current implementation, its really simple to do (I have already done it, so this statement is based on xp :).


## Propagate your Sitemap
You can submit the location of your sitemap to each search engine separately, but there is an easier way around. Let the bots read its location. You would probably want to create www/robots.txt. This file is automatically read by our "little" friends. An example of its content:


	User-agent: *
	Sitemap: http://<yourdomain>/sitemap.php

## Todo
If this extension will be successful, module maintainers/developers should write strategies for their modules as well, if there is some content they would like to get indexed.

Navigation strategy should be improved as well because it doesn’t support drivers & complex resource\_uri-s.

Any feedback is welcome, & I will try to be quick on bugfixes. 


 

[1]:	http://www.sitemaps.org/protocol.php
<!-- Name: Integration/FUDforum -->
<!-- Version: 25 -->
<!-- Last-Modified: 2007/03/17 14:01:21 -->
<!-- Author: jcasanova -->

# FUDforum Forum Integration
[[TOC]]

## Intro
The link is http://www.wastetomarket.com/ 
If you click on forum, you can see that I am using FUDforum with my
seagull site's header format and CSS. 

FUDforum is not what I would call truly 'embedded' into Seagull, just
integrated to provide a common look and single sign-on, so that the user
would not be aware of working with separate apps, and it seems to be
doing the trick, but it took a bit of fiddling to get it to work.

## FUDforum Install
I created a 'forum' folder in seagull/modules, and in seagull/www as
well. 
Then I ran the fudforum installer, and told it to place the FUDforum
libraries in seagull/modules/forum/FUDforum, to keep the
non-web-accessible stuff safe. Then I had it put the web-accessible part
of the forum in seagull/www/forum.

## Integrating Templates
I edited the templates in FUDforum to use the css import that Seagull
0.5.x uses by calling style.php in my forum header, and tack on 'forum'
as the module. 
Partially shown here (you can see the whole thing in the forum html
source): style.php?navStylesheet=SglDefault\_MultiLevel&moduleName=forum

Then I just took the forum.css file that comes with FUDforum, and saved
it as forum.php in seagull/www/themes/myTheme/css/ , and now I am able
to use CSS variables from vars.php to produce the correct colors in the
forum, since seagull looks for a modulename.php in the css folder when
you specify a module in the css request. 

I also edited the header template in FUDforum - I placed a div with ID
'sgl' in the header, as well as a nested div called 'header', in which I
had fudforum echo the sitename, so that the #sgl #header style would
apply to the header of the forum, rather than create an additional
style.

## Integrating Navigation
I also changed the navigation template of the forum so that it is
contained in the ul/li structure that Seagull places it in, to mesh with
the stylesheet layout. 

## Single Sign On
As far as single sign-on, FUDforum provides a script called
forum\_login.php that can be easily called from seagull since the forum
is installed inside seagull's directory structure, I just use something
like require SGL\_MOD\_DIR . '/forum/FUDforum/scripts/forum\_login.php' and
then call the forum login/logout functions from within the LoginMgr -
conveniently creates a FUDforum cookie upon logon to Seagull, and
removes it on logout. 

This function requires a uid, so it is much easier if you can sync up
your forum userid #'s with your seagull userid #'s, as I was able to do
(by calling FUDforum's api to create a forum user upon new seagull user
registration or import, and having both user tables' sequences at the
same point.)
That's not necessarily the best way, or even possible for all sites, so
you may need to do something such as create a reference table containing
your forum userid's and seagull id's in order to keep track of those. 
Also should be noted, FUDforum's api scripts contain a few bugs that I
had to track down to get them working properly. 

I'm still in the process of cleaning a few things up, and I have to turn
off user registration in FUDforum and login as well, so that everything
is handled through the Seagull site. 

-- Clay Hinson

## More Single Sign On
 * in seagull/modules/forum/scripts/forum\_login.php modify the following:


	$GLOBALS['PATH\_TO\_FUD\_FORUM\_GLOBALS\_PHP'] = dirname(__FILE__).'/GLOBALS.php';

 * after the comment block in the same file, define the missing constant:


	if (!defined('__request\_timestamp__')) {
		define('__request_timestamp__', time());
	}
	  
 * replicate your Seagull user table to fud26\_users or equivalent
 * as you've setup FUD in the normal way, keep the two user records that it created for you: 1 for anonymous user, 2 for admin
 * after those records insert all your Seagull users, *not including* Seagull anon or admin records
 * minimum required fields are id, login, alias, theme and users\_opt
 * set theme = 1 for all users
 * set users\_opt = 4357110 for all users except the admin user
 * remove the UNIQUE indexes on login, email and alias
 * remove the auto-increment as Seagull will control the primary keys

## Single Sign On - Round 3
 * to disable password checking for settings updates (Seagull should manage passwords, not FUD), do the following:
   * make your FUD files writable using admin setting
   * edit seagull/www/forum/theme/default/register.php
   * comment out lines 1723 - 1729
 * disable FUD registration
   * this can be done in FUD admin screen
 * to remove settings -\> Required Information
   * login to FUD as admin
   * Admin Control Panel -\> Template Management -\> Template Editor -\> (edit) Template Set Selection -\> register.tmpl
   * click thh 'update\_user' section
   * comment out all fields except email, which you can mark as disabled
   * anything that's disabled or commented out must be passed as a hidden field
 * removing register and login navigation links
   * go to template editor as above
   * edit usercp.tmpl  and comment out links
 * removing quick-login link from forum homepage
   * manually edit /modules/forum/thm/default/tmpl/index.tmpl
   * around line 99 you'll find the following, remove it.  Once you change that you’ll have to rebuild your theme from Theme Manager in the FUD admin CP.
 * remove 'post password' from new post template
   * go to template editor as above, select post.tmpl, then post\_password, then comment out html, but reinstate the password input as a hidden field


	{IF: __fud\_real\_user__}{TEMPLATE: quick\_login\_loged\_in}{ELSE}{TEMPLATE: quick\_login\_on}{END}


-- Demian Turner

*NOTE:* target integration is for fudforum 2.7.4
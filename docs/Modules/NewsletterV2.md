<!-- Name: Modules/NewsletterV2 -->
<!-- Version: 3 -->
<!-- Last-Modified: 2005/11/15 15:29:07 -->
<!-- Author: aj -->
## Newsletter module - V2

The newsletter module is used to send text and HTML formatted emails to subscribed and registered users. 

Tested on 28-Feb-2005 CVS version but it should be compatible with older versions too. It updates the NewsletterMgr.php 1.20v file.

### New Features:
- unregistered SGL users can subscribe too
- multiple news lists to subscribe to
- email confirmation with auth key available for actions (un/subscribe, update etc.)
- subscribers administration (add/edit/delete)
- news lists administration (add/edit/delete)
- added possibility to change the From: field of the newsletter
- subscribe block available
- removes duplicate emails when sending newsletter to users that are in more then one list / group

### Features:
- send newsletter to registered SGL users
- use HTMLArea as editor
- address book

### Install:
 - download the newsletter.zip: [http://www.bluestardesign.ro/seagull/newsletter.zip](http://www.bluestardesign.ro/seagull/newsletter.zip)
 - please take a look at the /etc and /lib files that are modified. The ~/lib/SGL/Sql.php file contains the patch for the install bug (read ML : CVS SGL fresh install generate SQL error )  and /lib/SGL/util.php contains a patch so we can use Flexy for newsletter. Hope that somebody will create a _Flexy_singleton()_
 - apply over SGL code (should be compatible with already installed SGL). 
 - if SGL already installed create SGL table from schema.my.sql and rebuild entities, add the newsletter table to default.conf.ini file 
 - add perms for Newsletter class so (guest, members etc.) can un/subscribe (by default no perms are set and only the admin can use the pages)
 
### Other info:
 - insert the subscribe block if you want
 - the user un/subscribe page is /subscribe.php
 - if you delete a list, all the subscribers from that list are deleted too
 - there is an *update* undocumented action available. If set and valid email/key entered, the last_updated field in DB is updated. This is useful if you want to renew the subscriptions.

Please comment on this module at the [RFC page](/wiki:RFC/Modules/Newsletter/). Also please notify any bugs found.

Thanks,

Rares
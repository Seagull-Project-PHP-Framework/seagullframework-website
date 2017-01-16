<!-- Name: RFC/Modules/Guestbook -->
<!-- Version: 3 -->
<!-- Last-Modified: 2005/11/13 19:33:08 -->
<!-- Author: werner -->
# RFC for Guestbook Module

  * add edit/delete/hide functionality. use AJ's new item stuff for that?
  * how about guestbook spam? -> ip logging etc.
  * http://www.jtr.de/scripting/php/ JAX-GB has a nice smiley functionality. online demo available here: http://www.jtr.de/scripting/php/guestbook/index.html
  * send email to gb-admin when new entry
  * JAX-GB (see above) has a "quicklink" functionality: in the admin-emails have a link to the admin-section of the gb. this needs "redirect after login" functionality in SGL
  * option to show emailadresses like _me AT foobar DOT com_
    * this could to to `output class` ? [/WernerKrauss WernerKrauss] /08.02.2005 21:42/
  * add pager
  * max size of comment? there are nice js functions that show the remaining characters.
  * every user should have the ability to have it's own guestbook
    * so guestbook_id = user_id, if guestbook_id = 0 the main guestbook
  * does a "sort by language" make sense? [/WernerKrauss WernerKrauss] /08.02.2005 21:42/
  * add a "captcha" pic for spam prevention [/WernerKrauss WernerKrauss] /11.02.2005 19:12/

It would be great to have:
  * in the e-mail sent to gb-admin to have 2 links: 'accept entry' and 'deny entry' so you can quick admin
  * set a default policy: the message is displayed immediately or only after the admin accepted it 
  * add category for messages so you can group them
  * maybe think the module as a general 'Comments' module so you can insert comments to any page in SGL
  [/RaresBenea RaresBenea] /08.02.2005 14:42/
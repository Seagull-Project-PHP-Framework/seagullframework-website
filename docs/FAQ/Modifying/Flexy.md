<!-- Name: FAQ/Modifying/Flexy -->
<!-- Version: 9 -->
<!-- Last-Modified: 2006/10/07 01:22:09 -->
<!-- Author: lyric -->

# Templates FAQs
* TOC
{:toc}

[[SubWiki(FAQ)]]

## How to I get rid of \>angle brackets\< around words?
I changed a value in userManager.html from "{translate(#add user#)}" to "{translate(#add child#)}". Now  I get "\>add child\<" where "add user" used to be.

Well, if you try to output something via _{translate}_ that cannot be found in the language file. So:
  1. check out <seagull-install>/modules/user/lang/english-iso-8859-15.php
  1. edit this file (entry _$words['add user']_) or create a new one with _$words['add child'] = 'add child';_
  1. be sure you update the other tranlsations. You can use the 'Manage Translations' section of the Default manager for this.

## Is there any way to test a condition other than true/false (like $a=='foobar') with flexy?
Yes there is, using the isEqual() function, however, the beware of putting business logic in the template, generally a bad idea.

Note: look for this method in Seagull 0.6, not Seagull 0.4.x.
 
If you need custom methods, create them using [wiki:Howto/Templates/Flexy/Plugins]

## Flexy mangles my HTML
I have some strings like "<acronym title="Seagull Framework">Seagull</acronym>" to translate. But Flexy destroys the HTML and converts '\<' to '&lt'.

This is a standard Flexy feature, to send raw html just append :h in the template, like this


	<p>{translate(#test 123#):h}</p>

## Is there any way to turn Flexy off or even better all output after action is called?
If you need to dynamically create pdf, png etc. files, have a look at FileMgr.php in the publisher module. 

Basically you have to select an empty template file to keep Flexy happy and call exit() after having generated your output like in \_download() for instance.

## How can I format strings / numbers?
  * Optionally you can format data in your modules using [ sprintf][1] but it is better MVC practice to move all formatting to the templates
  * You may also have a look at [wiki:Howto/Templates/Flexy/Plugins] or see http://pear.php.net/manual/en/package.html.html-template-flexy.php
  * and comments in the Flexy.php lib like:
  \_ 'numberFormat'  =\> ",2,'.',','",  \_ default number format  {xxx:n}
  format = eg. 1,200.00''


## So I want to create my own templates...
look at [wiki:Howto/Templates/WorkingWithTemplates] for info on how to do this.

## How can i use PHP variables inside JavaScript code?

You can find more information in [wiki:Howto/Templates]

[1]:	http://de.php.net/manual/en/function.sprintf.php
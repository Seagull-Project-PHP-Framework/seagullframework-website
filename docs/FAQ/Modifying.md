---
layout: page
title: Modifying Seagull
permalink: /FAQ/Modifying.html
---

<!-- Name: FAQ/Modifying -->
<!-- Version: 8 -->
<!-- Last-Modified: 2006/03/29 01:02:29 -->
<!-- Author: demian -->
<!-- Status: In Progress -->

# Modifying Seagull
* TOC
{:toc}

[[SubWiki(FAQ)]]
## General
### How can I add PEAR packages?
You can add PEAR packages for using them in your own modules.
Obviously all PEAR classes have to go in the PEAR directory (/lib/pear/). You can either use the PEAR package manager (many tutorials at [PHPkitchen][1]) or copy the file there yourself.

If you want to use the PEAR package manager you have to set the install dir:

	pear config-set php_dir <your_path_to_seagull>/lib/pear/ 

Then simply install the PEAR-packages you want using

	pear install <package>

Don't forget to reset your settings for php\_dir after the installation.

### So I want to uninstall some modules...
At the moment you can hide a module from the "manage modules" screen or really uninstall a module per hand. There will be a function for that soon.

`cleanly remove`

  * remove module's folders (modules/ and www/theme/default/ directories)
  * remove module's entry in 'module' table
  * remove module's database tables

`hide`

  * remove module's entry in 'module' table

(Take a look at these two [seagull-general][2] [threads][3].)

### How can I change the User Module

	Is it possible to change the user-settings easily? 
	I mean I don't need the entries like Adress 2, Adress 3, State, Country 
	etc. But I need some others, like occupation, website ...

The way things currently work, you can manually edit your 'usr' table
and modify/add whatever attributes you want, regenerate DB\_DataObjects
from the Maintenance mgr, and everything will continue to work as
normal.

If you create your own theme (make a copy of default, but have it
include only files that are different from default, ie header.html,
footer.html, etc) then upgrades should be quite painless.  Only fields
that are important not to change are usr\_id, email and the FK fields.

Ideally we want to move towards pairing the usr table with a child
'additional\_attributes' table so upgrades would be more flexible, this
is on the radar for the future.


## Front Controller
### how is it supposed to deal with GET forms?
there's a few different approaches we've taken to incorporating FC into forms, it basically breaks down into these categories:

  * raw urls
  * javascript targeting

for something like `<form action="">` we would do the following:


	<form id="foo" name="foo" action="{makeUrl(#results#,#search#,#bar#)}" method="get" flexy:ignore>

this will produce an url as follows, when submitted:

_http://example.com/index.php/bar/search/action/results/?term=baz_

this is a somewhat messy mixture of FC and conventional styles but anything passed in the format `?foo=bar` becomes available in `$_GET` and is therefore usable by the script.

the second approach would be for select onChange events, we've used stuff like:

	<select name="groupBy" 
	onchange="document.location.href='{makeUrl()}frmOrgId/{page.orgId}/blockIndex/{blockIndex}/groupBy/'+ 
	getSelectedValue(self.document.actionBarBottomForm{blockIndex}.groupBy);">

### How about arrays inside the form?

	> For instance, an action link with this url:
	> 
	> http://.../module/action/myaction/sth[]/5
	> 
	> is considered invalid (FWICS the array
	> is changed to a string: "sth=5" instead
	> of "sth[]=5").
	> 
	> Can anyone confirm it?
we have a basic level of array parsing working.  using FC, PHP's magic GET/POST 
array parsing is no longer available and is mimicked in `SGL_Url::dirify()` and 
`getVarNames()`.  Form elements like:

	<select name="array[foo]">
	<select name="array[bar]">

which would produce, once built up, a big url like:

_http://example.com/index.php/max/report/action/reportBuilder/reportData[report\_id]/2/reportData[report\_id]/2/reportData[organisation\_id]/5/reportData[period]/today/reportData[format]/1/submitted/1/reportData[report\_id]/2/reportData[organisation\_id]/5/reportData[period]/today/reportData[format]/1/submitted/1_/

which is url-encoded to:

_http://example.com/index.php/max/report/action/reportBuilder/reportData%5Breport\_id%5D/2/reportData%5Breport\_id%5D/2/reportData%5Borganisation\_id%5D/5/reportData%5Bperiod%5D/today/reportData%5Bformat%5D/1/submitted/1/reportData%5Breport\_id%5D/2/reportData%5Borganisation\_id%5D/5/reportData%5Bperiod%5D/today/reportData%5Bformat%5D/1/submitted/1_/

this format is currently supported, to get your reportData[]/5 to work would be a small job.

while we're on the subject of url parsing, i think google (naturally) are doing 
it best, they build form pseudo objects which look much easier to deal with on 
the server side:

	<input type="hidden" name="creative.destUrl.protocol" value="http">
better mapping, no?

## Modifying the Database
### How can I modify the usr table to include some other biographical information?
I modify the Usr object/table with each new project, what's in the default 
distro is just a generic suggestion.  People have asked if they should extend 
and override the UserManager and Register class, I would say no, it's not worth 
it, just modify your table accordingly and regenerate dataobjects if you use these.

[1]:	http://www.phpkitchen.com
[2]:	http://marc.theaimsgroup.com/?l=seagull-general&m=109309345731449&w=2
[3]:	http://marc.theaimsgroup.com/?l=seagull-general&m=109311181632069&w=2
---
layout: page
title: How to setup a Virtual Host in Apache
permalink: /Howto/JavaScriptAndCssImprovements.html
---
<!-- Name: Howto/JavaScriptAndCssImprovements -->
<!-- Version: 3 -->
<!-- Last-Modified: 2008/01/23 16:49:31 -->
<!-- Author: demian -->

# CSS and Javascript reorganisation and optimisation
> since Seagull 0.6.3

* TOC
{:toc}

## PART I: CSS/Javascript optimizer
Remember `style.php` for each Seagull theme? We developed a replacement, 
which optimizes both Javascript and CSS files. 
`Optimizer` can be located in `SGL_WEB_ROOT/optimizer.php`. 

How to invoke optimizer: 
-  do same as you did before in your managers e.g.

`$output->addCssFile('themes/theme/css/my.css')`

or 

`$output->addJavascriptFile('js/my.js')`

- in `header.html` of your theme put the following lines 

`<link rel=“stylesheet” type="text/css" href="{makeCssOptimizerLink()}" />`
`<script type="text/javascript" src="{makeJsOptimizerLink()}"></script>`

That's it. 

How it works: 
 * optimizer receives file names of files to load and their type (css or javascript), 
 * then it looks if files were changed or not,  so if yes, it simply tells browser to use it's cache, 
* if no, it creates one file from all files names specified (just outputting their contents in one request) and ensures that next time browser will use it's cache to receive it, 
* for CSS optimization we also load `csshelpers.php` (found in `SGL_WEB_ROOT/themes`), which functions (helpers) can be used in CSS files like `isBrowserFamily`. 

But caching can happen by both sides. Optimizer ensures that browser receives cache for appropriate files, if it has it. But browser itself can cache the whole optimizer link, thus if files were changed we will still get old cache, because browser is too cool. To avoid this problem we add revision number to generated optimizer link to make sure optimizer will be launched if CSS/JS files were changed. 
 
### Notes: 
* `csshelpers.php` is nothing more than a replacement for old style `vars.php` and `helpers.php`, 
 * we need the ability to turn off/on optimization - useful for debugging (i.e. with `optimization` turned off it should each file separately), 
 * in your CSS files don't use `@import` rule, use PHP's `include`. 

And last but not least, optimizer doesn't optimize anything. It can be improved to compress Javascript code of the fly, gzip it or whatever.  Same with CSS - removing whitespaces for example. But this step should be done only after off/on stuff is developed, otherwise it will be impossible to debug. 

So we left some work you. :) 

## PART II: Javascript
I guess all of you agree that `SGL_WEB_ROOT/js` directory was messy, functions in it were redundant, no CS or whatever.   We fixed it. Now all Seagull Javascript files go either into modules 
`js` directory or in `SGL_WEB_ROOT/js/SGL`.   Some basic libraries already exist there, but it's almost nothing, so we need start populating it (e.g. merging from private projects etc). 

### Notes: 
* worth reading (and probably closing?)  [http://trac.seagullproject.org/ticket/1468][1], 
 * all old Javascript files (like `mainPublic.js`) were moved to `SGL_WEB_ROOT/themes/default_admin/js`, as you can see it is left  for BC with `default_admin` theme, 
 * `js/SGL.js` should be always loaded - it is main Seagull Javascript file (nothing more than old `global.js`). 

## PART III: CSS
This is the greatest improvement from my POV. It is what we actually called `default2` originally, nobody thought that we had to develop previous two steps. 

First of all I want to say that appearance of default theme didn't changed. It is absolutely the same, but the CSS code is completely different. 

To get a basic idea of new approach read the following - 
[http://www.contentwithstyle.co.uk/Articles/17/a-css-framework/][2]. Mike 
Stenhouse is the guy whose approach we incorporated into Seagull. 
The main idea is too easy new theme development. We used modular 
approach to split CSS in different files, where every file does it's own 
job. The full list of default files are as follows: 

	* themes/default/css/reset.css, 
	* themes/default/css/tools.css, 
	* themes/default/css/typo.css, 
	* themes/default/css/forms.css, 
	* themes/default/css/layout.css, 
	* themes/default/css/blocks.css, 
	* themes/default/css/common.css. 

Also we supplied default2 with 16(!) available layouts. The current 
default layout is `Layout-Navtop-3Col.Css`.  So to create your own theme you just need 

 * to customise `typo.css` and `forms.css` (colours, fonts, sizes -  that sort of thing)
 * `layout.css` - is actually where your work happen (header, footer etc), 
 * and pick up one of available layouts. 
* Please note, that you don't need to change master template. It is same for all available layouts.  Moreover `master.html` is structured in the way to be friendly for SEO and screen readers: 

* content goes first, 
* then blocks, 
* and the last one is navigation. 

So we let them to read most useful stuff from our page first. To change layout for specific manager/action you must do the following in your code: 

`$output->masterLayout = 'layout-navtop-2col.css'; `

otherwise default layout will be used.  As usual some notes: 

 * current bunch of default CSS files is hard-coded in `SGL_Output::makeCssOptimizerLink()`, we are looking for the best way to be able to customise it, so you can load your own global list of files, 
 * also default layout `layout-navtop-3col.css` is hardcoded in `SGL_Output::makeCssOptimizerLink()`, so if you don't specify `$output->masterLayout` directly, you will get default one, we are looking for the best way to be able to customise it, 
 * old `default1` theme can be found in `SGL_WEB_ROOT/themes/default1`, 
* template files in modules still from `default1`, all new stuff  currently can be found directly in `SGL_WEB_ROOT/themes/default`,  as soon as we will resolve above probs I guess we will replace module files with new ones. 

[1]:	http://trac.seagullproject.org/ticket/1468
[2]:	http://www.contentwithstyle.co.uk/Articles/17/a-css-framework/
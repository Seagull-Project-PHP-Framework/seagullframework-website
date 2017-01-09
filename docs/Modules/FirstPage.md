<!-- Name: Modules/FirstPage -->
<!-- Version: 5 -->
<!-- Last-Modified: 2007/02/24 23:34:09 -->
<!-- Author: davert -->
[[TOC]]
## FirstPage Module

### Download
  * *.tgz 11Kb:* [http://trac.seagullproject.org/attachment/wiki/Modules/FirstPage/SGL_FirstPage.tgz]

### News

At last I have free time to fix all module problems. I was working on my site with FirstPage module in it, and discovered some errors. But still module looks great on large portals. So check this out. I have tested it on Seagull 6.2 platform, and made some updates in code. I hope that in nearest future this module will be a part of Seagull ;)

--

I detected there was several errors in repository and in tgz, so the module wouldn't work properly. I have updated this page, and now you can download FP from this page. 

---

At last module is complete. It can work with multiple pages, place every blocks and render it by two methods, using old-style <table> tags, and <div>s. Still this module is very easy to install and work. Indispensable part for internet portals

### Overview

FirstPage module created to provide flexible interface to design pages of site. If you need to create a page with several blocks on it, you do not need to edit HTML and write all tags routine. First, because its embarrassing. Second, because you need to edit the template every time you want to add new block or reorder old ones.
If you need not only static pages for your site, but main page for portal, with all last portal news and updates, it's better to use FirstPage on your site. 

### Screenshots
This is old one shows page of portal with russian blocks on standart Seagull design. Pay attention to placement of blocks. Block at the bottom uses 2 columns and is 200% wider ten other. Most left block fits two blocks in right column. 
[[Image(demo.jpg,800)]]

This is how this blocks looks in Admin panel. You can see spans value in blocks that use 2 rows or 2 columns.

[[Image(demo_admin.jpg,800)]]

More screenshots soon avaible.


### How it Works
FirstPage provides admin GUI, where you can organize view of blocks on your start page. You create blocks in *BlockMgr* with 'FirstPage' position, and then go to firstpage admin page. It's located on [http://yoursite.com/firstpage/firstpage/]. There all 'FirstPage' blocks are pushed in grid, where one cell of this grid contains one block. You can change size of grid, reorder blocks, and set horizontal and vertical spans for grid cells (Simple like colspan and rowspan in <table>). Then click 'Install' button and your start page will be replaced with FirstPage generated

### Features
  * simple admin gui. With WYSIWYG interface.
  * very flexible: you can create any blocks design on your page, without template editing.
  * refactoring: you can add/remove/reorder blocks any time you want
  * multi-pages: you can create unlimited amount of pages and push blocks on them
  * 2 renderers: DIV and TABLE. You can chose how your blocks will be rendered on your pages

### Tasks To Do
  * Fixing :)

### Changelog
  * 02/24/2007 - fixed JS problems. Updated to Seagull 6.2 version
  * 09/23/2006 - Added multi-pages, and new renderer. Created this page
  * 08/06/2006 - Code and templates updated for Seagull 6.0


### How to use that
  * Download (see section above)
  * Copy contents to your Seagull 0.6 dir.
  * Detect module
  * Create page where your blocks will be placed, using *FirstPage > Manage Pages* 
  * Push blocks on this page using Blocks module. Chose position FirstPage and section that corresponds to your page
  * Open *FirstPage > FirstPage Editor* and organize placement of this blocks. You can change span ( works similar as colspan and rowspan in tables ), number of rows and columns and order of blocks.
  * When you have finished, you can replace start page. Press 'Install' button FirstPage menu and now it is used as a default loading module.

### Comments?
Here:
[http://trac.seagullproject.org/wiki/RFC/FirstPage]
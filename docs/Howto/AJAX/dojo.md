<!-- Name: Howto/AJAX/dojo -->
<!-- Version: 12 -->
<!-- Last-Modified: 2006/11/30 15:48:13 -->
<!-- Author: demian -->
# dojo
[[TOC]]
*WORK IN PROGRESS*
## Overview
The purpose of this Howto is to guide you through the process of using dojo to AJAXify a module. We will use the Faq module as an example.

## Integration
 * [wiki:Integration/AJAX/dojo]

## Modifying Templates
### Master Templates
dojo has many [widgets](http://manual.dojotoolkit.org/index.html#widgets) which you can use for various tasks. We will be using the [ContentPane](http://manual.dojotoolkit.org/WikiHome/DojoDotBook/Book29) widget to dynamically replace the content of a <div>. In all of the master template files you use you will need and add the attributes (dojoType & executeScripts) below. Since we will only be modifying the Admin Manager for the Faq Module you will need to modify [source:trunk/modules/default/templates/admin_master.html]. You will also need to enclose outputBody() within a <div> with the ID adminContent. 


    <div id="adminContent" dojoType="ContentPane" executeScripts="true"> 
        {outputBody()} 
    </div> 

### Admin Templates
We need to require the dojo widget [ContentPane](http://manual.dojotoolkit.org/WikiHome/DojoDotBook/Book29) in [source:trunk/modules/default/templates/admin_header.html] before the </head> tag.


    <script type="text/javascript">
           dojo.require("dojo.widget.ContentPane");
    </script>

### Faq Templates
Next we need to modify the links to use javascript to request and display the desired action. Modify line 3 of [source:trunk/modules/faq/classes/admin_faqList.html] to resemble the line below


    <a class="action add" onclick="javascript:dojo.widget.byId('content').setUrl('{makeUrl(#add#,#adminfaq#,#faq#)}')">{translate(#new faq#)}</a>

Now modify the edit link on line 51 to look like the line below


    <a onclick="javascript:dojo.widget.byId('content').setUrl('{makeUrl(#edit#,#adminfaq#,##,aPagedData[data],#frmFaqId|faq_id#,key)}')">{aValue[question]}</a>

## Modifying Admin Faq Manager
Next, we need to modify our add() and edit() methods to use the masterFeed.html master template. Add the following line to both methods.


    $output->masterTemplate = 'masterFeed.html';

## Submitting Froms via XmlHttpRequest

[[AddComment]]
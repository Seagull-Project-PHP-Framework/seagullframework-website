<!-- Name: RFC/Translation2 -->
<!-- Version: 10 -->
<!-- Last-Modified: 2006/03/20 10:49:23 -->
<!-- Author: aj -->
# RFC for Translation2 integration


[[SubWiki(RFC/Translation2)]]

See: source:branches/translation2
[[TOC]]
This class provides an easy way to retrieve all the strings for a multilingual site from a data source (i.e. db).
[http://pear.php.net/package/Translation2]

## Roadmap
  * Simplify and shorten Translation keys. #780
  * Remove random data from translation keys. #781
  * Add missing translations to their own table and merge them into the fallback language when translated. #782
  * Module Help System that is translatable documentation. #40
  * Move all error messages for each module to an error array or error constants. #783
  * Build a Glossary. #784
  * Create guildlines for the creation of Translation keys. #785
  * This article is available in: English, German etc. #776
  * Allow for the selection of a reference language. #777
  * Suggestion: RFC/Translation2/TemplateArticles #778
  * language specific parameter or paths for exernal URIs. #779
  * Create a phparray container for Translation2. #774
  * Write tutorial about how to modify existing modules to use translation2 - #276
  * Add flags in articleMgr::list to display which languages are available for each article. #775
  * ~~Do not show all language versions of an article on the same page on article edit. You can make a selector (Ex: Edit the: English, Italian, German version of this article) ~~
  * ~~Separate Translation functionality out of MaintenanceMgr into TranslationMgr.~~
  * ~~Modify MaintenanceMgr to work with Translation2.~~
  * ~~Add missing keys~~
    * ~~Category Manager: Delete~~
    * ~~Article Manager: Article type~~
    * ~~Article Manager: select languages of new article~~
    * ~~Maintenance: check all modules~~
    * ~~My Account: change password~~
    * ~~Article: contributed by~~
    * ~~Article: edit~~
  * ~~Use config driven table names~~
  * ~~create function in SGL_Translation called getLangID to replace $languageID = str_replace('-', '_', SGL::getCurrentLang() .'-'. $GLOBALS['_SGL']['CHARSET']);~~     
  * ~~add translation config options to config templates~~

## Bugs
### Navigation
  * ~~BUG: You cannot edit the navigation title of a category section that is set by default on install. You can add multilingual navigation only to new categories section~~
  * ~~BUG: rev 474: you cannot add a new item to navigation. it doesn't show any languages in the select fields.~~

### Publisher
  * ~~BUG: You get an empty record in the Articles List section if you don't have a language version for an article. Or do not show that record Or show the article in the default or first available language. You can consider a language is available if you don't have an empty title for that language.~~
  * ~~BUG: the 'Article Name' field is empty in  the article listing~~
  * ~~BUG: the form name has been changed from 'newArticle' so preselecting the 'articles button doesn't work~~
  * ~~BUG:  FCK hack to allow more than one text area to be displayed 
    * see [!/FCKEditor] for a possible solution for that [/WernerKrauss WernerKrauss] /16.08.2005 16:45/ ~~
  * ~~BUG:  when "All" selected as Article Type you can add an new article. JS disable button not working properly~~
  * ~~BUG: the text for the article doesn't show up in the textarea, it shows up below it.~~
      Debug Notes: What is happening is that when there isn't an fieldID (an article w/o a translation) to be place in the hidden input the fieldValue is being used causing the text to appear below the textarea. SGL_Item::getDynamicContent needs to be modified to fetch the next ID from the DB and place the old ID in an array which can be used to update the item table with the new translation id.
      Note: this bug is still valid for "default article" in installation from scratch. Maybe it's because of an unmasked ""'"" ? [/WernerKrauss WernerKrauss] /12.10.2005 21:10/

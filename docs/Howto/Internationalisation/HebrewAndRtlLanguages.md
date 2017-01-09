<!-- Name: Howto/Internationalisation/HebrewAndRtlLanguages -->
<!-- Version: 8 -->
<!-- Last-Modified: 2009/03/04 10:53:31 -->
<!-- Author: demian -->
# Hebrew and RTL Languages
[[TOC]]

 * [Start](/wiki:Howto/Internationalisation/)
 * [General overview](/wiki:Howto/Internationalisation/General/)
 * [How to setup the environment](/wiki:Howto/Internationalisation/TechSetup/)
 * [Best practices](/wiki:Howto/Internationalisation/TranslationBestPractices/)
 * [How to convert your Seagull site to utf-8](/wiki:Howto/Internationalisation/ConvertingSeagullSitesToUtf8/)
 * [RTL support](/wiki:Howto/Internationalisation/HebrewAndRtlLanguages/)
 * [Submitting translations](/wiki:Howto/Internationalisation/SubmittingTranslations/)

Here's a summary of how Asaf got this working.

## New in Seagull 0.6.3
The language direction attribute is now automatically set to RTL if your current language is Arabic or Hebrew.  In the SGL_Task_BuildOutputData task the following code sets the attribute:


    $output->langDir = ($output->currLang == 'ar' || $output->currLang == 'he') ? 'rtl' : 'ltr';

And in the header.html template the result is echoed out:


    <html xmlns="http://www.w3.org/1999/xhtml" lang="{currLang}" xml:lang="{currLang}" dir="{langDir}">



## Setting up UTF-8 in the MySQL 4.1
in ary.languages.php we added:


    'he-utf-8'=> array('he([-_][:alpha:]{2})?|hebrew',  'hebrew-utf-8', 'he'),


We kinda move SGL's encoding to UTF8 and use UTF8 instead of ISO, that way
we have good support for all languages.

For best results, modify  _Config -> DB -> Post-connection query_ with a value like: SET NAMES 'utf8'

We also added just for fun to the master.html file the following when ($lang == "he") :


    <script>
      document.forms['language'].lang.value='he-utf-8';
      document.forms['language'].submit();
    </script>
    
    <form name=language action="{sScriptName}?{sQueryString}" method=post flexy:ignore>
    <input name=lang type=hidden value='en-utf-8'>
    </form>
    
    <center>
    <a href="javascript:changeLanguage('he');">
      <img src="{webRoot}/themes/{theme}/images/icons/tiny/il.png" alt="Hebrew"/>
    </a>&nbsp;
    <a href="javascript:changeLanguage('en');">
      <img src="{webRoot}/themes/{theme}/images/icons/tiny/us.png" alt="English"/>
    </a>
    </center>

## Implementing RTL Texf flow
This is very easy in modern browsers, simply implement a 'dir' attribute in the page's parent container div:


    <div id="sgl-header-left" dir="rtl" style="position:absolute;top:0px;left:1px;z-index:2;">
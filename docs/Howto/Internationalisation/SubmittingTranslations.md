<!-- Name: Howto/Internationalisation/SubmittingTranslations -->
<!-- Version: 5 -->
<!-- Last-Modified: 2009/03/04 10:53:47 -->
<!-- Author: demian -->
# How to submit a new translation
[[TOC]]

 * [Start](/wiki:Howto/Internationalisation/)
 * [General overview](/wiki:Howto/Internationalisation/General/)
 * [How to setup the environment](/wiki:Howto/Internationalisation/TechSetup/)
 * [Best practices](/wiki:Howto/Internationalisation/TranslationBestPractices/)
 * [How to convert your Seagull site to utf-8](/wiki:Howto/Internationalisation/ConvertingSeagullSitesToUtf8/)
 * [RTL support](/wiki:Howto/Internationalisation/HebrewAndRtlLanguages/)
 * [Submitting translations](/wiki:Howto/Internationalisation/SubmittingTranslations/)

Submitting translations is as simple as [wiki:Code/SubmittingPatches].

1. Create a patch that includes all newly created files.
  * cd to seagull [install-dir] directory
  * if you're adding new files, first do the following for the file to get picked up by the patch:


    $ svn add newfilename

  * then


    $ svn diff > translation.patch


2. Submit the patch as a new ticket to Seagull's [Trac](http://trac.seagullproject.org/) site. You have to login to do so. Please select the matching category for the translation you are submitting. If there is not a language listed use 'Translation - Other'. This way a new category can be added for that new language. 
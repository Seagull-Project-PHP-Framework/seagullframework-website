<!-- Name: Howto/Internationalisation/SubmittingTranslations -->
<!-- Version: 5 -->
<!-- Last-Modified: 2009/03/04 10:53:47 -->
<!-- Author: demian -->

# How to submit a new translation
* TOC
{:toc}

 - [Start][1]
 - [General overview][2]
 - [How to setup the environment][3]
 - [Best practices][4]
 - [How to convert your Seagull site to utf-8][5]
 - [RTL support][6]
 - [Submitting translations][7]

Submitting translations is as simple as [wiki:Code/SubmittingPatches].

1. Create a patch that includes all newly created files.
  2. cd to seagull [install-dir] directory
  3. if you're adding new files, first do the following for the file to get picked up by the patch:


	$ svn add newfilename

  * then


	$ svn diff \> translation.patch


2. Submit the patch as a new ticket to Seagull's [Trac][8] site. You have to login to do so. Please select the matching category for the translation you are submitting. If there is not a language listed use 'Translation - Other'. This way a new category can be added for that new language.

[1]:	/Howto/Internationalisation.html
[2]:	/Howto/Internationalisation/General.html
[3]:	/Howto/Internationalisation/TechSetup.html
[4]:	/Howto/Internationalisation/TranslationBestPractices.html
[5]:	/Howto/Internationalisation/ConvertingSeagullSitesToUtf8.html
[6]:	/Howto/Internationalisation/HebrewAndRtlLanguages.html
[7]:	/Howto/Internationalisation/SubmittingTranslations.html
[8]:	http://trac.seagullproject.org/
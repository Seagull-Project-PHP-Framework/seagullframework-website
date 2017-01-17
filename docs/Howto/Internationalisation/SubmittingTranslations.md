---
layout: page
title: How to submit a new translation
permalink: /Howto/Internationalisation/SubmittingTranslations.html
---

<!-- Name: Howto/Internationalisation/SubmittingTranslations -->
<!-- Version: 5 -->
<!-- Last-Modified: 2009/03/04 10:53:47 -->
<!-- Author: demian -->
<!-- Status: In Progress -->

# How to submit a new translation
* TOC
{:toc}

Submitting translations is as simple as [wiki:Code/SubmittingPatches].

1. Create a patch that includes all newly created files.
  2. cd to seagull [install-dir] directory
  3. if you're adding new files, first do the following for the file to get picked up by the patch:


	$ svn add newfilename

  * then

	$ svn diff \> translation.patch


2. Submit the patch as a new ticket to Seagull's [Trac][1] site. You have to login to do so. Please select the matching category for the translation you are submitting. If there is not a language listed use 'Translation - Other'. This way a new category can be added for that new language.

## Also In This Series

 - [Start][2]
 - [General overview][3]
 - [How to setup the environment][4]
 - [Best practices][5]
 - [How to convert your Seagull site to utf-8][6]
 - [RTL support][7]
 - [Submitting translations][8]

[1]:	http://trac.seagullproject.org/
[2]:	/Howto/Internationalisation.html
[3]:	/Howto/Internationalisation/General.html
[4]:	/Howto/Internationalisation/TechSetup.html
[5]:	/Howto/Internationalisation/TranslationBestPractices.html
[6]:	/Howto/Internationalisation/ConvertingSeagullSitesToUtf8.html
[7]:	/Howto/Internationalisation/HebrewAndRtlLanguages.html
[8]:	/Howto/Internationalisation/SubmittingTranslations.html
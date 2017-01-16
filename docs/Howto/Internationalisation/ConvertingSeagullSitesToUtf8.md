---
layout: page
title: Ho Converting Seagull Sites to Utf-8
permalink: /Howto/Internationalisation/ConvertingSeagullSitesToUtf8.html
---

<!-- Name: Howto/Internationalisation/ConvertingSeagullSitesToUtf8 -->
<!-- Version: 9 -->
<!-- Last-Modified: 2009/03/04 10:53:12 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Converting Seagull Sites to Utf-8
* TOC
{:toc}

 * [Start][1]
 * [General overview][2]
 * [How to setup the environment][3]
 * [Best practices][4]
 * [How to convert your Seagull site to utf-8][5]
 * [RTL support][6]
 * [Submitting translations][7]

## Overview
It's very easy to run your apps/sites with full utf-8 support since Seagull 0.6.3.  Seagull also supports easy switching between LTR and RTL text flow (left to right and right to left).

## Convert database to utf-8 and 'general' collation
There are some issues you should be aware of if your DB data was saved in a latin 1 encoding - this was default behaviour of MySQL before version 4.1.  See [this article][8] for some advice on how to convert latin 1 data to utf-8.

## Save language, data and template files as utf-8
Many editors use an ASCII or latin 1 encoding by default, others offer utf-8 but are very poor at displaying it correct, a case in point is ZDE, the Zend Development Environment.  Find an editor which reliably saves and presents utf-8 and use it exclusively for the language, data and templates files in your project.  I have found the following editors reliable:
 * TextMate
 * BBEdit

## Select the utf-8 version of your app/site language
This sets charset encoding correctly to utf-8 in terms of html meta tags and HTTP headers

## In config set fallback language to utf-8
This is Config setting available under the Translation tab, login as admin to modify.

## In admin prefs, set 'interface lang' to english-utf-8
Also logged in as admin, select My Account -\> Edit preferences and select a utf-8 language.

[1]:	/Howto/Internationalisation.html
[2]:	/Howto/Internationalisation/General.html
[3]:	/Howto/Internationalisation/TechSetup.html
[4]:	/Howto/Internationalisation/TranslationBestPractices.html
[5]:	/Howto/Internationalisation/ConvertingSeagullSitesToUtf8.html
[6]:	/Howto/Internationalisation/HebrewAndRtlLanguages.html
[7]:	/Howto/Internationalisation/SubmittingTranslations.html
[8]:	http://codex.wordpress.org/Converting_Database_Character_Sets
<!-- Name: Howto/Internationalisation/TranslationBestPractices -->
<!-- Version: 5 -->
<!-- Last-Modified: 2009/03/04 10:52:00 -->
<!-- Author: demian -->

# Translation Best Practices
* TOC
{:toc}

 - [Start][1]
 - [General overview][2]
 - [How to setup the environment][3]
 - [Best practices][4]
 - [How to convert your Seagull site to utf-8][5]
 - [RTL support][6]
 - [Submitting translations][7]

## Use underscore to separate variable names in translation strings

E.g. "%user\_name%" instead of "%userName%"

## Put punctuation marks inside the translation

As each languages behaves differently, e.g.:
 * What's your name?
 * Quel est ton nom ?
 * ¿Qual es tu nombre?

## Translating dynamic strings
"Dynamic string" means a translation phrase which contains some variables. Variables are identified by surrounding "%" percentage signs. So when you see something like this as the key: "Hi %member%!", put it inside the flexy translate method tr() and consult a developer about the corresponding name of the variable in the system.


	{tr(#Hi %member%!#,vprintf,#member|userName#)}

*Note:*
 * member is the name of the variable in the string to be translated
 * userName is the name of the variable whose value comes from the system and will be used in the translation

[1]:	/Howto/Internationalisation.html
[2]:	/Howto/Internationalisation/General.html
[3]:	/Howto/Internationalisation/TechSetup.html
[4]:	/Howto/Internationalisation/TranslationBestPractices.html
[5]:	/Howto/Internationalisation/ConvertingSeagullSitesToUtf8.html
[6]:	/Howto/Internationalisation/HebrewAndRtlLanguages.html
[7]:	/Howto/Internationalisation/SubmittingTranslations.html
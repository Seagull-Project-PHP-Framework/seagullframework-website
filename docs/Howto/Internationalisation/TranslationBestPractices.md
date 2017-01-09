<!-- Name: Howto/Internationalisation/TranslationBestPractices -->
<!-- Version: 5 -->
<!-- Last-Modified: 2009/03/04 10:52:00 -->
<!-- Author: demian -->
# Translation Best Practices
[[TOC]]

 * [Start](/wiki:Howto/Internationalisation/)
 * [General overview](/wiki:Howto/Internationalisation/General/)
 * [How to setup the environment](/wiki:Howto/Internationalisation/TechSetup/)
 * [Best practices](/wiki:Howto/Internationalisation/TranslationBestPractices/)
 * [How to convert your Seagull site to utf-8](/wiki:Howto/Internationalisation/ConvertingSeagullSitesToUtf8/)
 * [RTL support](/wiki:Howto/Internationalisation/HebrewAndRtlLanguages/)
 * [Submitting translations](/wiki:Howto/Internationalisation/SubmittingTranslations/)

## Use underscore to separate variable names in translation strings

E.g. "%user_name%" instead of "%userName%"

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

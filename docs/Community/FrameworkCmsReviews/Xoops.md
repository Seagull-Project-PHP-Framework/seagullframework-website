<!-- Name: Community/FrameworkCmsReviews/Xoops -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/12/31 00:24:59 -->
<!-- Author: demian -->
# Comparing XOOPS to Seagull
[[TOC]]
## review
Since I really like Xoops, it has limitations that made me looking for something like Seagull:
  * Xoops is definitely more a CMS than a framework, of course you can disable a majority of bundled modules, but using Xoops as a framework is really heavy
  * No native multi lingual support. You have to patch core files at EACH version upgrade to keep this function.
  * No native friendly URLs at the moment..(I think) but there is a complicated trick with mod_rewrite (and patch core files again)
  * Pear integration only for DB abstraction layer
  * For each upgrade, need to drop or modify strings into templates as they are for a community usage, not a professional one. ie: "rank" in user info, "comments" in news...
  * making a table free design is a pain as Xoops iterates left /right columns is cells
  * And last but not least: Documentation! (the nightmare of devs ;p) there is a strong support for users but the api doc for programmers is 2 versions old and so light (PHPDoc generated)...


## conclusion
Xoops is better as a cms
Seagull is better as a framework

Xoops has by the moment one advantage: production status.

EmmanuelBrendel
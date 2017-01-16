<!-- Name: Standards/PhpBestPractices -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/15 10:26:13 -->
<!-- Author: demian -->
# PHP Best Practices

  * variables
    * dealing with request variables securely
    * testing variable values
    * all whitespace and potentially nasty ascii zeros are removed from request variables
    * javascript is stripped by default and optionally all html too
    * magic quotes are dealt with according to enduser config
    * variable passing works with register_globals = on or off
  * date handling
    * using strftime() for all user-facing dates, to take into account their locales
    * using gmstrftime() for all system-facing dates
  * templates
    * presentation layer only responsible for dressing processed output in html, no logic aside from looping and conditional display
  * directory naming: Directory names commonly or easily associated with a given server-side technology unnecessarily disclose implementation details and discourage permanent URLs. More generic paths should be used. For example, instead of /cgi-bin, use a /scripts directory, instead of /css, use /styles, instead of /javascript, use /scripts, and so on. 
  * application security
    * some great ideas over at the [http://wact.sourceforge.net/index.php/PhpApplicationSecurity WACT site]
    * some more security best practices [http://www.securereality.com.au/studyinscarlet.txt here]
  * a good summary of [PHP gotchas](http://www.sitepoint.com/blog-post-view.php?id=176388) by Harry F.

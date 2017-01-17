<!-- Name: RFC/SiteSearch -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/13 19:38:09 -->
<!-- Author: werner -->

# RFC for internal site search

Some ideas for internal site search.

  * each module should return a result-object. we need a static function per module that knows how to search and where (which table, which cells) to search.
  * return and universal result set, with the keys: title, shortdescription, link
  * maybe sort by rights (if user guest isn't able to watch the article, he should not get the search result). this will prevent guests from seeing too much "you are not allowed... please login" screens.
  * caching
  * indexing
  * indexing of uploaded documents


show resultset like:
1 result in faq: -\> link to restult
13 results in "articles" -\> link to article-results...

from phpdig docs:

PhpDig indexes HTML and text files by itself. PhpDig could index PDF, MS-Word, MS-Excel, and MS-PowerPoint files if you install external binaries on the server for this purpose. PhpDig is configured to use catdoc, xls2csv, pstotext or pdftotext, and ppt2text programs.

  * You can find catdoc and xls2csv at this link: http://www.45.free.net/\~vitus/ice/catdoc/.

  * You can find pstotext at this link: http://research.compaq.com/SRC/virtualpaper/pstotext.html.

  * You can find pdftotext at this link: http://public.planetmirror.com/pub/xpdf/

  * You can query for ppt2text at this link: http://www.google.com/search?q=ppt2text

The author of PhpDig does not offer support for the binary programs. Contact the authors of those programs if you have trouble with compiling and/or installing them.

Of course, you can use other binary programs to extract text from PDF, MS-Word, MS-Excel, and MS-PowerPoint files.

To demonstrate the external binaries feature, you can search Hamlet (tragedy, Shakespeare, from MS-Word format) or \~L'Avare (comedy, Molière, from PDF format).
[/end phpdig docs]

don't know if those programs would be available for windows, too... maybe use pear classes for indexing those docs?

good links for search related things:

external site search:
  * http://www.phpdig.net/navigation.php?action=doc (maybe one can get some ideas there)
  * http://www.htdig.org

sbaturzio: an interface from SGL to htdig IMHO is a nice solution
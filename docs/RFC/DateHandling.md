<!-- Name: RFC/DateHandling -->
<!-- Version: 3 -->
<!-- Last-Modified: 2005/11/13 19:18:46 -->
<!-- Author: werner -->
# Date Handling
[[TOC]]


There are currently a few date handling methods in the Output class, they take care of the basics like creating dropdowns over year ranges, and !MySQL format to unix timestamp format conversion, etc.

Ideally I'd like to pull the code out of Output and have a dedicated Date class, for everything other that what PEAR::Date handles, some good ideas here: http://www.phpclasses.org/browse/file/7702.html

  * my idea exactly, this will be easy to do  [/DemianTurner Demian Turner] 

Also we should change SGL::getTime to SGL_Date::now

## Phillipe's suggestions
4) formatPretty()

I believe I saw a recent message about this in the archive.
The format given for French date format is quite curious:
'%d %B, %Y %H:%M' giving something like 05 juillet, 2005 11:56
This 05 is too "geek", we usually drop the leading zero, and no coma, so it should be:
"%e %B %Y %H:%M" -> 5 juillet 2005 11:56

Caveat: %e doesn't work on Windows, so either we stick to %d (ugly but simple) or we post-process $output...
I suppose it is easier to write my own formatter than to patch Seagull...

4) format()

Inconsistency between comment (dd.mm.yyyy) and code (variable depending on locale).
Perhaps should rename formatPretty and format (ugly?) to formatLong and formatShort or something like that.
I think the default should fall back on ISO format <http://en.wikipedia.org/wiki/Iso_dateQ> which is international.

Suggestion: perhaps use a PHP file per locale allowing to set such L10N settings, instead of centralizing all formats in a few functions (which would become huge as locales are added).

In Zen Cart e-shop, there is a file per locale/language, where are defined things like that:

# english.php
define('DATE_FORMAT_SHORT', '%m/%d/%Y');  '' this is used for strftime()
define('DATE_FORMAT_LONG', '%A %d %B, %Y'); '' this is used for strftime()
define('DATE_FORMAT', 'm/d/Y'); '' this is used for date()
define('DATE_TIME_FORMAT', DATE_FORMAT_SHORT . ' %H:%M:%S');

# french.php (my comments...)
'' Format court (ici 31/04/2005)
define('DATE_FORMAT_SHORT', '%d/%m/%Y');  '' this is used for strftime()

'' Format long (ici samedi 31 avril 2005)
define('DATE_FORMAT_LONG', '%A %e %B %Y'); '' this is used for strftime()
'' Format court (ici 31/04/2005). Voir http://www.nexen.net/docs/php/annotee/function.date.php
define('DATE_FORMAT', 'd/m/Y'); '' this is used for date()
'' Format de la date et de l'heure (ici 31/04/2005 21:05:15)
define('DATE_TIME_FORMAT', DATE_FORMAT_SHORT . ' %H:%M:%S');

but I think that providing a set of functions (allowing post-processing...) or perhaps a class per language/locale would be even better. 

## Mindaugas suggestions
SGL_Date::format() and SGL_Date::formatPretty() is not flexible. Version
0.4.4 has only French, Brazilian and UK/US date formats. To use any other
format, let's say Lithuanian, I need to modify these methods, which doesn't
seems to be good idea to modify original sources. My suggestion would be to
change 'dateFormat' configuration option from 2 letter code to strftime's
format string. In this case any date format could be set with no additional
code modification.

Another thing is, would be nice to have methods (or same format() method
with differrent parameters) in SGL_Date (and in SGL_Output) returning date
with time and only time.

Forgot one more thing. Global option (or even parameter to date/time
functions) to show dates in GMT, server or user timezone would be usefull
too.
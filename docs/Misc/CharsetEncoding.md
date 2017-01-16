<!-- Name: Misc/CharsetEncoding -->
<!-- Version: 8 -->
<!-- Last-Modified: 2006/12/31 00:28:57 -->
<!-- Author: demian -->
# Charset Encoding Issues
[[TOC]]
## Unicode
 * ideal output for web is utf8
 * UTF-8 is a decent character set for mapping characters to integers
 * data can be encoded into a variety of character sets
 * Unicode represents all characters (in all languages) with integers.
 * ASCII represents every character as a binary digit between 32 and 127
 * in the ASCII character set, each binary value between 0 and 127 is given a specific character
 * character data is stored one character per byte, but the ASCII system only required 7 bits
 * of the 256 possible values in an 8 bit byte, there were 128 slots left free that people decided to play with, randomly assigning different values
 * unicode is a single character set that represents every writing system known
 * in unicode, each letter is represented by a code point, ie, a U for unicode and an hexadecimal number: U+FEC9
 * 'Hello' corresponds to the following code points: U+0048 U+0065 U+006C U+006C U+006F
 * encodings are used to store the code points in memory and in web documents
 * at the beginning of some unicode strings is an indication of the Byte Order Mark (BOM), which specifies if the string is stored in high-endian or low-endian mode, 
 * UTF-8 is an encoding that stores unicode code points in 8 bit bytes
 * English text looks exactly the same in UTF-8 as it does in ASCII, only code points 128 and above are stored using 2, 3, in fact, up to 6 bytes
 * 2 ways of encoding unicode:
   * The traditional store-it-in-two-byte methods are called UCS-2
   * And there's the popular new UTF-8 standard
 * It does not make sense to have a string without knowing what encoding it uses

## Character Encoding Negotiation
 * how the browser decides which encoding to use

## Forms Data Set Encoding
 * can be encoded in application/x-www-form-urlencoded
 * in this mode, changing charsets changes encoding
 * modern browsers send x-www-form-urlencoded data to the server in the CHARSET that was determined to be that of the *form*, however that determination was made
 * drawbacks
   * document charset must be correctly identified (often is not)
   * fails with multiple encodings handled by a single CGI
   * fails with transcoding proxies
 * it is recommended to use UTF-8 in both directions
 * and use multipart/form-data encoding

## Encoding According to Target Type
 * you must encode your characters according to whether the target is html, xml or a URI, eg:



    character	html		xml		url
    ~~~~~~~~-	~~~~		~~~		~~~
    €		= &euro;	&#x20AC;	%E2%82%AC


## Various Transformations
 * between one charset and another: I18N::UnicodeString
 * determining string position: I18N::UnicodeString
 * unicode_to_entities_preserving_ascii()

## Converting from IS0-8859-1 to UTF-8
 * All characters in the range of 0-127 (hex 00 through 7F), are represented identically in both encodings.  This covers the entire range of the original ASCII characters
 * All iso-8859-1 characters in the range of 128-191 (hex 80 through BF) need to be preceeded by a byte with the value of 194 (hex C2) in utf-8, but otherwise are left intact
 * All iso-8859-1 characters in the range of 192-255 (hex C0 through FF) not only need to be preceeded by a byte with the value of 195 (hex C3) in utf-8, but also need to have 64 (hex 40) subtracted from the iso-8859-1 character value.  For example, a &#241;  (decimal 241, hex F1) becomes a 195 followed by a 177 (hex C3 B1)
 * here's some code that will do it: http://miscoranda.com/96

## Good Encoding Practices (T. Braye)
 * Embrace Unicode, don't fight it; it's probably the right thing to do, and if it weren't you'd probably have to anyhow
 * Inside your software, store text as UTF-8 or UTF-16; that is to say, pick one of the two and stick with it
 * Interchange data with the outside world using XML whenever possible; this makes a whole bunch of potential problems go away
 * Try to make your application browser-based rather than write your own client; the browsers are getting really quite good at dealing with the texts of the world
 * If you're using someone else's library code (and of course you are), assume its Unicode handling is broken until proved to be correct

## BOM

If you use UTF-8 as charset, check every file you save with UTF-8 compliant editor and *remove* UTF-8 BOM from beginning of file if it's present, otherwise this will break display in IE, however Mozilla browsers seem to be immune to the problem.

UTF-8 BOM characters written in HEX are EF BB BF

BOM is the abbreviation for Byte Order Mark.

## Resources
 * http://blog.joshuaeichorn.com/archives/2005/09/30/html_ajax-021-released/#comment-5133
 * http://www.gravitonic.com/do_download.php?download_file=talks/oscon2005/php_unicode_oscon2005.pdf
 * http://www.acko.net/blog/unicode-in-php
 * http://www.w3.org/International/
 * http://www.xencraft.com/training/webstandards.html
 * http://www.joelonsoftware.com/articles/Unicode.html
 * http://intertwingly.net/stories/2004/04/14/i18n.html
 * http://www.onlamp.com/pub/a/php/2002/11/28/php_i18n.html
 * http://www.randomchaos.com/document.php?source=php_and_unicode
 * http://www.cl.cam.ac.uk/~mgk25/unicode.html#utf-8
 * http://ppewww.ph.gla.ac.uk/~flavell/charset/form-i18n.html
 * http://www.webreference.com/dlab/books/html/39-0.html
 * http://pear.php.net/package/I18N_UnicodeString
 * http://keithdevens.com/weblog/archive/2004/Jun/29/UTF-8.regex
 * http://blog.ajohnstone.com/index.php?p=7
 * http://dev.mysql.com/tech-resources/articles/4.1/unicode.html (good ideas for testing charset of data posted from a from)
 * http://www.w3.org/International/questions/qa-forms-utf-8.html
 * http://www.nathan-syntronics.de/midgard/midcom/utf8.html
 * http://derickrethans.nl/files/wereldveroverend-ffm2004.pdf
 * http://www.oracle.com/technology/tech/opensource/php/globalizing_oracle_php_applications.pdf
 * http://minutillo.com/steve/weblog/2004/6/17/php-xml-and-character-encodings-a-tale-of-sadness-rage-and-data-loss
 * http://blog.bitflux.ch/archive/how-to-get-rid-of-invalid-utf-8-characters.html
 * http://wact.sourceforge.net/docs/doku.php?id=php:i18n
 * http://www.sitepoint.com/blog-post-view.php?id=254984
 * http://wact.sourceforge.net/docs/doku.php?id=php:i18n:charsets
 * http://farm.tucows.com/blog/_archives/2003/10/16/4630.html
 * http://laughingmeme.org/archives/2004_03_21.html

## Tests
1. form submission
 * encoding utf-8
 * lang french
 * Content-Type: application/x-www-form-urlencoded
 * data: 

action=send&token=02186cc2b41b4c4d64fcde3d611884a4&contact%5Bfirst_name%5D=&contact%5Blast_name%5D=User&contact%5Bemail%5D=demian%40phpkitchen.com&contact%5Btype%5D=General+enquiry&contact%5Bcomment%5D=Veuillez+remplir+les+champs+indiqu%C3%A9s+et+recommencer&submitted=Envoyer

----

 * encoding iso-8859-1
 * lang french
 * Content-Type: application/x-www-form-urlencoded
 * data: 

action=send&token=02186cc2b41b4c4d64fcde3d611884a4&contact%5Bfirst_name%5D=&contact%5Blast_name%5D=User&contact%5Bemail%5D=demian%40phpkitchen.com&contact%5Btype%5D=General+enquiry&contact%5Bcomment%5D=Veuillez+remplir+les+champs+indiqu%C3%A9s+et+recommencer&submitted=Envoyer

----

 * encoding utf-8
 * lang french
 * Content-Type: multipart/form-data; 
 * data: Veuillez remplir les champs indiqu&#195;©s et recommencer

----

 * for db storage: utf-8
 * for data manip by PHP: unicode
 * for display in x/html: utf-8

## input / output encoding with GET/POST

process:
  * client browser
  * http post
  * server response
  * client browser

*input GET (iso-8859-1)(application/x-www-form-urlencode)*:


    param: input= <html> is a tag
    param: contentType = text/plain


*output*:


    input=%3Chtml%3E+is+a+tag&contentType=text%2Fplain&submit=


*input POST (iso-8859-1)(application/x-www-form-urlencode)*:


    param: input= <html> is a tag
    param: contentType = text/plain


*output*:


    input=%3Chtml%3E+is+a+tag&contentType=text%2Fplain&submit=


*input POST (iso-8859-1)(multipart/form-data)*:


    param: input= <html> is a tag
    param: contentType = text/plain


*output*:


    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~-12141224864143487581745862222
    Content-Disposition: form-data; name="input"


<html> is a tag


    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~-12141224864143487581745862222
    Content-Disposition: form-data; name="contentType"


text/plain

## Character encoding with PHP

    $str = '<html> is a tag';
    print urlencode($str);
    //  %3Chtml%3E+is+a+tag
    
    $str = '<html> is a tag';
    print rawurlencode($str);
    //  %3Chtml%3E%20is%20a%20tag
    
    $str = '<html> is a tag';
    print utf8_encode($str);
    //  <html> is a tag
    
    $str = '<html> is a tag';
    print base64_encode($str);
    //  PGh0bWw+IGlzIGEgdGFn

## Notes
  * UTF-8 is a multi-byte  encoded character set
  * good description of detecting Byte Order Mark in .NET : http://www.devhood.com/tutorials/tutorial_details.aspx?tutorial_id=469
  * Linux/Unix does not use any BOMs and signatures
  * On POSIX systems, the selected locale identifies already the encoding expected in all input and output files of a process
  * you can detect charset encoding in PHP with the multibyte functions: http://uk.php.net/manual/en/ref.mbstring.php
  * good Unicode FAQ with lots of info on the difference between utf-8 and ascii: http://pipin.tmd.ns.ac.yu/unicode/unicode-faq.html
  * check out the How do I have to modify my software? section from the above link
  * UCS characters U+0000 to U+007F (ASCII) are encoded simply as bytes 0x00 to 0x7F (ASCII compatibility). This means that files and strings which contain only 7-bit ASCII characters have the same encoding under both ASCII and UTF-8.
  * All UCS characters >U+007F are encoded as a sequence of several bytes, each of which has the most significant bit set. 
  * All possible 231 UCS (Universal Character Set) codes can be encoded
  * UTF-8 encoded characters may theoretically be up to six bytes long
  * if you want to display two charsets on a single page, impossible without utf-8 encoding, ie, comments in french and japanese
  * how data in web forms gets encoded in POST/GET : http://ppewww.ph.gla.ac.uk/~flavell/charset/form-i18n.html
<!-- Name: TipsAndTricks/CheckingHtmlLinks -->
<!-- Version: 1 -->
<!-- Last-Modified: 2008/10/31 15:50:55 -->
<!-- Author: demian -->
# Tips for checking HTML links
A piece of cake on OS X and linux:


    $ port install linkchecker
    $ linkchecker -v -o html http://localhost/my_seagull_site/0.6-bugfix/www/ > myfile.html
    $ open myfile.html



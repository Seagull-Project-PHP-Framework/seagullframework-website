<!-- Name: TroubleShooting/RemovingIndex.php -->
<!-- Version: 6 -->
<!-- Last-Modified: 2008/10/27 09:46:26 -->
<!-- Author: rungss -->
# Removing index.php from the url
## Problem: removing index.php (the front controller script) from Seagull urls causes modules with web resources to fail to load

 * note: a module with web resources is any module that has a www dir, upon install this is symlinked to the webroot, so your web files like CSS, js and images can be loaded by browser requests

## UPDATE
This problem has been fixed with the etc/htaccess-cleanUrl.dist file supplied in Seagull 0.6.6.  Please ignore the rest of this page if you're using Seagull 0.6.6 or later.

## Solution
Modify your .htaccess file (copied from etc/htaccess-cleanUrl.dist) and replace the line


    RewriteCond %{REQUEST_FILENAME} !-d

with 


    RewriteCond %{REQUEST_FILENAME} !-l

This allows mod_rewrite to correctly interpret the symlinks Seagull has created in the www dir.  Making this modification however will prevent you from being able to list the contents of a directory in Seagull.  Requesting distinct files from within a folder will continue to work fine.

works

    http://example.com/foo/bar.txt
    http://example.com/foo/baz.js
    http://example.com/foo/quux.css

doesn't work

    http://example.com/foo/

Between loading Seagull modules (with and without web resources) and using routes, you should never have to list the contents of a directory.  If you do however please be aware that this will not work.

*Windows Users*

This hack will not work as the web resources are copied and not symlinked under Windows. To have the same behavior do the following:

 * add a placeholder file called e.g. norewrite.txt in each module's web resources folder
 * modify you .htaccess file:

Change

    RewriteCond %{REQUEST_FILENAME} !-d

to


    RewriteCond %{REQUEST_FILENAME} !-d [OR]
    RewriteCond %{REQUEST_FILENAME}norewrite\.txt -f


*Note 1:* In the 0.9 releases of Seagull we will be prefixing the web resources dirs with an underscore, so htaccess mods won't be necessary.

0.6.x

    web resource: modules/foo/www
    symlink created: www/foo

0.9.x

    web resource: modules/foo/www
    symlink created: www/_foo

see #1610

*Note 2:* In Seagull 0.6.5 the etc/htaccess-cleanUrl.dist was changed to support symlinks by default, ie -d was replaced with -l

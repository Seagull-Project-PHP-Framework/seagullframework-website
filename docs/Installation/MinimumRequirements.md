<!-- Name: Installation/MinimumRequirements -->
<!-- Version: 6 -->
<!-- Last-Modified: 2006/12/30 22:37:21 -->
<!-- Author: demian -->
# Minimum Requirements

To run Seagull you need at least 
  * a Webserver (e.g [Apache (any version)](http://www.apache.org/) or IIS)
  * [PHP 4.3](http://php.net) or greater, including php 5.1 or greater (running either as a cgi or mod_php)
  * a [MySQL](http://mysql.com), [Postgres](/wiki:Howto/DB/WorkingWithPostgres/), or Oracle database.  As the PEAR::DB abstraction layer is used, any database supported by the library should be usable by Seagull, however you may need to customise the supplied SQL schema and data files.
  * in php.ini, set memory_limit to at least 16M (only required when PHP is compiled with --enable-memory-limit)

Currently Seagull works fine in a shared-hosting environment, but safe_mode must be disabled.

All of the above is flagged in the environment detection screen of the web-based installer.
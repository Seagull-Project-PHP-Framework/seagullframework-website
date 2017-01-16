<!-- Name: RFC/PearStatus -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/04/24 12:24:17 -->
<!-- Author: demian -->
# PEAR Status
This page keeps track of mods that have been made to standard PEAR libs
 * Generator.php from DB_DataObject has latest CVS code, not available in DB_DataObject 1.8.5
 * latest version of DB is being held back as it breaks BC
 * Config is being held back as it breaks our quoting hacks for sub-keys
 * pgsql.php was modified to deal with pg quoting probs, see [3069]
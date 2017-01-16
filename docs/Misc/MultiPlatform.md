<!-- Name: Misc/MultiPlatform -->
<!-- Version: 1 -->
<!-- Last-Modified: 2005/11/06 15:50:06 -->
<!-- Author: trac -->
FIXME: Look over new file... 

Borrowing from PHP's own multi-platform flexibility, Seagull  operates almost identically on Windows and Unix platforms.  Thanks to the extensive use of PEAR libraries it also offers connectivity to just about any database on the market*, including:

  * Mysql
  * Postresql
  * MS SQL Server* 
  * Oracle
* Only Mysql, Postgres and Oracle are supported at the time of writing, although earlier versions included schema for MS SQL.  Other than that PEAR::DB is used throughout.
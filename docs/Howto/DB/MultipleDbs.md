<!-- Name: Howto/DB/MultipleDbs -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/04/15 22:25:46 -->
<!-- Author: demian -->

# How To Work with Multiple Database Resources

By default, the standard way to get the db handle to the Seagull's database is to use the following code:

	$dbh = & SGL_DB::singleton();

This presumes you have a db correctly configured in the config file. If you, however want to connect to an additional db, just use the same convention with a $dsn argument, ie:


	$dsn = 'pgsql://username:password@localhost/myDB';
	$dbhExt = & SGL_DB::singleton($dsn);

From here you can use $dbhExt as an usual PEAR DB, or its extended version SGL\_DB, connection to your database.

Additional notes on PEAR datasource name conventions are [here][1].

[1]:	http://pear.php.net/manual/en/package.database.db.intro-dsn.php
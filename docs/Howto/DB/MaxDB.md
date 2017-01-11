<!-- Name: Howto/DB/MaxDB -->
<!-- Version: 4 -->
<!-- Last-Modified: 2007/07/02 17:47:13 -->
<!-- Author: demian -->

# Getting SGL to work with MaxDB

I wrote a DB-Wrapper for the PEAR-Odbc-Database-Driver called maxdb\_SGL, it will be available soon.......

## Reserved Words for MaxDB
|| *Reserved in !MaxDB* || *Context of usage in !MaxDB* || *!MySQL counterpart* ||
|| `@` || May prefix identifier, like '@table' || Not allowed ||
|| `ADDDATE()` || SQL function || `ADDDATE()`; new in !MySQL version 4.1.1 ||
|| `ADDTIME()` || SQL function || `ADDTIME()`; new in !MySQL version 4.1.1 ||
|| `ALPHA` || SQL function || Nothing comparable ||
|| `ARRAY` || Data type || Not implemented ||
|| `ASCII()` || SQL function || `ASCII()`, but implemented with a different meaning ||
|| `AUTOCOMMIT` || Transactions; `ON` by default || Transactions; `OFF` by default ||
|| `BOOLEAN` || Column types; `BOOLEAN` accepts as values only `TRUE`, `FALSE`,[[BR]]and `NULL` || `BOOLEAN` was added in !MySQL version 4.1.0; it is a synonym for `BOOL`[[BR]]which is mapped to `TINYINT(1)`. It accepts integer values in the same[[BR]]range as `TINYINT` as well as `NULL`. `TRUE` and `FALSE`[[BR]]can be used as aliases for `1` and `0`. ||
|| `CHECK` || `CHECK TABLE` || `CHECK TABLE`; similar, but not identical usage ||
|| `COLUMN` || Column types || `COLUMN`; noise word ||
|| `CHAR()` || SQL function || `CHAR()`; identical syntax; similar, not identical usage ||
|| `COMMIT` || Implicit commits of transactions happen when data definition queries are being[[BR]]issued || Implicit commits of transactions happen when data definition queries are being[[BR]]issued, but also with a number of other queries ||
|| `COSH()` || SQL function || Nothing comparable ||
|| `COT()` || SQL function || `COT()`; identical syntax and implementation ||
|| `CREATE` || SQL, data definition language || `CREATE` ||
|| `DATABASE` || SQL function || `DATABASE()`; `DATABASE` is used in a different context, for[[BR]]example `CREATE DATABASE` ||
|| `DATE()` || SQL function || `CURRENT_DATE` ||
|| `DATEDIFF()` || SQL function || `DATEDIFF()`; new in !MySQL version 4.1.1 ||
|| `DAY()` || SQL function || Nothing comparable ||
|| `DAYOFWEEK()` || SQL function || `DAYOFWEEK()`; the first day (`1`) by default is Monday in[[BR]]!MaxDB, and Sunday in !MySQL ||
|| `DISTINCT` || SQL functions `AVG`, `MAX`, `MIN`, `SUM` || `DISTINCT`; but used in a different context: `SELECT DISTINCT` ||
|| `DROP` || inter alia in `DROP INDEX` || `DROP INDEX`; similar, but not identical usage ||
|| `EBCDIC()` || SQL function || Nothing comparable ||
|| `EXPAND()` || SQL function || Nothing comparable ||
|| `EXPLAIN` || Optimization || `EXPLAIN`; similar, but not identical usage ||
|| `FIXED()` || SQL function || Nothing comparable ||
|| `FLOAT()` || SQL function || Nothing comparable ||
|| `HEX()` || SQL function || `HEX()`; similar, but not identical usage ||
|| `INDEX()` || SQL function || `INSTR()` or `LOCATE()`; similar, but not identical syntaxes[[BR]]and meanings ||
|| `INDEX` || `USE INDEX`, `IGNORE INDEX` and similar hints are being used[[BR]]right after `SELECT`, like `SELECT ... USE INDEX` || `USE INDEX`, `IGNORE INDEX` and similar hints are being used[[BR]]in the `FROM` clause of a `SELECT` query, like in `SELECT[[BR]]... FROM ... USE INDEX` ||
|| `INITCAP()` || SQL function || Nothing comparable ||
|| `LENGTH()` || SQL function || `LENGTH()`; identical syntax, but slightly different implementation ||
|| `LFILL()` || SQL function || Nothing comparable ||
|| `LIKE` || Comparisons || `LIKE`; but the extended `LIKE` !MaxDB provides rather[[BR]]resembles the !MySQL `REGEX` ||
|| `LIKE` wildcards || !MaxDB supports '%', '_', 'ctrl+underline', 'ctrl+up arrow', '\*', and '?'[[BR]]as wildcards in a `LIKE` comparison || !MySQL supports '%', and '_' as wildcards in a `LIKE` comparison ||
|| `LPAD()` || SQL function || `LPAD()`; slightly different implementation ||
|| `LTRIM()` || SQL function || `LTRIM()`; slightly different implementation ||
|| `MAKEDATE()` || SQL function || `MAKEDATE()`; new in !MySQL version 4.1.1 ||
|| `MAKETIME()` || SQL function || `MAKETIME()`; new in !MySQL version 4.1.1 ||
|| `MAPCHAR()` || SQL function || Nothing comparable ||
|| `MICROSECOND()` || SQL function || `MICROSECOND()`; new in !MySQL version 4.1.1 ||
|| `NOROUND()` || SQL function || Nothing comparable ||
|| `NULL` || Column types; comparisons || `NULL`; !MaxDB supports special `NULL` values that are returned[[BR]]by arithmetic operations that lead to an overflow or a division by zero; !MySQL does[[BR]]not support such special values ||
|| `PI` || SQL function || `PI()`; identical syntax and implementation, but parantheses are[[BR]]mandatory ||
|| `REF` || Data type || Nothing comparable ||
|| `RFILL()` || SQL function || Nothing comparable ||
|| `ROWNO` || Predicate in `WHERE` clause || Similar to `LIMIT` clause ||
|| `RPAD()` || SQL function || `RPAD()`; slightly different implementation ||
|| `RTRIM()` || SQL function || `RTRIM()`; slightly different implementation ||
|| `SEQUENCE` || `CREATE SEQUENCE`, `DROP SEQUENCE` || `AUTO_INCREMENT`; similar concept, but differing implementation ||
|| `SINH()` || SQL function || Nothing comparable ||
|| `SOUNDS()` || SQL function || `SOUNDEX()`; slightly different syntax ||
|| `STATISTICS` || `UPDATE STATISTICS` || `ANALYZE`; similar concept, but differing implementation ||
|| `SUBSTR()` || SQL function || `SUBSTRING()`; slightly different implementation ||
|| `SUBTIME()` || SQL function || `SUBTIME()`; new in !MySQL version 4.1.1 ||
|| `SYNONYM` || Data definition language: `CREATE [PUBLIC] SYNONYM`, `RENAME SYNONYM`,[[BR]]`DROP SYNONYM` || Nothing comparable ||
|| `TANH()` || SQL function || Nothing comparable ||
|| `TIME()` || SQL function || `CURRENT_TIME` ||
|| `TIMEDIFF()` || SQL function || `TIMEDIFF()`; new in !MySQL version 4.1.1 ||
|| `TIMESTAMP()` || SQL function || `TIMESTAMP()`; new in !MySQL version 4.1.1 ||
|| `TIMESTAMP()` as argument to `DAYOFMONTH()` and `DAYOFYEAR()` || SQL function || Nothing comparable ||
|| `TIMEZONE()` || SQL function || Nothing comparable ||
|| `TRANSACTION()` || Returns the ID of the current transaction || Nothing comparable ||
|| `TRANSLATE()` || SQL function || `REPLACE()`; identical syntax and implementation ||
|| `TRIM()` || SQL function || `TRIM()`; slightly different implementation ||
|| `TRUNC()` || SQL function || `TRUNCATE()`; slightly different syntax and implementation ||
|| `USE` || `!MySQL` commandline user interface command || `USE` ||
|| `USER` || SQL function || `USER()`; identical syntax, but slightly different implementation, and[[BR]]parantheses are mandatory ||
|| `UTC_DIFF()` || SQL function || `UTC_DATE()`; provides a means to calculate the result of `UTC_DIFF()` ||
|| `VALUE()` || SQL function, alias for `COALESCE()` || `COALESCE()`; identical syntax and implementation ||
|| `VARIANCE()` || SQL function || Nothing comparable ||
|| `WEEKOFYEAR()` || SQL function || `WEEKOFYEAR()`; new in !MySQL version 4.1.1 ||


## Modify SGL DB lib file (OLD)
You have to modify the `SGL_DB::singleton()`, if you want to use all PEAR objects. Because the object variables will be returned in uppercase letters and SGL works with lowercase variable names.

Insert the line after the fetchmode setting:

	$conn->setFetchMode(DB_FETCHMODE_OBJECT);
	$conn->setFetchMode('portability', DB_PORTABILITY_LOWERCASE);

This option is tested for !MySQL, !PostgreSQL and !MaxDB and works fine.

If you need more information use the PEAR docs [here][1]

## Next Step (OLD)
You have to replace all COALESCE-Functions in the SQL-Statements with the VALUE-Function, because VALUE() is the alias for COALESCE().

[1]:	http://pear.php.net/manual/en/package.database.db.intro-portability.php
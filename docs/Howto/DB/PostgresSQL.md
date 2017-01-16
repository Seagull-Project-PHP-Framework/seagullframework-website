<!-- Name: Howto/DB/PostgresSQL -->
<!-- Version: 5 -->
<!-- Last-Modified: 2007/04/24 15:28:24 -->
<!-- Author: demian -->
<!-- Status: Original -->

# PostgreSQL Troubleshooting

Just a little list of possible problems with PostgreSQL

1) configuring SGL to connect PgSQL through Unix socket, selecting port 5432, but pg\_hba.conf doesn't let user enter using password or md5 authentication for local connections.

php\_log.txt shows:

	May 24 13:32:44 Seagull [error] PEAR :: DB Error: connect failed :
	[nativecode=Unable to connect to !PostgreSQL server: FATAL:  IDENT
	authentication failed for user &quot;seagull&quot;]
	May 24 13:32:44 Seagull [error] sgl_errorhandler->errhandler: DB Error:
	connect failed

Solution: change pg\_hba.conf, the line:
   local   all  all      ident sameuser
must become:
   local   all  all      md5

otherwise use tcp connection if already configured for password/md5 authentication.



2) configuring SGL to connect PgSQL through Unix socket, *without* selecting tcp port 5432.

In php\_log.txt I see:

	May 24 13:37:48 Seagull [error] PEAR :: DB Error: connect failed :
	[nativecode=Unable to connect to !PostgreSQL server: could not connect to
	server: DÛ
	<80>nÔ·@xÔ·@xÔ·<80>nÔ·(rÿ¿ÄäÔ×ËmÈ·@xÔ·P|Ü^Th·
	        Is the server running locally and accepting
	        connections on Unix domain socket /var/run/postgresql/.s.PGSQL.3306?]
	May 24 13:37:48 Seagull [error] sgl_errorhandler->errhandler: DB Error:
	connect failed

Seems SGL want connect to the wrong tcp port (3306).
In mysite.default.conf.ini I see:

	[db]
	type=pgsql
	host=localhost
	protocol=unix
	port=3306
	user=seagull
	pass=seagull
	name=seagull
	bootstrap=0

and my theory seems confirmed by that port=3306.

In other configurations, adding an organisations works fine.
My PgSQL config give password/md5 authorization only for tcp connection
and SGL works fine when configured for tcp connection through 5432 port.

3) Instalation fails with translated error messages (Probed with Debian Sarge 3.1)

If the PostgreSQL server is configured to print error messages in any language other that English, the error message will not be correctly parsed by PEAR.

You can solve it adding (or uncommenting) this line at postgresql.conf

	lc_messages = 'C'               # locale for system error message strings 

4) Table name prefix

Since Seagull 0.6.2 release, there is a new feature: "table name prefix". In order to make it works with PostgreSQL, all subqueries on module.data.pg.sql files must be on a single line.
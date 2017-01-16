<!-- Name: Howto/DB/WorkingWithPostgres -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/12/30 22:36:40 -->
<!-- Author: demian -->
<!-- Status: Original -->

# Working with Postgres
* TOC
{:toc}

## Intro
Out of interest for Windows users, the latest version of Postgres now works fine under Windows.  Download it from [here][1].

## Setup
Very detailed instructions on setting up Postgres on Windows are covered in the above link so we'll not deal with them here.  To install the Seagull schema for Postgres in Windows follow these instructions from a DOS command prompt:

create the Seagull database

	C:\Documents and Settings\Administrator> createdb seagull

login to the database

	C:\Documents and Settings\Administrator> psql seagull

import the schema and data

	'' making sure your path to the schema is correct
	seagull=# \i  schema.pg.sql
	seagull=# \i  data.default.pg.sql


## Config
Then all you need to do is setup your Seagull config correctly, here are some typical settings:

	[db]
	type                    = pgsql
	host                    = localhost
	protocol                = unix
	port                    = 5432
	user                    = Administrator ; whatever your Windows login name is
	pass                    = 
	name                    = seagull

If you're ineterested in accessing your Postgres and Mysql databases from the same client, and like the idea of auto-complete SQL statements, try out the [Aqua Data Studio][2] tool, free for personal use.


## Troubleshooting
If you're running !PostgeSQL on linux and are connecting via Unix domain sockets instead of TCP/IP on the usual port 5432. you must modify two setting in the global conf file:

  * protocol = unix
  * host = false

The second, less obvious setting is very important, without it anything with DataObject will fail to connect to the db.  For more details see the  15-Dec-2003 06:47 post by Cybertinus at http://uk2.php.net/manual/en/function.pg-connect.php

[1]:	http://www.hagander.net/pgsql/win32snap/
[2]:	http://www.aquafold.com/
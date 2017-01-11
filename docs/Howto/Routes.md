<!-- Name: Howto/Routes -->
<!-- Version: 16 -->
<!-- Last-Modified: 2011/06/21 09:48:19 -->
<!-- Author: henryjuan -->

# Working with Routes
* TOC
{:toc}

## Setup
 * works with \>= PHP 5.2.3 
 * set your pear package manager to the system pear repo


	$ pear channel-discover pear.horde.org
	$ pear install horde/Horde\_Routes

 * login as admin and in Config -\> General 


	Input URL handlers = Horde\_Routes
	Additional include path = /path/to/pear/system/dir

 * edit your routes here


	seagull/var/routes.php


That's it for now.

## Caveats
 * When using Horde\_Routes single arg url construction invalid, eg: SGL\_Output::makeUrl('search'), instead you must use SGL\_Output::makeUrl('search', 'usersearch', 'usersearch')

## Related links
 * http://dev.horde.org/routes/
 * http://pear.horde.org/index.php?package=Routes


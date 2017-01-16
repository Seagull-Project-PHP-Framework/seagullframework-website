<!-- Name: Howto/Caching -->
<!-- Version: 4 -->
<!-- Last-Modified: 2007/10/24 15:35:27 -->
<!-- Author: demian -->
<!-- Status: In Progress -->

# Working with Caching
* TOC
{:toc}

## Overview
> since Seagull 0.6.3

The Seagull framework includes support for caching via the SGL\_Cache class which is basically a wrapper for PEAR's [Cache\_Lite][1] class.  The following types of caching are supported
 * file
 * function
 * output

## Usage
To cache any data in your code you need to get an instance of the SGL\_Cache object:

	$cache = &SGL_Cache::singleton();

then either retrieve cached results or perform a data call, and save the results to the cache.  A typical operation looks like this:


	$cache = & SGL_Cache::singleton();
	if ($serialized = $cache->get('all_users', 'perms')) {
	    $aPerms = unserialize($serialized);
	    SGL::logMessage('perms from cache', PEAR_LOG_DEBUG);
	} else {
	    require_once SGL_MOD_DIR . '/user/classes/UserDAO.php';
	    $da = & UserDAO::singleton();
	    $aPerms = $da->getPermsByModuleId();
	    $serialized = serialize($aPerms);
	    $cache->save($serialized, 'all_users', 'perms');
	    SGL::logMessage('perms from db', PEAR_LOG_DEBUG);
	}

You don't need require the SGL\_Cache lib, it's done by Seagull.

## How to load various cache types
 Default Cache\_Lite instance.


	1. $cache = &SGL_Cache::singleton();

Cache\_Lite\_Function instance with specified params on the fly.


	2. $cache = &SGL_Cache::singleton('function', array(
	         'dontCacheWhenTheResultIsFalse' => true,
	         'dontCacheWhenTheResultIsNull'  => true,
	         'lifeTime'                      => 3,
	         'debugCacheLiteFunction'        => true,
	     ));

Cache\_Lite\_Output instance.


	3. $cache = &SGL_Cache::singleton('output');

Force Cache\_Lite\_Function instance.


	4. $cache = &SGL_Cache::singleton('function', array(), true);

BC way to force cache.


	5. $cache = &SGL_Cache::singleton(true);

## Clearing the cache
You can clear cached results using the following convention:


	SGL_Cache::clear('group_name');

The clear method can be called statically for convenience.





[1]:	http://pear.php.net/manual/en/package.caching.cache-lite.php
<!-- Name: RFC/Seagull2 -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/05/11 11:14:45 -->
<!-- Author: demian -->

# Seagull2 Ideas

maintaining BC with Config array, but shortening call


	class SGL
	{
	    static $conf;
	}
	
	SGL::$conf['db']['name'] = 'myname';

<!-- Name: Howto/DB/Oracle -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/12/30 22:38:33 -->
<!-- Author: demian -->
<!-- Status: Original -->

# Getting SGL to work with Oracle

After the last update of pear::DB (1.7.4) it seems, that all necessary changes for oci8 are already implemented.

You only have to modify SGL\_Session::dbGc, if you want to store sessiondata in db.


	$query = "DELETE FROM {$conf['table']['user_session']} WHERE (TO_DATE('" 
	. SGL::getTime(true) . "') - TO_DATE(last_updated ]*86400 > $expiry";


In terms of date handling:

you have to set the NLS\_DATE\_FORMAT to the format used in seagull. You can do this by executing the following command 



	BEFORE (re)starting the apache webserver:
	
	export NLS_DATE_FORMAT="YYYY-MM-DD HH24:MI:SS"
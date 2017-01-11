<!-- Name: Howto/Logging -->
<!-- Version: 6 -->
<!-- Last-Modified: 2007/04/05 11:29:39 -->
<!-- Author: bjorn -->

# Logging
* TOC
{:toc}

## Overview

*Note*: _To use SGL::logMessage() you first need to enable logging in Seagull Admin -\> General -\> Configuration -\> Logs_

  * Use SGL::logMessage for detailed logging, or just PHP's error\_log() for simple messages.
  * You can optionally pass file and line args (see below)
  * You can also set the priority (defaults to PEAR\_LOG\_INFO), so PEAR\_LOG\_DEBUG messages will only be logged when logging is in debug mode.
  * Format is


	SGL::logMessage(String $msg[, String $file, Int $line, Constant $priority]);\`.

  * Examples:


		SGL::logMessage('I went here', PEAR_LOG_DEBUG);
		SGL::logMessage(null, PEAR_LOG_DEBUG); // Just record date/level/class/function
		SGL::logMessage('Parameter foo is set to  ' . $foo);  // Record at INFO level
Note that __CLASS__ and __FUNCTION__ are actually automatically set and output by the logMessage function.

Sample output:


	Aug 26 14:50:47 Seagull [debug] sgl_controller->_displaypage:

## Managing the logfile
As a general hint:  If you are running/managing a \*nix system, you can use logrotate tot control your logfiles: 



	Logrotate.conf:
	============
	# see "man logrotate" for details
	# rotate log files weekly
	weekly
	
	# keep 4 weeks worth of backlogs
	rotate 4
	
	# create new (empty) log files after rotating old ones 
	create
	
	# uncomment this if you want your log files compressed
	#compress
	
	# RPM packages drop log rotation information into this directory
	include /etc/logrotate.d


Create a file named 'seagull' in logrotate.d containing:



	/var/log/php_log.txt {
	    missingok
	    notifempty
	}


 

See [wiki:Howto/DebuggingIdeas] for more information.

#### Comment by aj on Thu Mar  8 15:51:15 2007
Note: If your log file grows over 2GB on Apache 1.3.x it will cause a white page to display.

[[AddComment]]
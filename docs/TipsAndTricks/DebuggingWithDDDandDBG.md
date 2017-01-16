<!-- Name: TipsAndTricks/DebuggingWithDDDandDBG -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/02/28 11:23:36 -->
<!-- Author: ilyahegai -->
# Debugging PHP using DDD and DBG
[[TOC]]
Sometimes a debugger is very useful. (please improve introduction)

This page will describe how to set up a free debugger for our PHP scripts.

[DDD](http://www.gnu.org/software/ddd/) is a graphical front-end for command-line debuggers for several debuggers like GDB, Perl debugger, JDB, etc...
[DBG](http://dd.cron.ru/dbg/home.php) is a free debugger (sources available) for PHP language.

All instructions below are related to my system (GNU/Linux Debian "unstable" with PHP downgraded to v4.3.10).

## DBG Installation
This chapter describe how to install the PHP extension for DBG:
  1. Download DBG from [http://dd.cron.ru/dbg/downloads.php], choose the version related to your system.
  2. Download even the DBG-CLI.
  3. Inside the tar/zip file there are a lot of versions of the same file *dbg.so*. Choose the one made for your PHP version and save it in directory /usr/lib/php4/20020439/, toghether the other PHP extensions libs. Pay attention that DBG for PHP 4.4.x non exists. You must use version 4.3.x or 5.x.
  4. Change your php.ini file, my is in /etc/php4/apache dir. Add at the end of the file these lines:

    [debugger]
    extension=dbg.so
    debugger.enabled = true
    debugger.profiler_enabled = true
    debugger.host_allow=127.0.0.1 192.168.0.4
    debugger.host_deny=ALL
    debugger.JIT_host = clienthost
    debugger.JIT_port = 7869
*debugger.host_allow* tell DBG which IP can be used to receive debugging data.
*debugger.host_deny* tell DBG to not send debugging data to that IP
*debugger.JIT_host* and *debugger.JIT_port* tell DBG where the debugger is listening for data. *clienthost* is a special DBG keyword that mean "the IP of the browser" asking the PHP page. (I'm not 100% sure, check this in DBG pages)
The other IP after 127.0.0.1 is the IP of my Ethernet card.
  5. Save the file and restart your Apache server.
  6. Check if DBG extension is loaded executing a phpinfo(). You must read: *with DBG v2.11.32, (C) 2000,2005, by Dmitri Dmitrienko* in the beginning section and below a section *DBG* with all actual parameters of the extension.

## DDD configuration
The Debugger is a front-end to the console debugger DBG-CLI. We need DBG-CLI to listen debugging data coming from the DBG extension installed above.
  1. Install DDD for your linux distribution. Default config should be ok.
  2. Extract *dbg-cli* binary file from its archive and put it into */usr/local/bin* directory
  2. To start DDD with DBG support just execute *ddd --debugger /usr/local/bin/dbg-cli* from your x-terminal, or create an icon with this command inside.
  2. Check in the bottom window if DBG is started. You should see:
 
    DBG php debugger, version 2.11.32, Copyright 2001, 2005, Dmitri Dmitrienko, www.nusphere.com
  3. By default DDD listen on 1001 port but our DBG will send its debugging data on port 7869, so we must change the listening port in DDD. Type this command in the bottom window:

    set port 7869
You can change this value even using the menu command 'Edit/DBG Settings'.
  4. Now DDD is ready for our debugging session.

## Debugging
  1. In the command window of DDD type the command: *listen*
  2. Now that DDD is ready to listen we can open our PHP script with our www browser. To activate the DBG debugger inside PHP just add *?DBGSESSID=1@localhost:7869* to the URL of your page. This setting will tell DBG extension to send its data to the DBG-CLI machine with ip = localhost and tcp port=7869.
  3. If we are lucky enough the browser will wait for HTML data but the execution of PHP script is blocked by our DDD+DBG debugger. So switch back to the DDD window. Here you must see the PHP source of the called page in the source window.
  4. From now you can, for example, press F5 key to advance step by step and see what happens in your variables. Other keys are available for looking inside variables or for setting breakpoints, just look inside the DDD menu or read its documentation.
  5. To add a breakpoint in the source you can do it from DDD window or you can add the instruction *DebugBreak();* inside your PHP source. DBG will stop execution on that instructions as it was a breakpoint.
  6. After ending debugging a page the DDD+DBG will end listening for other PHP execution. So before starting debugging another page you must type again the command *listen* in the DDD command window.
  7. You don't need to add DBGSESSID to the URL every time because the debugger will works on every pages from now in advance. To stop a debugging session you must add *?DBGSESSID=0* to the URL.

## Troubleshooting
  * If you get a page telling:

    Failed to start debug session
    reason:
    failed to establish connection to client host on localhost:7869
you have forgotten to give the *listen* command in the DDD command window.
  * I have tried to leave the DBG-CLI listening port to 10001 but running *listen* "netstat -na" tell me that dbg-cli is listening on a random port. I don't know why. The only working port I've found seems to be 7869.
  * Don't know if this debugger works if other debuggers, like Zend Studio Server, are installed in your PHP. I think is better to remove all other debugger before installing DBG extension. 
  * I haven't tested if DBG works with Zend Optimizer installed. If you have more info about it please write it here. ;-)
  * I know DDD can be used with *PHP Edit* but I'm working under Linux so Windows users are invited to test it and to add some info in this page.

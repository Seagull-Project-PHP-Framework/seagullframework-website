<!-- Name: Misc/DeveloperFriendly -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/14 00:25:21 -->
<!-- Author: demian -->
## Developer Friendly

The Seagull framework aims to be developer-friendly, with this in mind the following features are available:

  * extensive feedback on what your code is doing through customisable debug levels
    * logging of debug messages to file, screen, database
    * windows developers are recommended to download [http://www.mingw.org/msys.shtml MSYS], the Unix command-line emulator.  With this tool you can easily tail the log files and pinpoint bugs
  * uses DB_Dataobject for generating application entities: the developer only has to define entity tables in database, DB_Dataobject does the rest 
  * the framework is divided into entities, manager classes and templates plus the base libraries, mostly PEAR, so development tasks can easily be shared and progressed in parallel 
  * All manager classes follow the Validate, Process, Display model so once you've developed one it's quick to recycle your work and build new modules.  See [some project diagrams](http://seagull.phpkitchen.com/index.php/publisher/articleview/frmArticleID/14/staticId/6/) for a visual summary of the framework workflow, or see the [basic workflow](/wiki:Howto/General/BasicWorkflow/) section
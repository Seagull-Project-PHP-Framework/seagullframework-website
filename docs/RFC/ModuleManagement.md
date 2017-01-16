<!-- Name: RFC/ModuleManagement -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/13 19:29:11 -->
<!-- Author: werner -->
= Module Management = 
[[TOC]]

The following tasks need to be taken care of in order for full module management to be implemented:

## for CLI installs
  * [module dependencies](http://seagull.phpkitchen.com/docs/wakka.php?wakka=ModulesDependencies) need to be resolved
  * see what ideas can be adapted from the [SimpleSeagull](http://seagullfiles.phpkitchen.com/simpleseagull.tar.gz) project
  * ~~PEAR Server setup (requires PHP5)~~
  * bug needs to be resolved which causes uploading of current seagull PEAR archive to fail (xml-rpc debugging)
  * ~~Pear_PackageFileManager v2 needs to be developed (it will produce package2.xml files), this will be used to automatically generate the package files for all the modules (liaise with Greg Beaver)~~
  * ~~looks like above will have to be hacked with current PackageFileManager, pear convert, and XML_Tree~~
  * PEAR lib dependencies will be tied to current modules installed, so SGL::import() type method required to wrap PEAR includes so they can die gracefully on failure
  * resolution of [package2.xml notation required](http://seagull.phpkitchen.com/docs/wakka.php?wakka=Package2NotationRequired)
  * creation of generatePackage.php builder files for each module
  * all table variable names ($conf['table']['user']) must be stored in the conf.ini files for each module
  * installation should be handled by a seagull installer which has the advantage of not requiring root access like the post install scripts invoked from the new PEAR installer - the seagull installer will also have the advantage of PEAR::DB instance with the ability to create and populate schemas
  * in addition to the current attributes, module entities also need the ability to be hidden, to have author/license info, status info
  * ideally a better parser implementation needs to be done to deal with reading SQL definitions (see [precog](http://babylon.idlevice.co.uk/php/precog/))
  * current installer class needs to become more sophisticated:
    * concept of pre and post tasks needs to be introduced, like in PEAR installer
    * requirement for SGL_Installer to implement following methods (these will wrap DAL methods):
      * ~~addPerm/delPerm~~
      * addPage/delPage
      * addBlock/delBlock
      * addCategory/delCategory
      * addModule/delModule
      * ~~addPref/delPref~~
    * SQL constraint issues on install should be dealt with by disabling constraint checking during install, and this way be stored in table definitions, not separate constraint files
    * the module installer screen will need to be updated to accomodate new functionality
    * the same installer must be capable of initial install, subsequent module installs, uninstalls, and updates

## for web installs
  * HTML_Template_IT needs to be decoupled from PEAR_Frontend_Web, the template-neutral driver Seagull's flexy can be used
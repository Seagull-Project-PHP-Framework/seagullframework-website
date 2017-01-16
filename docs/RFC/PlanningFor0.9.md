<!-- Name: RFC/PlanningFor0.9 -->
<!-- Version: 7 -->
<!-- Last-Modified: 2008/08/13 07:22:13 -->
<!-- Author: demian -->
# Planning for 0.9

 * Removing all BC cruft
 * decrease/merge some tasks related to debugging configuration,
 * make Zend_Cache and routes default
 * improve_cmd_actionName() syntax
 * rename managers to contollers and make a separate folder for it, I don't like that we mix module's lib with contollers in one folder called "classes", we can at least split to two folders like "controllers" and "logic" or "model",
 * integrate  PHPUnit 3 with it's DBUnit support via phpUnderControl, for PHP5 libs it would be a huge improvement in testing,
 * simplify installation process - merging tasks,
 * improve event handling,
 * add plugin functionality (functionality could be plugged in thanks to some system events),
 * integrate new css framework to adminGui, add new admin theme,
 * make Routes default to handle URI (eases URI parsing process)
 * integrate Routes to navigation
 * all includes go at top of files
 * append fiter chain classes with Pre or Post to clarify execution order
 * fewer files included per request is not a concern, usage of APC or similar is presumed
 * create ability for a module to add to site-wide config

## PEAR packaging
 * Seagull package
 * $foo subpackage (plugins + modules)
   * eg, Seagull_db
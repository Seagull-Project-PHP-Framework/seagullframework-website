<!-- Name: Internal/ReleaseProcess -->
<!-- Version: 14 -->
<!-- Last-Modified: 2006/04/09 16:08:08 -->
<!-- Author: aj -->
# Release Process
[[TOC]]

## General
  * run tests, run release.sh
  * create and upload seagull pear package to SF
  * compile latest API docs and upload to project page
  * update 'downloads' box on sgl site, update download links
  * update live demo
  * notify registered sgl users


### PEAR process
 * "pear uninstall seagull"
 * remove last installed version from /var/www/html/tmp_install/seagull
 * remove /var/www/html/tmp/seagull
 * export clean copy of svn src to /var/www/html/tmp/seagull
 * copy sgl's PEAR override libs to /var/www/html/tmp/seagull/
 * run http://localhost/seagull/trunk/etc/generate_package_xml.php
 * cd to install dir and "pear package package2.xml package.xml"
 * test installed version
 * upload release to pear.phpkitchen.com


### Notify release websites
 * www.phpkitchen.com : [/User/DemianTurner Demian Turner]
 * freshmeat : [/User/DemianTurner Demian Turner]
 * hotscripts.com : [/User/DemianTurner Demian Turner]
 * phpmagazine: http://www.phpmagazine.net/contact.html
 * php announce mainlinglists (english, german, etc...)
 * php solutions magazine : [/User/WernerKrauss Werner]
 * german php magazin : [/User/WernerKrauss Werner]
 * php architect
 * direction php
  
### Other cms/framework related sites
  * http://www.contentmanager.net / http://www.contentmanager.de
  * http://opensourcecms.com
  * http://www.scripts.com/php-scripts/
  * http://php.resourceindex.com
  * http://scriptsearch.com
  * http://www.scriptsbank.com
  * http://www.needscripts.com
  * http://cmsmatrix.org/
  * http://www.oscom.org/matrix/index.html
  * http://de.groups.yahoo.com/group/small-cms/links/ (german)
  * http://www.cmswatch.com/ (makes sense to add there?)

### Other Wiki's / Sites about Seagull
  * http://faq.phpbar.de/index.php/Seagull : [/User/WernerKrauss Werner], also update in the news section.

### PHP News
  * http://www.phpn.org
 
### PHP announcement sites
#### English
  * http://www.phpbuilder.com/news/
  * http://www.phpfreaks.com/submit_news.php : [/User/MikeWattier Mike]
  * http://www.phpdeveloper.org/section/form_add_submission [/User/AjTarachanowicz AJ]
  * http://www.php-mag.net/magphpde/magphpde_services/psecom,id,1,d,9,nodeid,5,xm,1.html [/User/AjTarachanowicz AJ]

#### Germany
  * http://php-homepage.de/  : [/User/WernerKrauss Werner]
  * http://www.phpbar.de/  : [/User/WernerKrauss Werner]
  * http://www.php-center.de  : [/User/WernerKrauss Werner] short text, no html
  * http://www.phpmagazin.de   : [/User/WernerKrauss Werner] short text, no html
  * http://www.phpwelt.de/contact.php  : [/User/WernerKrauss Werner] short text, no html
  
#### Italy
  * http://www.ziobudda.net/ : [/User/PierpaoloToniolo Pierpaolo]

## Press Release

  * [0.6.0RC1](/wiki:PressRelease/0.6.0RC1/)

### DE
Seagull PHP Framework ist ein objektorientiertes, auf PEAR-Klassen basierendes MVC-Framwork. Es unterstützt MySQL, PostgreSQL, Oracle und theoretisch alle von PEAR::DB unterstützten Datenbanken. Es zeichnet sich durch einfache Bedienung und hohe 
Flexibilität aus.
  
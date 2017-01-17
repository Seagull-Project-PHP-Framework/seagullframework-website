<!-- Name: RFC/UpgradingTrac -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/06/11 11:33:52 -->
<!-- Author: demian -->

# Upgrading Trac
Currently kludgy template system means 
 * css stored in different directory to custom themes
 * custom headers/footers ignored by Trac
 * default CSS must be modified
 * default CSS always reverted to stock install every upgrade

## To customise
 * find CSS dir /usr/share/trac/htdocs/css 
 * grep for link-related CSS


	grep -R b00 .
 * replace with 408000
 * paste in google ads css rules from http://trac.edgewall.org/wiki/TracInterfaceCustomization
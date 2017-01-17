<!-- Name: Integration/XmlRpc -->
<!-- Version: 5 -->
<!-- Last-Modified: 2006/11/30 15:54:04 -->
<!-- Author: demian -->

# Creating and Consuming XML-RPC Web Services

## Introduction
By default Seagull provides and XML-RPC server which means you can serve any data that your persistence layer can access to a range of clients.  By requiring HTTP authorisation over an SSL connection (supported by PEAR XML-RPC package) you can reliably supply data to a subset of clients only.  Optionally you can use your Seagull installation to provide public web services in the style of PEAR, Yahoo, Google, etc.

== Setting up a Client ==  
See the \_checkLatestVersion() method in MaintenanceMgr [source:/trunk/modules/default/classes/MaintenanceMgr.php] Line 108 for an example of consuming a Seagull XML-RPC web service.

## Setting up a Server
The 'server' is simply the seagull/www/rpc/server.php file which is in its own directory so you can limit access to it with .htaccess, etc.

Seagull comes with RAD web service tool which allows you create and deploy XML-RPC based services quickly and easily.  See the contents of the services folder [source:/trunk/lib/SGL/XML/RPC/services] for an example of how to create a service.  To provide a data service the best approach is to:
 1. create your relevant data access methods, see either [source:/trunk/modules/user/classes/DA\_User.php] or [source:/trunk/modules/default/classes/DA\_Default.php] for examples
 1. create a file that contains a function with the same name as the file, and that calls the relevant data access method, see [source:/trunk/lib/SGL/XML/RPC/services/determineLatestVersion.php] for naming conventions
 1. wrap the returned data structure in accordance with the XML-RPC standard, see above file for an example
 1. setup access constaints if required, ie using an Apache .htaccess file in the rpc directory
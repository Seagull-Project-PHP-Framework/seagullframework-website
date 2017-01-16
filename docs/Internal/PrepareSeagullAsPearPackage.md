---
layout: page
title: Preparing PEAR Packages
permalink: /Internal/PrepareSeagullAsPearPackage.html
---

<!-- Name: Internal/PrepareSeagullAsPearPackage -->
<!-- Version: 6 -->
<!-- Last-Modified: 2006/08/29 12:32:13 -->
<!-- Author: aj -->
<!-- Status: Updated -->

# Preparing PEAR Packages
One of the formats Seagull is currently available in is as a PEAR package.  For more info on how to install PEAR packages see [Using the PEAR package manager][1].

To create PEAR package for your Seagull modules follow these basics:

  * get more info on the PEAR package format http://pear.php.net/manual/en/developers.packagedef.php
  * check out the package builder script [gitlink:trunk/etc/generatePearPackageXml.php]
  * create your own release notes for $notes
  * modify the script's listing of all the dependencies on PEAR libs you may have added/removed
  * it's easiest to work with an exported version of Seagull, ie, no svn folders
  * specify the path to the directory where your module can be found, in `$packagedir`
  * modify the location and roles of any files you may have changed
  * call the generate script from a web browser with a 'make' parameter, ie, http://localhost/myProject/etc/generatePearPackageXml.php?make (or you can call from CLI)
  * this will create a `package2.xml` file in the root of your module folder
  * at the commandline, in the module's root folder, with the PEAR binary in your path, type 

		$ pear package package2.xml

That's all there is too it.  The resulting `.tgz` archive can be uploaded to any correctly configured PEAR channel server.

[1]:	/Installation/UsingThePearPackageManager.html "Using the PEAR package manager"
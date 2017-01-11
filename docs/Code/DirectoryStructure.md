<!-- Name: Code/DirectoryStructure -->
<!-- Version: 8 -->
<!-- Last-Modified: 2007/10/27 12:47:42 -->
<!-- Author: lyric -->

# The Seagull Directory Structure

The Seagull directory structure has been organised with the unix filesystem in mind, following conventions set out in the [Filesystem Hierarchy Standard][1].
The following is a schematic of the directory structure:

  * *seagull*
	* \'''etc''' '': et cetera: basic configuration files, sql-files, etc...''
	* \'''lib''' '': libraries:''
	  * \'''SGL''' '': Seagull core libraries''
	  * \'''data''' '': data files, mostly arrays, for things like countries, languages, states, etc ''
	  * \'''other''' '': other libs''
	  * \'''pear''' '': PEAR libs''
	* \'''modules''' '': for modules'' 
	  * \'''faq''' '': the faq-module for example''
		* \'''classes''' '': the module-specific classes''
		* \'''lang''' '' the module specific lang files''
		* \'''data''' '' the module specific sql-data''
		* \'''www''' '' the module specific www-data''
		  * \'''css''' '' the module specific css-files''
		  * \'''images''' '' the module specific images''
		  * \'''js''' '' the module specific javascript-files''
	* \'''var''' '': all temporary data goes here, must be writable by the webserver ''
	  * \'''cache'''
		* \'''entities''' '': !DataObject entities ''
		* \'''tmpl''' '': Flexy's compiled templates ''
		  * \'''default''' '': the theme name, by default it's called 'default' ''
	  * \'''log''' '': file-based logging goes here ''
	  * \'''tmp''' '': sessions get stored in the tmp dir, when file-based handler is used ''
	* \'''www''' '': application webroot, only this dir should be viewable to the web, make sure to protect rest with .htaccess if otherwise''
	  * \'''fck''' '': wysiwyg tool for editing html ''
	  * \'''images''' '': for uploaded images. Used by fck editor ''
	  * \'''js''' '': all javascript goes here, regardless of module ''
	  * \'''themes''' '': the themes go here''
		* \'''default''' '': this is also for module templates''
		  * \'''css''' '': .css and .htc files ''
		  * \'''images'''
			* \'''uploads'''
		  * \'''faq''' '': the templates of the faq-module''

[1]:	http://www.pathname.com/fhs/
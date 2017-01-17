<!-- Name: Howto/Templates/Savant -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/11/30 15:49:33 -->
<!-- Author: demian -->

# Using Savant2 as your Template Engine

Thanks to Andrey Podshivalov we now have the [Savant2][1] template engine integrated into Seagull.  To use simply download the Savant2 package using the PEAR installer:


	$ pear channel-discover savant.pearified.com
	$ pear install savant/savant2

Then in Config select 'Savant2' from the 'Template Engine' dropdown.  This will use the templates from the Savant theme:

	/path/to/seagull/www/themes/savant

With this setup, continue to develop your modules as usual, taking advantage of Savant2's unique features.

[1]:	http://www.phpsavant.com/
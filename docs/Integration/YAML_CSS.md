<!-- Name: Integration/YAML_CSS -->
<!-- Version: 2 -->
<!-- Last-Modified: 2008/04/23 15:49:51 -->
<!-- Author: demian -->

# Integrating YAML CSS
[[TOC]]
## Overview

Integrating the YAML CSS  is really simple in Seagull:

 1. create your new theme, let's call it foo
 1. add the yaml src in sgl/www/themes/foo/css so you have:

		sgl/www/themes/foo/css/navigation
		sgl/www/themes/foo/css/patches
		sgl/www/themes/foo/css/screen
		sgl/www/themes/foo/css/yaml
		sgl/www/themes/foo/css/my_root_yaml_file.css
 1. in sgl/www/themes/foo/default/header.html just add this line:


		{makeCssOptimizerLink(##,#my_root_yaml_file.css#):h}


Put this right after the javascript variables at the top of the file and that's it!
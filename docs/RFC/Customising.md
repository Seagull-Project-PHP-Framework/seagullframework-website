<!-- Name: RFC/Customising -->
<!-- Version: 2 -->
<!-- Last-Modified: 2007/01/09 17:24:02 -->
<!-- Author: demian -->
# Making it easier to customise a Seagull install
[[TOC]]
## current probs
 * every project has unique navigation, so basically all navigation.php files must be unique
 * blocks are unique, so *.block.add.sql must be unique
 * Task/Install.php doesn't take custom module dir into account

## current benefits
 * installer can have most values customised

## suggestions
 * ability for a bash script to overwrite stock sgl files if necessary

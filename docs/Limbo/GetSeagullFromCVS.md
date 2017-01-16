<!-- Name: Limbo/GetSeagullFromCVS -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/12/30 22:40:47 -->
<!-- Author: demian -->
FIXME: Look over new file... 

FIXME: can be deleted cause it's outdated? [/WernerKrauss WernerKrauss] /05.11.2005 21:38/
[[TOC]]

## Why CVS?

If you want to take advantage of the latest project developments, you can go to our (?CVS==Concurrent Versions System?) server and get the most recent version of the code.

_please note: like any code from a (?CVS==Concurrent Versions System?) repository, this version may contain bugs, but most of the time it's fairly stable ..._

If you want to submit patches or translations, please *always use the latest (?CVS==Concurrent Versions System?) version*:

_cvs -d:pserver:anonymous@phpkitchen.com:/var/cvs login_'''


## How to get the latest CVS version:

`$ cvs -d:pserver:anonymous@phpkitchen.com:/var/cvs login`

password: seagull

`$ cvs -d:pserver:anonymous@phpkitchen.com:/var/cvs get seagull`

if you don't want to type all the pserver stuff, export the variable CVSROOT 
like this

## How to check which of your files changed compared with what's in CVS:

`$ cvs -n -q update`

## How to return to the CVS version after applying a patch, ie, how to rollback a patch:

`$ cvs up -d -P -C`

## How to create a tag

`$ cvs -q tag release_0_4_0dev3`

## How to export a release

`$ cvs -q export -r release_0_4_0dev3 seagull`

## How to update an existing copy

_current directory_ 
`$ cvs update`

_recursively, ie, for all directories_ 
`$ cvs update -d`

_recursively, and removing (pruning) empty directories_ 
`$ cvs update -d -P`


## Hints

`$ CVSROOT=pserver:anonymous@phpkitchen.com:/var/cvs `
`$ export CVSROOT`
`$ cvs login`
_type the above password_
`$ cvs get seagull`

''note: on my Linux it works as following:
$ export CVSROOT=:pserver:anonymous@phpkitchen.com:/var/cvs''

If you wondered how to keep your modified SGL up to date, have a look at /TipsAndTricks/UpdatingFromCVS

## CVS Programs

  * Windows:
    * [http://www.tortoisecvs.org/ TortoiseCVS]
  * Linux
    * type `cvs` in your console (see above)
 
Some editors and (?IDE= Integrated Development Environment?)s like [Quanta](http://quanta.sourceforge.net/) (KDE) or [eclipse](http://www.eclipse.org/) (java, all platforms) have (?CVS =Concurrent Versions System?) support built in or available via plugin.
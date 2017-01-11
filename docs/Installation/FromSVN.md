---
layout: page
title: Checking out code from svn
permalink: /Installation/FromSVN.html
---

<!-- Name: Installation/FromSVN -->
<!-- Version: 37 -->
<!-- Last-Modified: 2008/10/20 16:32:23 -->
<!-- Author: demian -->

# Subversion
* TOC
{:toc}

## Introduction

If you want to take advantage of the latest project developments, you can go to our subversion server and get the most recent version of the code.

> please note: like any code from an svn repository, this version may contain bugs, but most of the time it's fairly stable..._ 
If you want to submit patches or translations, please *always use the latest SVN version*:

## How to get the latest svn version:
For the latest bugfixes for a release, use the bugfix branch:


	$ svn co http://svn.seagullproject.org/svn/seagull/branches/0.6-bugfix
	
	
	#!html
	<p style="color: Red;">
	NOTE: CURRENTLY THE LATEST CODE CAN BE FOUND IN THE 0.6-BUGFIX BRANCH ABOVE
	</p>

In your \~/.subversion/config you can optionally set the following:


	[miscellany]
	enable-auto-props = yes
	
	[auto-props]
	*.php = svn:eol-style=native
	*.html = svn:eol-style=native
	*.lang = svn:eol-style=native
	...
	etc.

This way eol is kept consistently in the repository (auto props apply only to new files added).

### User and Password
There is NO username or password required. In tortoise leave these fields blank

### List Branches
You see a list of all branches at http://svn.seagullproject.org/svn/seagull/branches/

## How to check which of your files changed compared with what's in SVN:


	$ svn status

## How to check which repo (remote) files have changed compared with yours:
	$ svn status -u


## How to create a tag
	$ svn copy http://svn.seagullproject.org/svn/seagull/trunk http://svn.seagullproject.org/svn/seagull/branches/foo


## How to export a release

	$ svn export http://svn.seagullproject.org/svn/seagull/trunk


## How to update an existing copy
Be sure you are in the directory where your seagull installation resides. If you checked out seagull in /home/user/seagull be sure to cd /home/user/seagull before running the update command.


	$ svn update

## How to submit a patch

See the section on creating and [Submitting Patches][1]

## How to merge

This scenario illustrates how you would merge the bugfix branch down into trunk.  

First cd into the trunk


	$ cd /var/www/html/seagull/trunk


Then get the revision number for when the branch was last merged down to trunk


	$ svn log --stop-on-copy


Say it's revision 1823.

Run the merge with a --dry-run, when you're sure, run without


	$ svn --dry-run merge -r 1824:HEAD http://svn.seagullproject.org/svn/seagull/branches/0.4-bugfix


where the args represent:
svn merge -r[evision] [revision when branched]:[revision of 'merge from'] [url of 'merge from']

the final argument is omitted in the example, it's the target directory of where the files will be merged into, this is presumed to be "." or the current directory

## How to switch a repo
If you are using the same server


	$ svn switch http://svn.seagullproject.org/svn/newrepos /path/to/local/copy


If you are switching from a server that no longer works to a new server


	$ svn switch --relocate http://seagull.phpkitchen.com:8172/svn/seagull/trunk http://svn.seagullproject.org/svn/seagull/trunk /path/to/local/copy


## How to import a repo

	$ svnadmin create --fs-type fsfs /usr/local/svn/newrepos

(subversion creates the filesystem type 'fsfs' by default from version 1.2 onwards, you *really* want to use this type as BDB can be highly unstable, especially with BDB 4.1)

then setup the  relevant apache perms , and:


	$ svn import mylocaldir http://svn.seagullproject.org/svn/newrepos/mylocaldir


## How to dump one repo and load it into another filesystem type
This is extremely useful if you're stuck using the BDB fs type, and want to recreate your repo in FSFS.

Dump the existing repo


	$ svnadmin dump /usr/local/svn/oldrepos > myfile.dump

delete (and potentially backup) existing repo


	$ rm -rf /usr/local/svn/oldrepos

create new repo


	$ svnadmin create --fs-type fsfs /usr/local/svn/newrepos

load dumpfile


	$ svnadmin load /usr/local/svn/newrepos < myfile.dump

## How to delete a folder

	$ svn remove http://svn.seagullproject.org/svn/seagull/branches/folder_to_delete

## How to setup svn:externals for vendor branches
It is advisable to put 3rd party libs in their own branch, at the same level as trunk, branches, tags.
 * create the top level folder called 'vendor'
 * cd into vendor and export your lib dir there, ie repo/vendor/pear
 * cd into your lib dir and type

	`svn propedit svn:externals .`
 * if your environment doesn't have the EDITOR variable set, do

	`export EDITOR=vim`
 * link the vendor branch with its target location in your repo as follows

	pear http://sdn.seagullsystems.com/svn/customer/project/trunk/pear
 * svn up from your local checkout to pull the vendor lib into its correct location
 * commit the svn  property for all users of the repo to benefit from the link

## How to set svn properties
Setting the revision number to the file

	svn propset svn:keywords "Revision" file.php

## How to make a file in your repo executable

	svn propset svn:executable ON etc/deploy_local.sh


## How to repair a damaged repository

	$ svnadmin verify /usr/local/svn/seagull
	
	
	$ svnadmin recover /usr/local/svn/seagull
	
	
	$ chown -R nobody /usr/local/svn/seagull


## Hot backup

	$ /usr/local/src/subversion-1.1.4/tools/backup/hot-backup.py /usr/local/svn/seagull /some/backup/dir

## Post-commit hooks, closing tickets
You can pass arguments in your commit messages, these are then searched for text in the form of:


	  command #1
	  command #1, #2
	  command #1 & #2 
	  command #1 and #2
You can have more then one command in a message. The following commands are supported. There is more then one spelling for each command, to make this as user-friendly as possible.



	 closes, closing, closed, fixes, fixing, fixed
	    The specified issue numbers are closed with the contents of this
	    commit message being added to it. 
	
	 references, refs, addresses, re 
	   The specified issue numbers are left in their current status, but 
	   the contents of this commit message are added to their notes.
 

A fairly complicated example of what you can do is with a commit message of:

	   Changed blah and foo to do this or that. Fixes #10 and #12, and refs #12.


This will close #10 and #12, and add a note to #12.

## Remove .svn Directories

	find . -name '.svn' -type d -exec rm -rf {} \;

## List contributors with number of commits each

	svn log -q | awk '/^r/ {print $3}' | sort | uniq -c


## SVN Programs

  * Command line client for all systems:
	* [http://subversion.tigris.org/ Subversion at Tigris.org]
	* type `svn` in your console
  * Windows XP/2k:
	* [http://tortoisesvn.tigris.org/ TortoiseSVN] - Integrates with shell (Windows Explorer)
  * All Windows:
	* [http://rapidsvn.tigris.org/ RapidSVN] - wxWidgets based. Lacks some doc.
	* [http://winmerge.sourceforge.net/ WinMerge] - Very good diff viewer.
  * Linux
	* [http://rapidsvn.tigris.org/ RapidSVN] - wxWidgets based. Lacks some doc.
	* [http://meld.sourceforge.net/ Meld] - Good diff viewer. End of page lists some others graphical diff clients, [http://kdiff3.sourceforge.net/ Kdiff3] is good too, also Kompare from the [http://kde.org KDE SDK]
	* [http://de.kde-apps.org/content/show.php?content=26589 kdesvn] - Uses its own svn libs
  * Mac
	* [http://www.lachoseinteractive.net/en/community/subversion/svnx/features/ Svn X]
	* [http://wiki.ehow.com/Install-Subversion-on-Mac-OS-X Installing svn]


## See also
 * http://svn.collab.net/repos/svn/trunk/doc/user/cvs-crossover-guide.html
* [svn reference][2]

[1]:	/Code/SubmittingPatches.html
[2]:	/files/svn-refcard.pdf "SVN Reference"
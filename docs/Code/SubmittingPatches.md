<!-- Name: Code/SubmittingPatches -->
<!-- Version: 16 -->
<!-- Last-Modified: 2007/04/17 17:12:39 -->
<!-- Author: lyric -->
<!-- Status: In Progress -->

* TOC
{:toc}

# Want to share your code? Fine!

The policy for all developers wishing to contribute to the Seagull project is that they retain their original copyright, and grant to [Demian Turner][1], the Seagull project maintainer and the Seagull group, a non-exclusive, worldwide, no-cost license to use the works and/or derived works under whatever license is used by the project. 

All code must be provided with the standard Code/SeagullHeader notice, and the contributor is expected to list himself as the author of the code at the bottom of the notice.

## Submitting Patches

See [http://pear.php.net/manual/en/contributing.patches.php] for the PEAR guide to creating patches.

To create a patch for Seagull, first you must get the latest code from SVN:

[wiki:Installation/FromSVN]

Make your changes ...

then change to the Seagull root directory, i.e. if your SGL installation is in /var/www/html/seagull go to 
that directory with a:



	$ cd /var/www/html/seagull


If you created new files in the checked out copy of Seagull you have to add them to svn.



	$ svn add path/to/file


then execute:



	$ svn diff > your.diff


Please post all patches to the [Trac][2] site.  If your patch matches an [existing ticket][3], please attach it to that ticket or create a [new one][4] accordingly. Next make sure you assign the ticket to the user 'demian'.

*NB*: We've setup Trac as read-only for anonymous users.  To add a patch or create a ticket you will need to [sign up on the Seagull site][5] and use the same details to log into Trac. 

## Create from Seagull Root Directory

*Remember, please create all patches from the Seagull root directory!*

This is how the patch file header looks like if you create the patch from the root directory:

	Index: modules/navigation/classes/CategoryMgr.php
	===================================================================
	RCS file: /var/cvs/seagull/modules/navigation/classes/CategoryMgr.php,v
	retrieving revision 1.14
	diff -u -r1.14 CategoryMgr.php
	--- modules/navigation/classes/CategoryMgr.php  29 Jan 2005 10:22:28 -0000  1.14
	+++ modules/navigation/classes/CategoryMgr.php  2 Feb 2005 12:44:32 -0000
	@@ -252,11 +252,11 @@

(the path from line 1 is taken from the root of the SGL directory and is the same to line 6 and 7. If necessary edit this manually)

## Diff Only Single File Or Single Directory

In case you only want to diff one directory recoursively you may use



	$ svn diff modules/publisher > your.diff


Same for only one file:



	$ svn diff modules/publisher/lang/german-iso-8859-1.php > your.diff


This is quite handy if you changed something you don't want to diff.

## Use Unix EOL

Also, please remember, especially if you're submitting a new file, Unix EOL is essential, ie, /n instead of /r/n.  If you're working on Windows and your editor does not have an option to save files with Unix EOL, simple run one of the following one-liners, the 2nd of which also converts tabs to 4 spaces:



	perl -pi -e 's/\r\n?/\n/g' $file


or



	tr -d '\15\32' < dos.txt > unix.txt


or with awk



	awk '{ sub("\r$", ""); print }' dos.txt > unix.txt


or use the program: dos2unix

## Use 4 Spaces Instead Of Tabs

and for tabs to 4 spaces:



	tr '\t' '    ' < file > file.new && mv -f file.new file

(may not work, depends on your version of tr)

or



	sed '/\t/    /g' < file > file.new && mv -f file.new file

(depending on your sed version, you may need to type an actual tab char instead of \t)

## See also:
  *  [wiki:Howto/Internationalisation/SubmittingTranslations]
  * http://svnbook.red-bean.com/en/1.1/ch03s06.html#svn-ch-3-sect-6.2

## Patching Seagull
If you want to add a patch to your existing Seagull installation use either the special function for applying patches like built in in phpEclipse or simply type to your linux-console:


	$ cd your/seagull/root/directory/
	$ patch -p0 --dry-run < your_patch_file.txt
	
	(Note: on FreeBSD patch does not recognize --dry-run option. Use -C or --check as it is equivalent.)

to test, then remove the `--dry-run` if no errors are reported.

[1]:	/User/DemianTurner.html
[2]:	http://trac.seagullproject.org/
[3]:	http://trac.seagullproject.org/report/6
[4]:	http://trac.seagullproject.org/newticket
[5]:	http://seagullproject.org/user/register/
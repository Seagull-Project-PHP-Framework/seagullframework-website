<!-- Name: Environment/SVN -->
<!-- Version: 37 -->
<!-- Last-Modified: 2006/09/21 01:42:34 -->
<!-- Author: cyril -->
## Setting up SVN Repository

[[TOC]]

### Creating Repository


    svnadmin create project


### Repository Structure
This is only a suggested structure and may not fit your project's needs. Feel free to adapt to your needs and be sure to contribute back anything that you find helps your project succeed.


#### Main Structure

 * branches/ - Branches are created for major enhancements, bugfixes, demo, testing, etc.
 * tags/ - Tagging of releases.
 * tags/modules/ - Branch that contains tags for your custom Seagull modules.
 * tags/releases/ - Branch that contains tags for your application releases.
 * trunk/ - Development Branch.
 * vendors/ - Branch that contains Seagull and other 3rd party apps.


Creating the repository structure in your repository.

Download the attached file [attachment:seagull-svn-structure.txt].


    svn mkdir svn+ssh://localhost/svn/project -F seagull-svn-structure.txt -m 'Created Project structure'

Tagging a release 


    svn copy svn+ssh://localhost/svn/project/modules/block/current svn+ssh://localhost/svn/project/modules/block/tags/1.0 -m 'Version 1.0'

#### Vendor Structure

 * vendors/
 * vendors/seagull
 * vendors/seagull/current
 * vendors/seagull/0.5.4
 * vendors/gallery2/
 * vendors/gallery2/current
 * vendors/gallery2/1.5.2 

Import the current version into current/ then copy to version number.

We'll use Seagull as a use case. 

You would first start by exporting the latest version of seagull and then import Seagull into the vendor branch. Next tag with version number and bring source into trunk for development.


    svn export http://svn.seagullproject.org/svn/seagull/tags/0.5.4 seagull-0.5.4
    
    svn import svn+ssh://localhost/svn/project/vendors/seagull/current seagull-0.5.4/ -m 'Importing Seagull 0.5.4'
    
    svn copy svn+ssh://localhost/svn/project/vendors/seagull/current svn+ssh://localhost/svn/project/vendors/seagull/0.5.4 -m 'Creating tag for 0.5.4'
    
    svn copy svn+ssh://localhost/svn/project/vendors/seagull/0.5.4 svn+ssh://localhost/svn/project/trunk -m 'Bringing Seagull into trunk'


## Upgrading

#### Upgrading to Seagull 0.5.5

First you want to check out the 'current' branch in vendors/seagull and copy Seagull 0.5.5 on top of your project's current Seagull version. 


    svn co svn+ssh://localhost/svn/project/vendors/seagull/current seagull-current
    
    svn export --force http://svn.seagullproject.org/svn/tags/0.5.5 seagull-current/

Next you'll want to run 'svn status' in 'seagull-current' directory to find new files, delete files, and conflicts. A '?' indicates a new file, '!' indicates a missing file, and 'C' indicates a conflict. Use the 'svn add filename.php' and 'svn delete filename.php' commands to add and delete files. After you have resolved the conflict in a file you need to use 'svn resolve filename.php'


    svn st | grep 'C' 
    
    svn st | grep '!'
    
    svn st | grep '?'

Now you need to commit your changes and tag the 'current' branch as Seagull 0.5.5.


    svn ci svn+ssh://localhost/svn/project/vendors/seagull/current seagull-0.5.5/ -m 'Upgrading to Seagull 0.5.5'
    
    svn copy svn+ssh://localhost/svn/project/vendors/seagull/current svn+ssh://localhost/svn/project/vendors/seagull/0.5.5 -m 'Creating tag for 0.5.5'

Next you'll want to add your custom modifications to Seagull 0.5.5. To do this you need check out a copy of trunk and merge it with Seagull 0.5.5.


    svn co svn+ssh://localhost/svn/project/trunk seagull-merge
    
    svn merge svn+ssh://localhost/svn/project/vendors/seagull/0.5.4 \
              svn+ssh://localhost/svn/project/vendors/seagull/current \
              seagull-merge

You need to resolve conflicts, add files, and remove files exactly how you did above. Only this time you want to be in the directory 'seagull-merge' when you use the 'svn status' command.

Now commit your changes


    svn ci -m 'Copied custom modifications into Seagull 0.5.5'

#### Upgrading with help of 'svn_load_dirs.pl'
Vendor drops that contain more than a few deletes, additions and moves complicate the process of upgrading to each successive version of the third-party data. So Subversion supplies the svn_load_dirs.pl script to assist with this process. This script automates the importing steps to make sure that mistakes are minimized. You will still be responsible for using the merge commands to merge the new versions of the third-party data into your main development branch, but svn_load_dirs.pl can help you more quickly and easily arrive at that stage.

In short, svn_load_dirs.pl is an enhancement to svn import that has several important characteristics:

 * It can be run at any point in time to bring an existing directory in the repository to exactly match an external directory, performing all the necessary adds and deletes, and optionally performing moves, too.
 * It takes care of complicated series of operations between which Subversion requires an intermediate commit—such as before renaming a file or directory twice.
 * It will optionally tag the newly imported directory.
 * It will optionally add arbitrary properties to files and directories that match a regular expression. 

The file svn_load_dirs.pl is included in the source distribution of Subversion, contrib/client-side/.
 1. Copy svn_load_dirs.pl.in from /usr/share/subversion/contrib/client-side/ to /usr/bin
 1. Remove the ".in" from the end of the file name
 1. Replace "@SVN_BINDIR@" in the script with the place where your Subversion binaries are installed /usr/bin 

When an update to Seagull is ready, export the latest version of Seagull, update the vendor branch and automatically create a tag (svn_load_dirs.pl takes three mandatory arguments. The first argument is the URL to the base Subversion directory to work in. This argument is followed by the URL—relative to the first argument—into which the current vendor drop will be imported. Finally, the third argument is the local directory to import.)

 1. local vendor branch = svn+ssh://localhost/svn/project/vendors/seagull 
 1. the current vendor source (in this case 0.5.4) = current i.e. svn+ssh://localhost/svn/project/vendors/seagull/currrent
 1. the export directory = seagull-0.5.5
The -t option will automatically create a tag in this case svn+ssh://localhost/svn/project/vendors/seagull/0.5.5


    svn export http://svn.seagullproject.org/svn/seagull/tags/0.5.5 seagull-0.5.5
    
    svn_load_dirs.pl -t 0.5.5 svn+ssh://localhost/svn/project/vendors/seagull current seagull-0.5.5

Next you'll want to add your custom modifications to Seagull 0.5.5 as already explained in the previous section.


The free book [[Version Control with SVN](http://svnbook.red-bean.com/)] covers vendor banches in detail in [[Chapter 7. Vendor Branches](http://svnbook.red-bean.com/nightly/en/svn.advanced.vendorbr.html)] 
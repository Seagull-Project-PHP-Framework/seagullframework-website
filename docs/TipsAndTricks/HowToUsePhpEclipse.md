<!-- Name: TipsAndTricks/HowToUsePhpEclipse -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/05/31 15:57:19 -->
<!-- Author: kanji78 -->
## How to use PhpEclipse and Subeclipse with Seagull

There are two ways:
 * download EasyEclipse for Php (Eclipse distribution with Php plugin already installed)
 * install plugin manually

About EasyEclipse http://www.easyeclipse.org/site/distributions/php.html

About manual install:

1) Download and Install PHPEclipse 

 * http://www.phpeclipse.de/
 * http://www.plog4u.org/index.php/Using_PHPEclipse

 * If you want to work with xampp (good package Apache+Php+MySql): http://www.diotalevi.com/weblog/2005/03/23/48/

2) Install Subeclipse (Eclipse plugin that adds Subversion integration to the Eclipse IDE)
   
 * http://subclipse.tigris.org
 * http://subclipse.tigris.org/install.html

3) Import seagull from svn

 *  Launch Eclipse.
 *  Select "Window -> Open Perspective -> Other... -> Svn Repository Exploring".
 *  Under "Svn Repository" right-click and select "New -> Repository Location".
 *  In "Url" insert http://svn.seagullproject.org/svn/seagull/branches/
 *  Click "Finish"
 *  Right-click in the url of repository or one of its subdirectories and choose "Check Out as Project".

4) PHPEclipse

 *  In eclipse: "Window -> Open Perspective -> Other... -> PHP"
 *  On "Navigator window" you see your project.
 *  For list of svn task right-click and select Team 

Ulterior information about PHPEclipse and Subeclipse choose under eclipse "Help -> Help Contents" and on left side there are "Subclipse - Subversion Eclipse Plugin" and "PHPEclipse Help" .

5) For project without svn:

 *  Select "File -> New -> Project -> PHP -> PHP Project"
 *  Type project name (i.e. seagull) and "Finish"
 *  Under "Navigator" right-click on project name and select "Import -> File System" and choose directory where you have Seagull file and select file                        that you want to import.

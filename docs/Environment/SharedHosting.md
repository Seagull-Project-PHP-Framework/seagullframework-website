<!-- Name: Environment/SharedHosting -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/01/26 23:57:41 -->
<!-- Author: aj -->

## Seagull on Shared Hosting

### Setting up Seagull with SSH Access

  1. Download Seagull from Sourceforge.
  2. In your home directory create a directory called lib.
  3. Decompress Seagull in the new lib directory.
  4. Move public\_html to public\_html.bak
  5. Create a symbolic link from /home/user/lib/seagull/www to public\_html

	ln -s /home/user/lib/seagull/www public\_html
  6. Make the 'var/' directory sticky (all new files created in this directory will be created with the same group as 'var/') and writable.

	cd /home/user/lib/seagull
	chmod g+s var/
	chmod 777 var/
  7. Next, run the installer by entering the url to your public\_html directory in your browser. (ex. http://localhost/\~username/ or http://domainname/)


### Setting up Seagull without SSH Access

  1. Download Seagull from Sourceforge.
  2. Decompress Seagull in a temp directory.
  3. Using FTP upload all the directories (etc/, lib/, modules/, tests/, www/, var/) and files within the Seagull directory to the directory below your web root. This is usually your home directory.
  4. Next, make the 'var/' directory writable.

	cd /home/user/lib/seagull
	chmod 777 var/
  5. Now rename your web directory to 'public\_html.bak'
  6. Rename the Seagull 'www' to 'public\_html'
  7. Run the Seagull Installer and in step 5 set the proper path to the Seagull www directory (ie. /home/user/public\_html)

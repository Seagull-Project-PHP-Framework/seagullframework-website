<!-- Name: TipsAndTricks/CleanupScript -->
<!-- Version: 5 -->
<!-- Last-Modified: 2006/09/11 13:27:11 -->
<!-- Author: werner -->
# Removing Unwanted Language Files

I made a little cleanup script that deletes some files so i don't have to upload that much on my webserver:

  * CVS directories
  * unwanted lang files (in my case i only need the files for english and german language)



    #!/bin/bash
    
    function removeLang () {
    echo "- "$i;
    for i in "$1"/*php;
        do
        case "$i" in
    # add your wanted languages here...
          *"german-iso-8859-1.php"   ) ;;
          *"english-iso-8859-15.php" ) ;;
          *"www"* ) ;;
          * ) rm $i;;
        esac;
      done
    }
    
    echo "Removing CVS Directories:";
    find -name CVS -type d -exec rm -rf {} ";" 2> /dev/null
    
    echo "Removing unwanted languages:";
    
    for i in $( find * -name lang -type d; )
      do
        removeLang $i;
      done
    
    echo "Done. Good Luck";
    

## simple find command for bash
I found a find one-liner to remove all lang files (exept german and english).

Try without "-exec" command and test if it only finds files you want to delete first.

Use on your own risk out of the modules dir. 


    find .  -path './*/lang/*' -not -regex '.*\(german\|english\).*' -exec rm {} \;
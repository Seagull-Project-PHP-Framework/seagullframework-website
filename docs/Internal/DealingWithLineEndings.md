<!-- Name: Internal/DealingWithLineEndings -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/12/31 00:36:43 -->
<!-- Author: demian -->
# Line Endings

The following script run against the repo will reinstate unix EOL which are required for patches (especially those created in Windows with TortoiseSVN) to apply correctly:



    find . -name "*.php" -type f -exec dos2unix {} \;

<!-- Name: TipsAndTricks/DeleteOldSessionsAndCache -->
<!-- Version: 2 -->
<!-- Last-Modified: 2007/03/30 09:21:26 -->
<!-- Author: werner -->
## Delete old Session Files
I run this oneliner inside the dir where all my webroots are:


    find  -path './*var/tmp/sess_*'  -mtime +1 -exec rm -f {} \;

This deletes all session files that were changed longer than 24 hours ago.

Can easily be adapted for deleting cache files.

Note: I run this script from my /html/ dir, where all my Seagull installations are in. For this the _-path './*var/..._ parameter.
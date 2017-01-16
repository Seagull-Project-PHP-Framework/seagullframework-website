<!-- Name: TipsAndTricks/UpdatingSeagull -->
<!-- Version: 5 -->
<!-- Last-Modified: 2006/06/18 23:51:25 -->
<!-- Author: thomas -->
# How to Rebuild DB_DataObjects Without Logging In


    [aJ] is there an easy way to just rebuild the DB_Objects without reinstalling everything
    demianturner well just set authentication=0
    demianturner then type the URL which is effectively a cmd
    demianturner http://localhost/seagull/branches/0.4-bugfix/www/index.php/maintenance/action/rebuildSequences/
    
    in version 0.6:
    http://localhost/seagull/branches/0.6-bugfix/www/index.php/default/maintenance/action/rebuildSequences/
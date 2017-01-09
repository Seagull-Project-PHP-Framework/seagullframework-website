<!-- Name: Modules/EmailQueue -->
<!-- Version: 1 -->
<!-- Last-Modified: 2008/05/21 16:10:25 -->
<!-- Author: demian -->
= Email Queue Module = 
[[TOC]]
## Overview
This module allows you to store emails in the backend of your choice, and control the rate at which they are sent.  By default a DB backend driver is included.

## Examples
How to add to the queue
 * simply install the emailqueue module
 * using SGL_Emailer2::send, specify queue params in optional 3rd arg, ie:


    ...
    // delivery options
    $aDeliveryOpts['toEmail']  = 'your data here';
    $aDeliveryOpts['toRealName'] = 'your data here';
    ...
    $aDeliveryOpts['fromEmail'] = 'your data here';.
    $aDeliveryOpts['fromRealName']  = 'your data here';
    
    // template vars
    $aTplOpts['var1'] = 'val1';
    $aTplOpts['var2'] = 'val2';
    $aTplOpts['var3'] = 'val3';
    
    // obligatory template options
    $aTplOpts['moduleName']    = 'mymodule';
    $aTplOpts['htmlTemplate']  = 'mytemplate.html';
    $aTplOpts['textTemplate'] = 'mytemplate.txt';
    
    // optionally send emails to queue for later sending
    $aQueueOpts['sendDelay'] = 'your send delay';.
    $aQueueOpts['groupId']  = 'your group';
    $aQueueOpts['userId']  = 'your user ID';
    $aQueueOpts['batchId']  = 'your batch ID';
    
    $ok = SGL_Emailer2::send($aDeliveryOpts, $aTplOpts, $aQueueOpts);



How to send from the queue
 * setup a cronjob that sends periodically, ie daily


    php www/index.php --moduleName=emailqueue --managerName=emailqueue --action=process --deliveryDate=2008-04-08
    php www/index.php --moduleName=emailqueue --managerName=emailqueue --action=process --deliveryDate=all
    php www/index.php --moduleName=emailqueue --managerName=emailqueue --action=flush

The above params all have the same effect.  To get the full list of options type the following at the commandline:


    php www/index.php --moduleName=emailqueue --managerName=emailqueue


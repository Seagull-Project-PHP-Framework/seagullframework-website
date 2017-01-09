<!-- Name: Installation/UsingThePearPackageManager/InstallLog -->
<!-- Version: 2 -->
<!-- Last-Modified: 2007/06/11 15:20:55 -->
<!-- Author: demian -->
# Install Log
The PEAR dependency solving typically looks like this:



    demian-turners-computer:~ root# pear install phpkitchen/seagull
    WARNING: "pear/DB" is deprecated in favor of "pear/MDB2"
    WARNING: "pear/HTML_Common" is deprecated in favor of "pear/HTML_Common2"
    WARNING: "pear/HTML_QuickForm" is deprecated in favor of "pear/HTML_QuickForm2"
    Did not download dependencies: pear/Validate, pear/Date, use --alldeps or --onlyreqdeps to download automatically
    WARNING: "pear/HTML_Common" is deprecated in favor of "pear/HTML_Common2"
    WARNING: "pear/HTML_QuickForm" is deprecated in favor of "pear/HTML_QuickForm2"
    Did not download dependencies: pear/HTML_Javascript, pear/File_Gettext, pear/Translation2, use --alldeps or --onlyreqdeps to download automatically
    WARNING: "pear/DB" is deprecated in favor of "pear/MDB2"
    Did not download dependencies: pear/Cache_Lite, pear/DB_DataObject, pear/MDB, pear/File_Gettext, pear/I18Nv2, use --alldeps or --onlyreqdeps to download automatically
    Did not download optional dependencies: pear/Date, use --alldeps to download automatically
    pear/HTML_Template_Flexy can optionally use package "pear/HTML_Javascript" (version >= 1.1.0)
    pear/HTML_Template_Flexy can optionally use package "pear/File_Gettext" (version >= 0.2.0)
    pear/Translation2 can optionally use package "pear/MDB"
    pear/Translation2 can optionally use package "pear/File_Gettext"
    pear/Translation2 can optionally use package "pear/I18Nv2" (version >= 0.9.1)
    downloading Seagull-0.5.5.tgz ...
    Starting to download Seagull-0.5.5.tgz (747,240 bytes)
    ...................................................................................................................done: 747,240 bytes
    downloading Cache_Lite-1.7.2.tgz ...
    Starting to download Cache_Lite-1.7.2.tgz (29,055 bytes)
    ...done: 29,055 bytes
    downloading Config-1.10.10.tgz ...
    Starting to download Config-1.10.10.tgz (27,577 bytes)
    ...done: 27,577 bytes
    downloading DB_DataObject-1.8.5.tgz ...
    Starting to download DB_DataObject-1.8.5.tgz (61,135 bytes)
    ...done: 61,135 bytes
    downloading DB_NestedSet-1.3.6.tgz ...
    Starting to download DB_NestedSet-1.3.6.tgz (48,291 bytes)
    ...done: 48,291 bytes
    downloading Date-1.4.7.tgz ...
    Starting to download Date-1.4.7.tgz (55,754 bytes)
    ...done: 55,754 bytes
    downloading File-1.3.0.tgz ...
    Starting to download File-1.3.0.tgz (25,727 bytes)
    ...done: 25,727 bytes
    downloading HTML_Common-1.2.4.tgz ...
    Starting to download HTML_Common-1.2.4.tgz (4,519 bytes)
    ...done: 4,519 bytes
    downloading HTML_QuickForm-3.2.9.tgz ...
    Starting to download HTML_QuickForm-3.2.9.tgz (101,392 bytes)
    ...done: 101,392 bytes
    downloading HTML_QuickForm_Controller-1.0.8.tgz ...
    Starting to download HTML_QuickForm_Controller-1.0.8.tgz (17,160 bytes)
    ...done: 17,160 bytes
    downloading HTML_Template_Flexy-1.2.5.tgz ...
    Starting to download HTML_Template_Flexy-1.2.5.tgz (122,089 bytes)
    ...done: 122,089 bytes
    downloading Log-1.9.11.tgz ...
    Starting to download Log-1.9.11.tgz (38,479 bytes)
    ...done: 38,479 bytes
    downloading Mail_Mime-1.5.0RC2.tgz ...
    Starting to download Mail_Mime-1.5.0RC2.tgz (22,807 bytes)
    ...done: 22,807 bytes
    downloading Net_Socket-1.0.8.tgz ...
    Starting to download Net_Socket-1.0.8.tgz (5,441 bytes)
    ...done: 5,441 bytes
    downloading Net_UserAgent_Detect-2.3.0.tgz ...
    Starting to download Net_UserAgent_Detect-2.3.0.tgz (10,178 bytes)
    ...done: 10,178 bytes
    downloading Pager-2.4.3.tgz ...
    Starting to download Pager-2.4.3.tgz (32,268 bytes)
    ...done: 32,268 bytes
    downloading Text_Password-1.1.0.tgz ...
    Starting to download Text_Password-1.1.0.tgz (4,341 bytes)
    ...done: 4,341 bytes
    downloading Translation2-2.0.0beta12.tgz ...
    Starting to download Translation2-2.0.0beta12.tgz (54,213 bytes)
    ...done: 54,213 bytes
    downloading Validate-0.7.0.tgz ...
    Starting to download Validate-0.7.0.tgz (16,841 bytes)
    ...done: 16,841 bytes
    downloading Seagull_default-1.0.tgz ...
    Starting to download Seagull_default-1.0.tgz (91,501 bytes)
    ...done: 91,501 bytes
    downloading Seagull_navigation-1.0.tgz ...
    Starting to download Seagull_navigation-1.0.tgz (102,468 bytes)
    ...done: 102,468 bytes
    downloading Seagull_user-1.0.tgz ...
    Starting to download Seagull_user-1.0.tgz (106,755 bytes)
    ...done: 106,755 bytes
    downloading Mail_mimeDecode-1.0.0RC1.tgz ...
    Starting to download Mail_mimeDecode-1.0.0RC1.tgz (8,320 bytes)
    ...done: 8,320 bytes
    install ok: channel://pear.php.net/Cache_Lite-1.7.2
    install ok: channel://pear.php.net/Config-1.10.10
    install ok: channel://pear.php.net/DB_NestedSet-1.3.6
    install ok: channel://pear.php.net/Date-1.4.7
    install ok: channel://pear.php.net/File-1.3.0
    install ok: channel://pear.php.net/HTML_Common-1.2.4
    install ok: channel://pear.php.net/HTML_Template_Flexy-1.2.5
    install ok: channel://pear.php.net/Log-1.9.11
    install ok: channel://pear.php.net/Net_Socket-1.0.8
    install ok: channel://pear.php.net/Net_UserAgent_Detect-2.3.0
    install ok: channel://pear.php.net/Pager-2.4.3
    install ok: channel://pear.php.net/Text_Password-1.1.0
    install ok: channel://pear.php.net/Translation2-2.0.0beta12
    install ok: channel://pear.php.net/Validate-0.7.0
    install ok: channel://pear.phpkitchen.com/Seagull_default-1.0
    install ok: channel://pear.phpkitchen.com/Seagull_navigation-1.0
    install ok: channel://pear.phpkitchen.com/Seagull_user-1.0
    install ok: channel://pear.php.net/Mail_mimeDecode-1.0.0RC1
    install ok: channel://pear.php.net/DB_DataObject-1.8.5
    install ok: channel://pear.php.net/HTML_QuickForm-3.2.9
    install ok: channel://pear.php.net/Mail_Mime-1.5.0RC2
    install ok: channel://pear.php.net/HTML_QuickForm_Controller-1.0.8
    install ok: channel://pear.phpkitchen.com/Seagull-0.5.5
    

<!-- Name: Modules/Export -->
<!-- Version: 2 -->
<!-- Last-Modified: 2007/04/24 17:58:20 -->
<!-- Author: demian -->
# Export Module
[[TOC]]
This module transform content/data into various formats, xml, PDF, RSS, etc

## Export2Mgr
### Overview
 * exports any CMS or Data Access Object content as a valid RSS2 feed
 * easy to configure

### Minimum requirements
 * Seagull 0.6.2
 * CMS 1.1
 * PHP5

### Exporting CMS content
 * to get the latest content of type FOO (content type ID = 5) add following line to modules/export/conf.ini


    [/datasrc/cms/contenttype/5]
    title = name
    description = text
 * calling URL 


    http://localhost/seagull/www/index.php/export/rss2/datasrc/cms/contenttype/5


### Exporting DAO content - eg1
 * to get an RSS feed of all users added to the system add following line to modules/user/conf.ini


    [/datasrc/dao/module/user/method/getUsers] ; you must create this method
    title = first_name
    description = usr_id, telephone, address_1, address_2
 * calling URL 


    http://localhost/seagull/www/index.php/export/rss2/datasrc/dao/module/user/method/getUsers

### Exporting DAO content - eg2
 * to get an RSS feed of the latest image media added to the system add following line to modules/media/conf.ini


    [/datasrc/dao/module/media/method/getMediaByFileType/5] ; 5 is the image type
    title = name
    description = mime_type
    createdBy = media_added_by
 * calling URL 


    http://localhost/seagull/www/index.php/export/rss2/datasrc/dao/module/media/method/getMediaByFileType/filetype/5

### Constraints
 * you can have as many args to dynamic methods as you like, or none
 * args must be scalar
 * in the calling URL the convention is for key1/value1/key2/value2
 * in the config file the pattern only requires the value ie, value1/value2

### Coming soon
 * ability to specify multiple fields for 'description' in config that will be merged for RSS output

 

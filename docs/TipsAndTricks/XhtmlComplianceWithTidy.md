<!-- Name: TipsAndTricks/XhtmlComplianceWithTidy -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/02/07 21:41:23 -->
<!-- Author: demian -->
= XHTML Compliance with Tidy = 

While cleaning up the pages to conform to XHTML standards, I thought it'd be easier to use the [tidy extension](http://pecl.php.net/package/tidy) from PECL.  The following code could prove useful for anyone wanting to clean up their Flexy templates:


        //  tidy.php
        //  commandline usage:  php tidy.php myFlexyTemplate.html
        $file = $_SERVER['argv'][1];
    
        tidy_setopt('wrap', 0);
        tidy_setopt('indent', 2);
        tidy_setopt('indent-spaces', 4);
        tidy_setopt('output-xhtml', true);
        tidy_setopt('show-body-only', true);
        tidy_setopt('new-empty-tags', 'flexy:include');
    
        $html = tidy_parse_file($file);
        tidy_clean_repair();
        $res = tidy_get_output();
        echo $res;
    
        //  rename original file
        $oldName = $file;
        $newName = $file. '.orig';
        copy($oldName, $newName);
        unlink($oldName);
    
        file_put_contents("./$oldName", $res);
    
        function file_put_contents($location, $whattowrite) 
        {
           if (file_exists($location] {
               unlink($location);
           }
           $fileHandler = fopen ($location, "w");
           fwrite ($fileHandler, $whattowrite);
           fclose ($fileHandler);
        }
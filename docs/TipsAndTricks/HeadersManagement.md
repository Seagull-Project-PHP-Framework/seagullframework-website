<!-- Name: TipsAndTricks/HeadersManagement -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/04/02 03:53:53 -->
<!-- Author: demian -->
# HTTP headers management
## How to output images/pdf/whatever bypassing Flexy headers management

Sometimes a programmer needs to manage the HTTP headers directly, bypassing the Flexy engine.
This happens, for example, when we have images stored in a database and we need to output them directly to the browser answering to a HTML tag like:


    <img src="imageRetriever.php?action=getImage&id=1">

or, as Seagull prefers:


    <img src="http://mysite/index.php/myModule/imageRetriever/getImage/id/1">

HTTP headers can be disabled adding in the conf.ini file:


    [ImageRetrieverMgr]
    setHeaders=false

Then create a template called masterEmpty.html like:


    {outputBody()}

and use it as a master template in your class.
Now you can create your own method to output the image:


    <?php
    function _getImage(&$input, &$output)
    {
        $record = getRecord($recId);
        $input->masterTemplate = 'masterEmpty.html';
        header('Content-type: image/jpeg');
        header('Content-disposition: inline; filename: ' . $record->name);
        header('Expires: Mon, 26 Jul 1997 05:00:00 GMT');
        header('Cache-Control: no-store, no-cache, must-revalidate');
        header('Cache-Control: post-check=0, pre-check=0', false);
        header('Pragma: no-cache');
        echo $record->image;
    }
    ?>

Pay attention to $record->image, it must contain binary data to build the right image in the browser.

With PostgreSQL, images can be stored inside a table in a bytea field using the PHP functions: pg_encode_bytea($myImageData) and pg_unencode_bytea($myRecordField)

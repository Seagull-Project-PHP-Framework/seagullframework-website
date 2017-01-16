<!-- Name: Integration/PDF/DOMPDF -->
<!-- Version: 5 -->
<!-- Last-Modified: 2008/12/01 18:29:09 -->
<!-- Author: malber -->
# Generating PDFs with DOMPDF

Here is my steps of usage DOMPDF(http://www.digitaljunkies.ca/dompdf/) with 
Seagull, for rendering HTML articles (publisher):

 * make dir: lib/other/Dompdf

 * Copy files from dompdf to Dompdf:


    lib/other/Dompdf/include
    lib/other/Dompdf/dompdf_config.inc.php
    lib/other/Dompdf/lib

 * Configure Seagull to use "lib/other" as additional include path:

General -> Configuration -> Additional include paths

 * Sample. Modified FileMgr from publisher/classes

 * Required includes (before class declaration):


    require_once SGL_CORE_DIR . '/Item.php';
    require_once 'Dompdf/dompdf_config.inc.php';

 * Modify "validate" function (just place to the end of function):


    $input->articleID = (int)$req->get('frmArticleID');


 *  Add new method:


       function _cmd_downloadPdf(&$input, &$output)
       {
           SGL::logMessage(null, PEAR_LOG_DEBUG);
    
           $aArticleDetail = SGL_Item::getItemDetail($input->articleID);
           $dompdf = new DOMPDF();
           $dompdf->load_html($aArticleDetail['bodyHtml']);
           $dompdf->render();
           $dompdf->stream($aArticleDetail['title'].".pdf");
           exit;
       }
    

 * Use {makeUrl()} with your templates, for example:


    {makeUrl(#downloadPdf#,#file#,#publisher#,aPagedData[data],#frmArticleID|item_id#,key)}


Or just type in address bar of your web browser(quick test):

http://your_sgl_website/index.php/publisher/file/action/downloadPdf/frmArticleID/1/

This way can be easy adopted for usage with cms or any other module, but in summary it's yet another way to handle "html 2 pdf" request.

## Autoload Issue
Fatal error: require_once() [function.require]: Failed opening required '/trunk/lib/other/Dompdf/include/html_template_flexy_token_doctype.cls.php'

Solution - Add a check for the include file in dompdf_config.inc.php, change from:

    function DOMPDF_autoload($class) {
      $filename = mb_strtolower($class) . ".cls.php";
      require_once(DOMPDF_INC_DIR . "/$filename");
    }
*to*

    function DOMPDF_autoload($class) {
      $filename = mb_strtolower($class) . ".cls.php";
      if(file_exists(DOMPDF_INC_DIR . "/$filename")) {
          require_once(DOMPDF_INC_DIR . "/$filename");
      }    
    }
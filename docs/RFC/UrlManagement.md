<!-- Name: RFC/UrlManagement -->
<!-- Version: 6 -->
<!-- Last-Modified: 2006/01/11 18:49:05 -->
<!-- Author: demian -->
# RFC for improved URL creating

http://trac.seagullproject.org/wiki/RFC/Modules/UriHandling

ie, how to make life easier than the current makeLink() template function:



    //  building SGL URLs
        $url = new SGL_Url();
        
        $url->setModule('publisher');
        $url->setManager('articleview');
        $url->setAction('list');
        $url->addQueryString('frmArticleId', 23);
        $output = $url->toString(SGL_URL_ABS);
        
    //  for Flexy output:
    makeLink(#self/publisher/articleview/action/view/frmArticleID/item_id#, aPagedData[data])
    
    //  https
    makeLink(#self/publisher/articleview/action/view/frmArticleID/item_id#,aPagedData[data],#https#)
    -------------------------------------------------^^^^^^^^^^^^^ var name
    --------------------------------------------------------------^^^^^^^^ obj prop/array key
    -----------------------------------------------------------------------^^^^^^^^^^^^^^^^ collection
    ----------------------------------------------------------------------------------------^^^^^^^ is https or not
    
    
    //  working with SGL_Url type, switching FC/traditional implementation at runtime
    $url = new SGL_Url($url, $useBrackets = true, new SefUrlStrategy()); // as in Search Engine Friendly
    

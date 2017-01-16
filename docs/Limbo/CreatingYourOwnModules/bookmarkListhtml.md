<!-- Name: Limbo/CreatingYourOwnModules/bookmarkListhtml -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/04/16 18:23:36 -->
<!-- Author: demian -->
FIXME: Look over new file... 


    #!text/html
    <h1 class="pageTitle">{translate(pageTitle)}</h1>
    <span flexy:if="msgGet()">{msgGet()}</span>
    <table class="wide">
    {if:results}
    
        {foreach:results,key,valueObj}
    
        <tr>
            <td><a href="bookmark.php?action=redirect&frmBookmarkId={valueObj.bookmark_id}" target="_blank">{valueObj.name}</a></td>
            <td align="right">{formatDate(valueObj.last_updated)}</td>
        </tr>
    
        <tr>
            <td colspan="2">{valueObj.text:h}</td>
        </tr>
    
        <tr>
            <td>{translate(#Clicked#)} {valueObj.count} {translate(#times#)}</td>
            <td colspan="1" align="right"><a href="#">{translate(#back to top#)}</a></td>
        </tr>
        <tr>
            <td colspan="2">&nbsp;</td>
        </tr>
        {end:}
        {else:}
        <tr>
            <td>{translate(#No Links found#)}.</td>
        </tr>
        {end:}
    </table>

As you can see, all output is translated in the templates. For variables (e.g. $output->pageTitle) use

    #!text/html
    {translate(pageTitle)}

for any other text use something like

    #!text/html
    {translate(#your text here#)}

The translations are stored in _modules/bookmark/lang_/
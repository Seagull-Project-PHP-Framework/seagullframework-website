<!-- Name: Integration/AJAX -->
<!-- Version: 2 -->
<!-- Last-Modified: 2008/04/28 15:33:53 -->
<!-- Author: demian -->
# Integrating various Ajax Frameworks
[[TOC]]
Seagull is easy to integrate with Ajax projects because its Request object is factorised to handle the basic request types:
 * browser
 * Ajax
 * CLI
 * AMF

Drivers for SOAP and XML-RPC requests could easily be developed but do not currently exist.

In the case of Prototype and jQuery, any Ajax request only needs to specify the module name and the action name, and the relevant method in the module's AjaxProvider will be discovered.  Here's an example in Prototype


    function saveSection()
    {
    	new Ajax.Request(
    		'{makeUrl(#addGroup#,##,#todo#)}',
    		{asynchronous:true,
    		 method:'post',
    		 parameters:'groupName=' + $('sectionName').value,
    		 onSuccess:handlerFunc});
    }

which will map to TodoAjaxProvider::addGroup which looks like this:


        function addGroup()
        {
            $groupName = SGL_Request::singleton()->get('groupName');
    
            $nextId = $this->dbh->nextId($this->conf['table']['todo_group']);
            $todoGrp = DB_DataObject::factory($this->conf['table']['todo_group']);
            $todoGrp->todo_group_id = $nextId;
            $todoGrp->name = $groupName;
            $todoGrp->order_id = $nextId;
            $success = $todoGrp->insert();
            if ($success) {
                $ret = 'Group added successfully';
            } else {
                $ret = 'There was a problem adding the group';
            }
            return $ret;
        }





## Prototype + Scriptaculous
These frameworks are integrated by default in the 0.6 series of Seagull, see the [Ajax tutorial](/wiki:Tutorials/BuildingAnAjaxEnabledModule/) for more detail.

## jQuery
jQuery is currently the dominant player in Ajax circles and is very easy to integrate into Seagull.  You could either

 * override SGL_Output::makeJsOptimizerLink() so it includes the packed version of jquery, adding to the file array:


    $aFiles = array(
        // jquery core
        'themes/myTheme/js/JQuery/jquery-1.2.3.pack.js',
        // ...
 * or you could add it manually to the global config under 


    $conf['site']['globalJavascriptFiles'] = "js/JQuery/jquery-1.2.3.pack.js"
 * or you could use your current Manager class and add it directly to the display() method


    $output->addJavascriptFile('themes/myTheme/js/JQuery/jquery-1.2.3.pack.js');
 

## Dojo
Dojo is quite heavyweight but also an interesting option, see [here](/wiki:Integration/AJAX/dojo/) for more integration details.

## Related links
 * [wiki:Howto/AJAX]
<!-- Name: Howto/Templates/IntegrateWYSIWYG -->
<!-- Version: 13 -->
<!-- Last-Modified: 2009/09/14 00:45:04 -->
<!-- Author: lyric -->
# Using a textarea WYSIWIG editor

## Integrating FCKeditor

### General header Template
Add the following javascript code to the header.html or
admin_header.html template (if it is not already there):


    {if:wysiwyg_fck}
        <script type="text/javascript"
    src="{webRoot}/wysiwyg/fckeditor/fckeditor.js"></script>
        <script type="text/javascript">
            var oFCKEditors = new Array;
    
            /* initalises an instance of FCK and returns the object. */
            function fck_add(id)
            {
                i = oFCKEditors.length;
                oFCKEditors[i] = new FCKeditor(id, '100%', 300);
                oFCKEditors[i].ToolbarSet = 'Basic' ;
                oFCKEditors[i].BasePath = SGL_JS_WEBROOT +
    "/wysiwyg/fckeditor/";
                oFCKEditors[i].Config["CustomConfigurationsPath"] =
    SGL_JS_WEBROOT + "/js/SglFckconfig.js"  ;
                oFCKEditors[i].ReplaceTextarea();
            }
            function fck_init()
            {
                if( document.getElementsByTagName ) {
                    areas = document.getElementsByTagName('textarea');
    
                    for( var i=0; i<areas.length; i++ ){
                        if( areas[i].className.match("wysiwyg") ) {
                            fck_add(areas[i].id);
                        }
                        else if( areas[i].id.match('frmBodyName') ) {
                            /* fallback for old templates */
                            fck_add('frmBodyName');
                        }
                   }
                }
            }
        </script>
        {end:}

### Module's Templates
All textareas with class="wysiwyg" will be replaced by the editor. This enables you to have multiple editor per page :

    <textarea  name="frmBodyName" id="frmBodyName" class="wysiwyg">{event.text}</textarea>

### Module's Mgr Class action
In your class method where you want to use the WYSIWYG editor, you just have to tell Seagull you want to use it.

    // enable hmtl wysiwyg editor
    $output->wysiwyg = true;

This includes the needed JavaScript to your output.


### Module's Mgr Class validation
Wondering why no HTML is passing the validation process? For security reasons Seagull strips all HTML automatically. But there is a way to get around this: use the `$allowTags = true` attribute in `$req->get`.

    $input->event = (object)$req->get('frmBodyName', $allowTags = true);



### Configuration
Use www/js/SglFckconfig.js to customize the look & feel of the editor. For more information about all the possible settings see: http://docs.fckeditor.net/FCKeditor_2.x/Developers_Guide/Configuration/Configuration_Options


[[AddComment]]
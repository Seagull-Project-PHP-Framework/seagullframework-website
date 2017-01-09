<!-- Name: Howto/JavaScript/IncludeFiles -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/01/11 19:20:15 -->
<!-- Author: jcasanova -->
# Adding JavaScript Files

### 0.6.2
Since Seagull 0.6.2 you can include any javascript file by calling $output->addJavascriptFile($file) in you manager.

This method takes a single param that can be either a single string or an array of files to load, relative to the SGL_WEB_ROOT, i.e. the www folder in your Seagull install:

        $output->addJavascriptFile('js/global.js');

or


        $output->addJavascriptFile(array(
            'relative/path/to/js/file.js',
            'http://www.example.com/js/file.js
        ));
are both valid. As you can notice, you may also require remote javascript files by providing an absolute path.

Also you now have the possibility to require a file that will be loaded in every page of your site.

Simply add it to the main configuration file :

    $conf['site']['globalJavascriptFiles'] = 'js/file';

Again you can provide several files, separating them with a semicolon ";"


    $conf['site']['globalJavascriptFiles'] = 'js/file1.js;js/file2.js';

and require remote files by providing an absolute path.


    $conf['site']['globalJavascriptFiles'] = 'http://www.google-analytics.com/urchin.js';

### Before 0.6.2
In order for your manager to load custom JavaScript files just build an array of 1 or more js filenames and add it to the javascriptSrc property of the output object.  Use a path starting from the webroot.


        $output->javascriptSrc = array('js/PageView.js','js/packaging.js');

This will add the files _js/PageView.js_ and _js/packaging.js_ to the <head> element of your html file when the manager is loaded.

## See Also
[[SubWiki(Howto/JavaScript)]]

[[AddComment]]
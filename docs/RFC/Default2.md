<!-- Name: RFC/Default2 -->
<!-- Version: 9 -->
<!-- Last-Modified: 2007/03/27 14:50:21 -->
<!-- Author: lakiboy -->
# Default2 theme [status: ongoing]

## Loading stylesheets
Style.php will be responsible of loading several stylesheets, in the following order :
 - some defaults stylesheets (reset.css, tools.css, typo.css, forms.css, layout.css, blocks.css ...)
 - a specific layout stylesheet, e.g. layout-navtop-3col.css
 - module/manager/action stylesheets (from <module>/www/css dir first, then overriding with <theme>/css dir)
 - custom stylesheets added through $output->addCssFile('any-stylesheet.css')

Style.php also loads vars.php and helpers.php

The request for stylesheets is made within the header calling :


    <link rel="stylesheet" type="text/css" media="screen" href="{makeCssLink(theme,conf[navigation][stylesheet],moduleName)}" />

And SGL_Output::makeCssLink() will construct the appropriate uri to access style.php and pass it all requested args through GET to have something like:


    <link rel="stylesheet" type="text/css" media="screen" href="http://localhost/seagull/www/themes/default/css/style.php?layout=layout-navtop-3col.css&amp;moduleName=default" />

~~I guess we can benefit from a single style.php file located under www/themes and let it require appropriate files depending on current theme.~~
*style.php has been moved to www/themes dir.*
### Performance and cache
*Caching*

We currently have some default stylesheets such as previously mentionned reset.css, typo.css, forms.css ... Prepending these files with "main-" (convention) could allow to automatically load these files without adding any configuration.

To benefit from caching I guess we should split this request into at least 2 requests:
 - a first request to load all default files, i.e. those which will not change depending on the page requested (we can use the browser cache straight for this request)
 - a second request to load module/manager/action stylesheets. We could then merge all these files into a single one, and use a simplistic file based cache system to serve them to the browser, then benefit from browser cache.

Following this idea, we could then have a third request for all custom stylesheets, i.e. thoses added through SGL_Output::addCssFile.

*Compression*

Using a gzipped output buffer, we can size down the request to 25% of original size.

### SGL_Output::addCssFile API
_Some crazy ideas_

        /**
         * 1. add css file from current theme to process by css loader:
         *      $output->addCssFile('style.css');
         *      - looked in SGL_BASE_URL/themes/currentTheme/css/styles.css,
         *
         * 2. add css file from another theme to process by css loader:
         *      $output->addCssFile('style.css', 'anotherTheme'),
         *      - looked in SGL_BASE_URL/themes/anotherTheme/css/style.css,
         *
         * 3. add css file from current/another theme, but skip css
         *    loader i.e. load "as is":
         *      $output->addCssFile('style.css', $skipLoader = true);
         *      $output->addCssFile('style.css', 'anotherTheme', $skipLoader = true);
         *
         * 4. add css file from directory relative to www dir:
         *      $output->addCssFile('custom-dir/css/styles.css');
         *      $output->addCssFile('custom-dir/css/styles.css', $skipLoader = true);
         *      - looked in SGL_BASE_URL/custom-dir/css/styles.css,
         *
         * 5. add css file relative to specified module:
         *      $output->addCssFile('todo', 'additional.css');
         *      $output->addCssFile('todo', 'additional.css', $skipLoader = true);
         *      - looked first in SGL_BASE_URL/themes/currentTheme/css/todo_additional.css,
         *      - if not found above, then looked in SGL_BASE_URL/todo/css/additional.css,
         *
         * 6. add css file from another host:
         *      $output->addCssFile('http://another-host/css/styles.css');
         *      - always loaded as is.
         *
         * 7. modify main styles:
         *      $output->addCssFile('main', 'styles.css'); // added to default list
         *      $output->addCssFile('main', // replace or delete example
         *          array(
         *              'reset' => '', // remove reset from default list
         *              'typo'  => 'print_typo.css' // replace with another stylesheet
         *          )
         *      );
         *      $output->addCssFile('main', 'reset.css', 'delete');
         *      $output->addCssFile('main', 'reset', 'delete');
         *
         *    main (loaded by default) stylesheets are:
         *    - reset.css,
         *    - tools.css,
         *    - typo.css,
         *    - forms.css,
         *    - layout.css,
         *    - blocks.css,
         *    - common.css.
         *
         * @param  mixed $fileName  string or array look above for possible values
         * @param  mixed
         * @param  mixed
         * @return void
         */
### Example of header.html

        <!-- 1st request - main files: reset.css, typo.css -->
        <link rel="stylesheet" type="text/css" media="screen" href="{makeCssLink(theme,#main#)}" />
    
        <!-- 2nd request - layout, module specific files:
               - layout-navtop-3col.css,
               - moduleName.css or moduleName.php,
               - moduleName_styleName.css -->
        <link rel="stylesheet" type="text/css" media="screen" href="{makeCssLink(theme,moduleName)}" />
    
        <!-- 3rd request - other files:
               - all files relative to www dir,
               - files from other themes -->
        <link rel="stylesheet" type="text/css" media="screen" href="{makeCssLink(#other#)}" />
    
        {if:aCssFiles[host]}
        <!-- Files from another host or skipped by loader -->
        <style type="text/css">
        {foreach:aCssFiles[host],fileName}
            @import url({fileName});
        {end:}
        </style>
        {end:}

_Probably too complicated_

## CSS / XHTML coding standards
Starting from _0.7_ Seagull enforces _CSS_/_XHTML_ coding standards. However, the following rules are trial and might be changed in future versions of framework.

### Doctype
All _HTML_ documents must be _XHTML 1.x Strict_ compliant.

### Selectors naming convention
Let's examine the following example:

    ...
    <div id="ajaxMessage">
        <p>Some text here</p>
    </div>
    ...
    <div id="footer">
        <div class="wrap-left">
            <div class="wrap-right>
                <p>Some text here</p>
            </div>
        </div>
    </div>

If there are class/ID names which represent a group of classes, then they should be given a common group name. In above example there are two rules: _wrap-left_ and _wrap-right_. We can logically combine mentioned rules in a group giving it a common name - _wrap_. Next we extend every class name with _-extension_, describing selectors' actual meaning, i.e. _wrap-left_ and _wrap-right_.

A rule, which describes a single element and is complete in it's meaning and is not a member of any other class group, is given a name in camel case format. In above example there is a _ajaxMessage_ class, which is standalone and is given a name in camel case.

### Selectors indent
Example:

    #footer {
    }
        #footer p {
        }
            #footer p a {
            }
        #footer ul {
        }
            #footer ul li {
            }
Blanks are stripped on dev machines to reduce file size.

<!-- Name: Howto/AJAX/AjaxProvider -->
<!-- Version: 4 -->
<!-- Last-Modified: 2007/05/26 10:00:56 -->
<!-- Author: demian -->
# AJAX Provider
[[TOC]]

 * *Note: requires >= Seagull 0.6.2*

When you want to add interactivity within your web applications and make your pages more responsive it is a good idea to separate your operations into several Ajax methods and have them exchange small units of data with the server. When an Ajax request is detected (see [wiki:Howto/AJAX/AjaxRequests]) the appropriate AjaxProvider is loaded and the requested method executed.

An AjaxProvider is a class where you will store all your Ajax methods and could be compared to a SGL_Manager. As mentioned previously the aim of these Ajax methods is to make your applications more responsive. That's why the AjaxProviders are run in a lighter environment and no default view is built.

## Creating a provider

AjaxProviders follow a simple convention. The class name must be *moduleName*AjaxProvider and this class must be created within a file called *moduleName*AjaxProvider.php in your module/*moduleName*/classes directory.

For a foo module create a file called FooAjaxProvider.php, and copy the following code:


    require_once SGL_CORE_DIR . '/AjaxProvider.php';
    
    class FooAjaxProvider extends SGL_AjaxProvider
    {
        /**
         * Constructor
         */
        function FooAjaxProvider()
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            parent::SGL_AjaxProvider();
        }
    }

Your FooAjaxProvider must extend the SGL_AjaxProvider class. This parent class will provide default access to the global configuration array (SGL_Provider::conf) and the DB abstraction layer (SGL_Provider::dbh) (see [browser:/branches/0.6-bugfix/lib/SGL/AjaxProvider.php SGL_AjaxProvider.php]) as long as you call its constructor within your FooAjaxProvider constructor.

If you want to define a responseFormat for the whole provider simply override its property value within the constructor.


    class FooAjaxProvider extends SGL_AjaxProvider
    {
        /**
         * Constructor
         */
        function FooAjaxProvider()
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            parent::SGL_AjaxProvider();
    
            $this->responseFormat = SGL_RESPONSEFORMAT_JSON;
        }
    }

## Adding methods to the provider

Simply add a function to your AjaxProvider.


        ...
        function fooBar()
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
    
            //  code here
            return $ret;
        }

If your method should return anything, you must explicitly return it. This is the response that will be automatically processed by the SGL_AjaxProvider::processResponse().

You may want to set a specific responseFormat in your method, in this case override the class property.


        ...
        function fooBar()
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
    
            //  code here
            $this->responseFormat = SGL_RESPONSEFORMAT_JSON;
            return $ret;
        }

## Going further

To work with DataAccessObjects in your providers, you can add them directly in your constructor.


    require_once SGL_CORE_DIR . '/AjaxProvider.php';
    require_once SGL_MOD_DIR . '/foo/classes/FooDAO.php';
    
    class FooAjaxProvider extends SGL_AjaxProvider
    {
        /**
         * Constructor
         */
        function FooAjaxProvider()
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            parent::SGL_AjaxProvider();
    
            $this->da = & FooDAO::singleton();
            $this->responseFormat = SGL_RESPONSEFORMAT_JSON;
        }
    }

If you need to retrieve some parameter values, simply get a reference to the SGL_Request instance.
Suppose we sent a param1=value1 parameter through GET.

        ...
        function fooBar()
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
    
            $param1 = SGL_Request::singleton()->get('param1');
            // => 'value1'
    
            //  code here
            $this->responseFormat = SGL_RESPONSEFORMAT_JSON;
            return $ret;
        }


<!-- Name: Howto/Templates/Flexy/Plugins -->
<!-- Version: 10 -->
<!-- Last-Modified: 2006/11/30 15:49:10 -->
<!-- Author: demian -->
# Working With Plugins In Flexy
## QuickStart

Let's make a simple plugin for our template in Seagull:

1) Create a file called *Output.php* in out *module/classes* directory.

2) In this file we create a PHP class called ModuleOutput and a method, for example, to generate a drop down list with data fetched from a DB:


    class ModuleOutput {
        function selectMyData($default)
        {
            $aList = getMyDataFromDatabase();  // fetch a list of data from my DB
            $sel = SGL_Output::generateSelect($aList, $default);
            return $sel;
        }
    }
Notice the class name is built appending *Output* to the name of our module *Module*.

3) Save the file and create the template as usual.

4) To use the plugin inside your template just write:

    <form method="post" flexy:ignore>
        <select name="mySelect">
            {this.plugin(#selectMyData#,default):h}
        </select>
    </form>
Please notice how parameters can be passed to the plugin: you must use *#* char as delimiter for plugin name and for constants. If you need to pass the value of a PHP variable, as in the example, don't surround the name of the variable with #.

5) Ok, save your template and run your page. The Seagull framework will look for a file called Output.php in your module directory and will do all the integration job with Flexy engine for you. ;-)

## As a first note
In the compiled template *$t* is the object that keeps all the variables that are available to the template (in our case, what we put in *$output*). So when you write:


    $output->name = 'Foo';

and in the template we put:

    {name}

the compiled code looks like:

    <?php echo htmlspecialchars($t->name); ?>


## Setting up the plugin
Looking to the Pear Flexy files it seems that there is only one plugin distributed with Flexy, called Savant and it's found in:
/lib/pear/HTML/Template/Flexy/Plugin directory.

I want to create a plugin named *Output* that will contain all the methods from ~/modules/publisher/classes/Output.php and use them instead of Custom Template Methods.

You have to create a php file for your plugin, in our case: *Output.php* 

In that file create a class, in our case: *PublisherOutput*

In the class you put the methods that you want to be available, in our case the ones from the original Output.php: 


      articleOutput()
      id2AssetIcon($documentID)
      . . . 
      test() // added this method for testing only''


The test method code is:

    function test($data) {
        echo '<b>Test Method Output</b><br>';
        echo '<pre>'; print_r($data); echo '</pre>';
    }

At this point the plugin is ready to be used.

## Calling the plugin
In the template file, you can call a plugin  in two ways.

*The first way is:*

    {this.plugin(#methodName#,#parameters#)} 

in our case:

    {this.plugin(#test#,#abc#)}

that after compilation will be transformed in:

    <?php if ($this->options['strict'] || (isset($this) && method_exists($this,'plugin']) echo htmlspecialchars($this->plugin("test","abc"];?> 

the part in focus is:

    $this->plugin("test","abc"] 

    
and the output HTML:

    <b>Test Method Output</b><br><pre>abc</pre>

Is important to say that the plugin class (ex: HTML_Template_Flexy_Plugin_Output) does not inherit the *$t* class, so the variables in *$t* are not available so you have to pass it as a parameter to the plugin.

Let's have:

    $output->name = 'Foo';

and:

    {this.plugin(#test#,name)} 

will generate:

    $this->plugin("test",$t->name) 

and the HTML output:

    <b>Test Method Output</b><br><pre>Foo</pre>

OR to have access to the whole class:

    {this.plugin(#test#,t)}

will generate:

    $this->plugin("test",$t)

and on the screen you'll get the whole content of *$t* in out case the content of *$output* plus some extras.


Of course you can call the plugin without parameters:

    {this.plugin(#test#)}

will generate:

    #!php $this->plugin("test")

You can use optional modifiers like:  


      :h - echos without any change - eg. Raw
      :u - echos urlencode($variable)

For example:


    {this.plugin(#test#,name):h} or {this.plugin(#test#,name):u}

*The second way to call the plugins is:*

    {params:methodName}

_This time params must be a variable in the $t class and the output is always echoed without any change (like :h modifier)_

In our case:

    {name:test}

is equivalent with:

    {this.plugin(#test#,name)}

and will generate:  

    <?php echo $this->plugin("test",$t->abc);?>

or the interesting part 

    {t:test}

will generate:

    $this->plugin("test",$t)

In the end we can make the change in ~/www/themes/default/publisher/ArticleView.html from Custom Method call:

    {articleOutput()}

to plugin call:

    {t:articleOutput}

modify a little the articleOutput to receive the  $t argument and use it instead of $this in the code:

        function articleOutput($out)
        {   
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            $templ = new HTML_Template_Flexy();
            $aCurrentType = explode(' ', $out->leadArticle->type);              
            $out->articleTmpl = 'articleView';     
        }  

*and it's working.*


## Related links:
http://pear.php.net/manual/en/package.html.html-template-flexy.php - Pear Flexy Manual (but no Plugin info)

http://pear.php.net/package/HTML_Template_Flexy/docs/0.9.2/apidoc/HTML_Template_Flexy-0.9.2/HTML_Template_Flexy_Plugin.html - the phpDocumentor output of flexy plugin code

http://cvs.php.net/pear/HTML_Template_Flexy/tests/ - some test and examples with Flexy made by the author

[[AddComment]]
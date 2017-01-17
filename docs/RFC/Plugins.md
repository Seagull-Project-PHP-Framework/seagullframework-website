<!-- Name: RFC/Plugins -->
<!-- Version: 5 -->
<!-- Last-Modified: 2008/08/01 15:00:05 -->
<!-- Author: demian -->

# Plugins
* TOC
{:toc}

## Overview
 * a plugin can be any collection of libs, drivers, themes, modules - ie, any seagull component
 * a plugin is distributed as a PEAR package so it knows about dependencies

## Requirements
 * a plugin must be able to add to the include path
 * ability to add tasks to the filter chain
 * autload lib classes
 * output objects loaded for templates
 * modification to global config
 * can be easily extended
 * a plugin must have a version
 * a plugin must handle dependencies (eg, parent tasks)

## Concepts
 * a plugin will be registered to a slot which corresponds to various stages within the framework execution cycle, eg
   * framework init
   * pre request parse
   * pre manager action execution
   * etc
	 
## Scenarios
Part of the design process is to ensure the plugin can satisfy the requirements of various scenarios

### DataGrid
DataGrid comprises SGL libs, templates, js, docs

### Smarty Template engine
Smarty includes the Smarty lib, smarty-specific templates, smarty renderer in lib/SGL

### XML RPC
Comprised of seagull xml lib, rpc server in webdir


	<?php
	/* required plugin functionality
	   =============================
	 * plugins have two states, installed() and enabled()
	 * during enabling process, plugin config can be added to global keys
	 * filter chain tasks can be added in correct slots
	 * plugins can resolve deps, plugin foo needs DB support, plugin bar needs cache support
	 * plugins can easily be enabled and disabled via GUI
	 * modules depend on plugins
	 * test only for optional deps, ie SGL_Captcha, but not mandatory, ie SGL_Config_Yml, which
	   will be enforced during install
	
	   structure
	   =========
	 * plugins registered when they appear in $conf[plugins][$pluginName]
	 * in terms of deps, both modules and plugins are pear packages, categorised accordingly
	 * minimal install will have no modules/plugins installed
	
	
	   differrence between module and plugin
	   =====================================
	 * a module is a program, it provides specific functionality, business logic, ability to
	   to create a store a range of data.
	 * module is a standalone program
	 * examples
	    * cms
	    * ecomm
	
	 * a plugin enhances functionality in a small increment, ie
	    * a filter to modify output, like a photoshop filter, a text processing filter
	    * an additional payment gateway
	 * a plugin can be used with a number of modules
	 * examples
	    * translation
	    * wysiwyg
	
	*/
	
	////////////////////////
	//  example with captcha
	////////////////////////
	
	//  during registration process, plugin config added to global keys
	if (SGL_Plugin::isRegistered('SGL_Captcha') && $this->conf['ArticleMgr']['enableCaptcha']) {
	    //  check days before using Captcha
	    $daysBeforeUsingCaptcha = (int) $this->conf['pluginsCaptcha']['daysBeforeUsingCaptcha'];
	    if ((strtotime($output->leadArticle['startDate']) + ($daysBeforeUsingCaptcha * 86400))
	            <= strtotime(SGL_Date::getTime())) {
	        #require_once 'SGL/Captcha.php';
	        #$captcha = new SGL_Captcha();
	        try {
	            $captcha = SGL_Plugin::load('SGL_Captcha');
	        } catch (Exception $e) {
	            echo 'Caught exception: ', $e->getMessage(), "\n";
	        }
	        $output->captcha = $captcha->generateCaptcha();
	    }
	}
	
	////////////////////////
	//  example with wysiwyg
	////////////////////////
	
	/*
	1. ability to set in client code whether a textarea is wysiwyg or not
	2. ability to add post-process task to load wysiwyg
	3. ability to add global config values, ie, which wysiwyg
	*/
	
	//  1.  set class='wysiwyg'
	?>
	<textarea id="foo" class='wysiwyg'>this is the text</textarea>
	<?php
	
	//  2 + 3.  post process init
	class SGL_Task_SetupWysiwyg extends SGL_DecorateProcess
	{
	    function process(&$input, &$output)
	    {
	        SGL::logMessage(null, PEAR_LOG_DEBUG);
	        $this->processRequest->process($input, $output);
	
	        // set the default WYSIWYG editor
	        if (SGL_Plugin::isRegistered('wysiwgy') && !SGL::runningFromCLI()) {
	
	            // you can preset this var in your code
	            if (!isset($output->wysiwygEditor)) {
	                $output->wysiwygEditor = isset($this->conf['pluginsWysiwyg']['editor'])
	                    ? $this->conf['pluginsWysiwyg']['editor']
	                    : 'fck';
	            }
	            switch ($output->wysiwygEditor) {
	
	            case 'fck':
	                $output->wysiwyg_fck = true;
	                $output->addOnLoadEvent('fck_init()');
	                break;
	            case 'xinha':
	                $output->wysiwyg_xinha = true;
	                $output->addOnLoadEvent('xinha_init()');
	                break;
	            case 'htmlarea':
	                $output->wysiwyg_htmlarea = true;
	                $output->addOnLoadEvent('HTMLArea.init()');
	                break;
	            case 'tinyfck':
	                $output->wysiwyg_tinyfck = true;
	                // note: tinymce doesn't need an onLoad event to initialise
	                break;
	            }
	        }
	    }
	}
	
	/////////////////////////////
	//  example with flesch score
	/////////////////////////////
	
	        //  calculate flesch score if enabled
	        if (SGL_Plugin::isRegistered('SGL_Text_FleschScore') && $this->conf['ArticleMgr']['showfleschScore']) {
	            $f = SGL_Plugin::load('SGL_Text_FleschScore');
	            $output->flesch = $f->getFleschScore($output->dynaContent);
	        }
	
	        function getFleschScore($text)
	        {
	            //  strip tags, parse out raw text
	            $search = array ("'<script[^>]*?>.*?</script>'si",  // Strip javascript
	                             "'<[\/\!]*?[^<>]*?>'si",           // Strip html tags
	                             "'([\r\n])[\s]+'",                 // Strip white space
	                             "'\*'si");
	            $replace = array (' ', ' ', '\1', '');
	            $lines = preg_replace($search, $replace, $text);
	
	            //  body text occurs in 4th element
	            if (!isset($lines[4])) {
	                $lines[4] = '';
	            }
	            $rawTxt = strip_tags($lines[4]);
	
	            //  detect if sufficient text to run stats
	            //  minimum is one word and a full stop
	            $bContainsPeriod = (boolean)preg_match("/\./", $rawTxt);
	            $words = explode(' ', $rawTxt);
	            if (count($words) && $bContainsPeriod) {
	                require_once 'Text/Statistics.php';
	                $block = & new Text_Statistics($rawTxt);
	                return number_format($block->flesch, 2);
	            } else {
	                return 'n/a';
	            }
	        }
	
	
	///////////////////
	//  example with db
	///////////////////
	
	/*
	
	 * a module requires db connectivity
	 * choice between pear::db and mdb2
	 * SGL_Plugin::isRegistered('PEAR_DB')
	 * on registration sets $this->dbh to valid db handle
	
	*/
	
	    function SGL_Manager()
	    {
	        SGL::logMessage(null, PEAR_LOG_DEBUG);
	
	        $c = &SGL_Config::singleton();
	        $this->conf = $c->getAll();
	        $this->dbh = $this->_getDb();
	    }
	
	    function &_getDb()
	    {
	        if (SGL_Plugin::isRegistered('PEAR_DB')) {
	            $locator = &SGL_ServiceLocator::singleton();
	            $dbh = $locator->get('DB');
	            if (!$dbh) {
	                $dbh = & SGL_DB::singleton();
	                $locator->register('DB', $dbh);
	            }
	            return $dbh;
	        }
	    }
	
	///////////////////////
	//  example with config
	///////////////////////
	
	/*
	 * for apps that want to use yaml for config
	 * SGL_Plugin::isRegistered('SGL_ParamHandler_Yml')
	 * $bar = SGL_Config::get('site/foo/bar');
	
	*/
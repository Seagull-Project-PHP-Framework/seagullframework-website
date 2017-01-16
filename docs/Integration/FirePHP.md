<!-- Name: Integration/FirePHP -->
<!-- Version: 5 -->
<!-- Last-Modified: 2009/06/18 18:39:09 -->
<!-- Author: malber -->
## FirePHP Integration
FirePHP (http://www.firephp.org/ and http://www.firephp.org/HQ/Use.htm) enables you to log to your Firebug Console using a simple PHP method call. All data is sent via response headers and will not interfere with the content on your page. FirePHP is ideally suited for AJAX development where clean JSON and XML responses are required. 

**** Note: Work with PHP 5 only ****
 
### Requirements
 
FireFox [[BR]]
FireBug add-on https://addons.mozilla.org/en-US/firefox/addon/1843 [[BR]]
FirePHP add-on https://addons.mozilla.org/en-US/firefox/addon/6149 [[BR]]

### Installation
 1. Get FirePHP Core library http://www.firephp.org/DownloadRelease/FirePHPLibrary-FirePHPCore-0.2.1 [[BR]]
 1. Copy the contents of FirePHPCore-0.2.1/lib/ into {seagull}/lib/other the result should be:[[BR]]


    lib/other/FirePHPCore with fb.php, FirePHP.class.php, and LICENSE

### Wrapper Class
Currently there is no customization of the FirePHP class, but this 'wrapper' class will allow for future customization if it is required. 

    require_once 'other/FirePHPCore/FirePHP.class.php';
    
    class SGL_FirePhp extends FirePHP 
    {
        
    }

### default/classes/ConfigMgr.php
Add a new log type of 'firephp'

    $this->aLogTypes = array(
                'file' => 'file',
                'mcal' => 'mcal',
                'sql' => 'sql',
                'syslog' => 'syslog',
                'firephp' => 'firephp',
                );

### Integration Points
#### SGL::logMessages()
If the log type is set to 'firephp' then ...

    if (!empty($conf['log']['type']) && $conf['log']['type'] == 'firephp' && version_compare(phpversion(), '5.0.0') >= 0) {
        require_once SGL_CORE_DIR . '/FirePhp.php';
        $logger = SGL_FirePhp::getInstance(true);
                
        // Disable logging for live sites
        if(!empty($conf['debug']['production']) && $conf['debug']['production']) {
            $logger->setEnabled(false);
        }            
    } else {
        include_once 'Log.php';
                
        // Instantiate a logger object based on logging options
        $logger = & Log::singleton($conf['log']['type'],
                    $logName,
                    $conf['log']['ident'],
                    array(  $conf['log']['paramsUsername'],
                            $conf['log']['paramsPassword'],
                           'dsn' => $dsn
                    ));
    }

#### ErrorHandler
If FirePHP is enabled then use the CLI error output format and output the error message to the FireBug Console.

    if (SGL::runningFromCLI() || 
        (!empty($conf['log']['type']) && 
        $conf['log']['type'] == 'firephp' && 
        version_compare(phpversion(), '5.0.0') >= 0)) {
        $output = <<<EOL
    MESSAGE: $errStr
    TYPE: {$this->errorType[$errNo][0]}
    FILE: $file
    LINE: $line
    
     --
    EOL;
    }
    
    if (!empty($conf['log']['showErrors']) && $conf['log']['showErrors'] == true) {
        if (!empty($conf['log']['type']) && 
            $conf['log']['type'] == 'firephp' && 
            version_compare(phpversion(), '5.0.0') >= 0) {
            require_once SGL_CORE_DIR . '/FirePhp.php';
            $logger = SGL_FirePhp::getInstance(true);
            $logger->log($output);
        } else {
            echo $output;
        }
    }

#### Manager
FirePHP can also be used for debugging. Instead of using echo to provide a quick way to output variable values, etc. FirePHP can be used to output those values to the console 

    require_once SGL_CORE_DIR . '/FirePhp.php';

In the Managers constructor add something like this

    $this->firePhp = SGL_FirePhp::getInstance(true);
    
    // Disable logging for live sites
    if($this->conf['debug']['production']) {
        $this->firePhp->setEnabled(false);
    }

Then in the _cmd_foo() method

    $this->firePhp->log('Plain Message');
    $this->firePhp->info('Info Message');
    $this->firePhp->warn('Warn Message');
    $this->firePhp->error('Error Message');
    
    $this->firePhp->dump('Key', $variable);

### Settings to enable FirePHP logging:
debug - production = false [[BR]]
debug - show errors = true [[BR]]
log - type = firephp [[BR]]
log - enabled = true [[BR]]

### Diff Files, Etc
Ticket URL: http://trac.seagullproject.org/ticket/1697

<!-- Name: RFC/HTTPCompression -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/12/31 00:18:54 -->
<!-- Author: demian -->
# HTTP Compression

Some time ago I found interesting class HTTP_Compress. Just add this 
code to _setHeaders():

    #!php
    <?php
    if($conf['site']['compression']) {
         require_once('HTTP/Compress.php');
         HTTP_Compress::start();
    }
    ?>
and this code at the end of _displayPage():

    #!php
    <?php
    if($conf['site']['compression']) {
         HTTP_Compress::output();
    }
    ?>
I found also that internet explorer 5 often crashed (very very often) on 
sites with compression on. Here is a little workaround:
in _setHeaders():

    #!php
    <?php
    if($conf['site']['compression']) {
         require_once('HTTP/Compress.php');
         require_once('Net/Detect.php');
         $bw = Net_UserAgent_Detect::singleton();
         if( !$bw->isBrowser('ie5') ) {
             $this->compress = true;
             HTTP_Compress::start();
         }
    }
    ?>
and to _displayPage():

    #!php
    <?php
    if(!empty($this->compress] {
         HTTP_Compress::output();
    }
    ?>

best regards,
Radek



    #!php
    <?php
    ''
    '' +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
    '' | PHP Version 4                                                        |
    '' +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
    '' | Copyright (c) 1997-2002 The PHP Group                                |
    '' +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
    '' | This source file is subject to version 2.02 of the PHP license,      |
    '' | that is bundled with this package in the file LICENSE, and is        |
    '' | available at through the world-wide-web at                           |
    '' | http://www.php.net/license/2_02.txt.                                 |
    '' | If you did not receive a copy of the PHP license and are unable to   |
    '' | obtain it through the world-wide-web, please send a note to          |
    '' | license@php.net so we can mail you a copy immediately.               |
    '' +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
    '' | Authors: Mark Nottingham <mnot@pobox.com>                            |
    '' |          Chuck Hagenbuch <chuck@horde.org>                           |
    '' +~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~+
    ''
    '' $Id: Compress.php,v 1.1 2004/01/25 10:35:32 chief Exp $
    ''
    
    /'''
     * HTTP_Compress:: provides a wrapper around php's output buffering
     * mechanisms and also does compression, generates headers - ETag,
     * Content-Length, etc. - which may be beneficial to bandwidth
     * usage and performance.
     *
     * @author Mark Nottingham <mnot@pobox.com>
     * @author Chuck Hagenbuch <chuck@horde.org>
     * @version $Revision: 1.1 $
     * @package HTTP
     */
    class HTTP_Compress {
        
        /'''
         * Start the output buffer, and make sure that implicit flush is
         * off so that data is always buffered.
         * @access public
         */
        function start()
        {
            ob_start();
            ob_implicit_flush(0);
        }
        
        /'''
         * Output the contents of the output buffer, compressed if
         * desired, along with any relevant headers.
         *
         * @param boolean $compress (optional) Use gzip compression, if the browser supports it.
         * @param boolean $use_etag Generate an ETag, and don't send the body if the browser has the same object cached.
         * @param boolean $send_body Send the body of the request? Might be false for HEAD requests.
         * @access public
         */
        function output($compress = true, $use_etag = true, $send_body = true)
        {
            $min_gz_size = 1024;
            $page = ob_get_contents();
            $length = strlen($page);
            ob_end_clean();
            
            if ($compress && extension_loaded('zlib') && (strlen($page) > $min_gz_size) && isset($GLOBALS['HTTP_SERVER_VARS']['HTTP_ACCEPT_ENCODING']] {
                $ae = explode(',', str_replace(' ', '', $GLOBALS['HTTP_SERVER_VARS']['HTTP_ACCEPT_ENCODING']];
                $enc = false;
                if (in_array('gzip', $ae] {
                    $enc = 'gzip';
                } else if (in_array('x-gzip', $ae] {
                    $enc = 'x-gzip';
                }
                
                if ($enc) {
                    $page = gzencode($page);
                    $length = strlen($page);
                    header('Content-Encoding: ' . $enc);
                    header('Vary: Accept-Encoding');
                } else {
                    $compress = false;
                }
            } else {
                $compress = false;
            }
            
            if ($use_etag) {
                $etag = '"' . md5($page) . '"';
                header('ETag: ' . $etag);
                if (isset($GLOBALS['HTTP_SERVER_VARS']['HTTP_IF_NONE_MATCH']] {
                    $inm = explode(',', $GLOBALS['HTTP_SERVER_VARS']['HTTP_IF_NONE_MATCH']);
                    foreach ($inm as $i) {
                        if (trim($i) == $etag) {
                            header('HTTP/1.0 304 Not Modified');
                            $send_body = false;
                            break;
                        }
                    }
                }
            }
            
            if ($send_body) {
                header('Content-Length: ' . $length);
                echo $page;
            }
        }
        
    }
    ?>
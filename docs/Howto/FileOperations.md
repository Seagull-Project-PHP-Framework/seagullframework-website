<!-- Name: Howto/FileOperations -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/11/30 15:43:42 -->
<!-- Author: demian -->

# Working With Files
* TOC
{:toc}

## Searching for Files
Seagull gives access to some nice PEAR utilities for managing directory listings.  Basically using PEAR's File::Util located in SGL\_Util you can:
  * list files or directories in one line of code
  * filter the results
  * tranform the results
The latter two features are achieved using callback functions, here's an example of all of the above:


	/***
	 * Various utility methods.
	 *
	 * @package SGL
	 * @author  Demian Turner <demian@phpkitchen.com>
	 * @version $Revision: 1.12 $
	 * @since   PHP 4.1
	 */
	class SGL_Util
	{
	    //  other methods left of for clarity
	
	    function getAllNavDrivers()
	    {
	        SGL::logMessage(null, PEAR_LOG_DEBUG);
	
	        require_once 'File/Util.php';
	        $navDir = SGL_MOD_DIR . '/navigation/classes';
	
	        //  match files with *Nav.php format
	        $ret = SGL_Util::listDir($navDir, FILE_LIST_FILES, $sort = FILE_SORT_NONE, 
	                create_function('$a', 'return preg_match("/.*Nav\.php/", $a);'];
	
	        //  parse out filename w/o extension and .
	        array_walk($ret, create_function('&$a', 'preg_match("/^.*Nav/", $a, $matches); $a =  $matches[0]; return true;'];
	        return $ret;
	    }
	
	    /**
	     * Wrapper for the File_Util::listDir method.
	     * 
	     * Instead of returning an array of objects, it returns an array of
	     * strings (filenames).
	     * 
	     * The final argument, $cb, is a callback that either evaluates to true or
	     * false and performs a filter operation, or it can also modify the 
	     * directory/file names returned.  To achieve the latter effect use as 
	     * follows:
	     * 
	     * <code>
	     * function uc(&$filename) {
	     *     $filename = strtoupper($filename);
	     *     return true;
	     * }
	     * $entries = File_Util::listDir('.', FILE_LIST_ALL, FILE_SORT_NONE, 'uc');
	     * foreach ($entries as $e) {
	     *     echo $e->name, "\n";
	     * }
	     * </code>
	     * 
	     * @static
	     * @access  public
	     * @return  array
	     * @param   string  $path
	     * @param   int     $list
	     * @param   int     $sort
	     * @param   mixed   $cb
	     */
	    function listDir($path, $list = FILE_LIST_ALL, $sort = FILE_SORT_NONE, $cb = null)
	    {
	        $aFiles = File_Util::listDir($path, $list, $sort, $cb);
	        $aRet = array();
	        foreach ($aFiles as $oFile) {
	            $aRet[$oFile->name] = $oFile->name;
	        }
	        return $aRet;
	    }
	}

== Using PEAR's System::find to search for files==
Searching a folder for files with a certain extension:


	    $dir = SGL_PATH . '/tmpl/basic';
	    $aFiles = System::find("$dir -name *.php -name *.serial");
	    if (!@System::rm($aFiles] {
	        SGL::raiseError('There was a problem deleting the files', 
	            SGL_ERROR_FILEUNWRITABLE);
	    } else {
	        SGL_Output::msgSet('Template cache successfullly deleted');
	        SGL_HTTP::redirect('translationMgr.php');
	    }


## Writing Files
The PHP5-only function, file\_put\_contents(), is emulated by Seagull when PHP4 is detected.  This is a handy function for quickly writing output to a file.

[[AddComment]]
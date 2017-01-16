<!-- Name: Limbo/CreatingYourOwnModules/BookmarkMgrphp -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/04/16 18:25:18 -->
<!-- Author: demian -->
FIXME: Look over new file... 

Ok, now we get down to the meat of things.  The framework does all the repetitive work, making it easier for you to concentrate on the code that's specific to your application.  This is that code: the class code.

#### Basics

At the beginnning please include the /SeagullHeader ;)  This header (basically the BSD licence and some identifiers) is necessary for modules you plan to contribute back to the Seagull project. Of course, since Seagull is BSD-licensed, you are not *required* to license your modules the same way, if you have a reason not to.  Please note, as indicated in the BSD license,  however, redistributions of source code must retain the original copyright notice.  If you're doing a job that you think is framework related rather than specific to your business area, feel free to design it in a portable, reusable fashion so that others can take advantage of it ~~ and you can take advantage of their help in maintaining it.  (Since that is the Open Source social contract... :-)

OK. let's PHP now:

First include the DataObject file:

    require_once SGL_ENT_DIR . '/Bookmark.php';
That's the file that the DataObject package creates based on the tables in your schema.

Let's begin with the main class: _BookmarkMgr_

This is extended from the class _SGL_Manager_ which acts as an interface, in the sense that it lists the methods you must implement. 

    /'''
     * To allow users to exchange interesting bookmarks.
     *
     * @package bookmark
     * @author  Werner Krauß <werner.krauss@hallstatt.net>
     * @version $Revision: 1.21 $
     * @since   PHP 4.1
     */
    class BookmarkMgr extends SGL_Manager
    {
        function BookmarkMgr()
        {
            '' set the desired debugging log level
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            parent::SGL_Manager();
    
            '' fill in some names and descriptions; these will be what you give the module manager later
            $this->module       = 'bookmark';
            $this->pageTitle    = 'Bookmark Manager';
            $this->template     = 'bookmarkList.html';
    
            '' and fill in the actions mapping array
            '' there are a collection of standard actions: the key is the param that's picked up from $_REQUEST, 
            '' the value array is the list of action methods that will be executed as a consequence, ie. if there are 
            '' more than one action, this usually represents a redirect that happens after the action
            $this->_aActionsMapping =  array(
                'add'       => array('add'),
                'insert'    => array('insert', 'redirectToDefault'),
                'edit'      => array('edit'),
                'update'    => array('update', 'redirectToDefault'),
                'reorder'   => array('reorder'),
                'reorderUpdate' => array('reorderUpdate', 'redirectToDefault'),
                'delete'    => array('delete', 'redirectToDefault'),
                'list'      => array('list'),
                'redirect'  => array('redirect'),
            );
        }
    ?>

In the constructor you define some basics. Very important is _$this->_aActionsMapping _. If you want to add functionality to the class don't forget to add your new actions to this array.  The constructor is called by the Front Controller, when the page is retrieved.

Next the method _validate()_:

        function validate($req, &$input)
        {
            '' again, we set the debug level
            SGL::logMessage(null, PEAR_LOG_DEBUG);
    
            '' start off assuming that the form input data is valid
            $this->validated    = true;
    
            '' and put a bunch of stuff into the input object
            $input->error       = array();
            $input->pageTitle   = $this->pageTitle;
            $input->masterTemplate = $this->masterTemplate;
            $input->template    = $this->template;
            $input->action      = ($req->get('action'] ? $req->get('action') : 'list';
            $input->aDelete     = $req->get('frmDelete');
            $input->bookmarkId  = $req->get('frmBookmarkId');
            $input->items       = $req->get('_items');
            $input->submit      = $req->get('submitted');
            $input->bookmark    = (object)$req->get('bookmark');
    
            '' check all the form fields for valid data
            if ($input->submit) {
                if (empty($input->bookmark->url] {
                    $aErrors['url'] = 'Please fill in a url';
                }
                if (empty($input->bookmark->name] {
                    $aErrors['name'] = 'Please fill in a name';
                }
                if (empty($input->bookmark->text] {
                    $aErrors['text'] = 'Please fill in a description';
                }
            }
    
            '' if errors have occured
            if (isset($aErrors) && count($aErrors] {
                SGL::raiseMsg('Please fill in the indicated fields');
                $input->error = $aErrors;
                $this->validated = false;
            }
        }

Take a special look at this line from that block of code:

    
            $input->action      = ($req->get('action'] ? $req->get('action') : 'list';

It determines which action parameter has been passed from $_REQUEST and assigns it the $input->action, setting 'list' as the default action.

In this method, as you can see, you can also add some form validation.

#### Functionality

Oh! You wanted your programs to actually *do* something? :-)  Then you'll have to implement some code to perform the actions you specified above.

First, we'll explain the standard method __cmd_list()_:

        function _cmd_list(&$input, &$output)
        {
            '' as always, set the debug level
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            
            '' check to see if the user is an administrator; if so, replace the template
            '' (remember: we set that out in the class definition?) with the admin template
            if (SGL_Session::getUserType() == SGL_ADMIN) {
                $output->template = 'bookmarkListAdmin.html';
                $output->pageTitle = 'Bookmark';
            }
    
            '' create a new DataObject from the bookmark table, and set it's sort order
            $bookmarkList = & new DataObjects_Bookmark();
            $bookmarkList->orderBy('item_order');
            
            '' tell DataObject to go get the list ~~ the whole list, since there are no params to find()
            $result = $bookmarkList->find();
    
            $aBookmarks = array();
            if ($result > 0) {
                while ($bookmarkList->fetch(] {
                    '' convert newlines to BReaks, if necessary
                    $bookmarkList->text = nl2br($bookmarkList->text);
    
                    ''add to array
                    $aBookmarks[] = clone($bookmarkList);
                }
            }
    
            '' and then put the results array into the output object, where Seagull can pick it up later in the process
            $output->results = $aBookmarks;
        }
    
First we determine if the user is an admin user, in which case you'll see the admin template (with edit/delete options, etc.), otherwise the normal list.

Then we just add hot water and stir, eg. fetch the data from the database and then output it in the template. All variables and objects that should be available in the templates are passed to the _$output_ object.  The template engine code then does the work of populating the final HTML output with the data you put into that _$output_ object.

~~-

The next two methods are for adding a new entity. The first, 'add', outputs the form, the next, 'insert' receives the form input and saves it to the DB. As always, the name of the actual function is the action name with a leading single underscore.  

The work is done with the PEAR module DB_DataObject. Cool, isn't it?

        function _cmd_add(&$input, &$output)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
    
            $output->template = 'bookmarkEdit.html';
            $output->action   = 'insert';
            $output->bookmark = & new DataObjects_Bookmark();
        }
    
        function _cmd_insert(&$input, &$output)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
    
            ''  get new order (sequence) number
            $bookmark = & new DataObjects_Bookmark();
            $bookmark->selectAdd();
            $bookmark->selectAdd('MAX(item_order) AS new_order');
            $bookmark->groupBy('item_order');
            $maxItemOrder = $bookmark->find(true);
            unset($bookmark);
    
            ''  insert record
            $bookmark = & new DataObjects_Bookmark();
            $bookmark->setFrom($input->bookmark);
            $dbh = $bookmark->getDatabaseConnection();
            $bookmark->bookmark_id = $dbh->nextId('bookmark');
            $bookmark->last_updated = $bookmark->date_created = SGL::getTime(true);
            $bookmark->item_order = $maxItemOrder + 1;
            $success = $bookmark->insert();
            if ($success) {
                SGL::raiseMsg('Bookmark saved successfully');
            } else {
               SGL::raiseError('There was a problem inserting the record', SGL_ERROR_NOAFFECTEDROWS);
            }
        }

Editing and updating is nearly the same.  Note that _add and _edit use the same template.

        function _cmd_edit(&$input, &$output)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            $output->template = 'bookmarkEdit.html';
            $output->action   = 'update';
            $output->pageTitle = $this->pageTitle . ' :: Edit';
            $bookmark = & new DataObjects_Bookmark();
    
            ''  get bookmark data
            $bookmark->get($input->bookmarkId);
            $output->bookmark = $bookmark;
        }
    
        function _cmd_update(&$input, &$output)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            $bookmark = & new DataObjects_Bookmark();
            $bookmark->get($input->bookmark->bookmark_id);
            $bookmark->setFrom($input->bookmark);
            $bookmark->last_updated = SGL::getTime(true);
            $success = $bookmark->update();
            if ($success) {
                SGL::raiseMsg('Bookmark updated successfully');
            } else {
                SGL::raiseError('There was a problem updating the record', SGL_ERROR_NOAFFECTEDROWS);
            }
        }
    

And maybe you want to delete something you don't like any more.

        function _cmd_delete(&$input, &$output)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            if (is_array($input->aDelete] {
                foreach ($input->aDelete as $index => $bookmarkId) {
                    $bookmark = & new DataObjects_Bookmark();
                    $bookmark->get($bookmarkId);
                    $bookmark->delete();
                    unset($bookmark);
                }
            } else {
                SGL::raiseError('Incorrect parameter passed to ' . __CLASS__ . '::' .
                    __FUNCTION__, SGL_ERROR_INVALIDARGS);
            }
            SGL::raiseMsg('bookmark deleted successfully');
        }

This is for clicking on a link. The counter is incremented, and SGL redirects to the given url.

        function _cmd_redirect(&$input, &$output)
        {
           SGL::logMessage(null, PEAR_LOG_DEBUG);
            $bookmark = & new DataObjects_Bookmark();
    
            ''  get bookmark data
            $bookmark->get($input->bookmarkId);
    
            ''update DB: count +1
            $bookmark->count ++;
            $bookmark->update();
    
            ''redirect to url...
            $url = $bookmark->url;
            SGL_HTTP::redirect($url,'');
        }
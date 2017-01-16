<!-- Name: Limbo/CreatingYourOwnModules/Source -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/04/16 18:24:43 -->
<!-- Author: demian -->
# Source Code for 'Creating Your Own Modules'

Back to CreatingYourOwnModules...

Bingo, this is the main class of this module.

In the following i'll explain the structure of this class:

First include the DataObject File


    #!php
    <?php
    /* Reminder: always indent with 4 spaces (no tabs). */
    
    require_once SGL_ENT_DIR . '/Bookmark.php';
    ?>

Let's begin with the main class: _BookmarkMgr_
This is extended from the class _SGL_Manager_ which acts as an interface, in the sense it lists the methods you must implement.


    #!php
    <?php
    class BookmarkMgr extends SGL_Manager
    {
        var $module = 'bookmark';
    
        function BookmarkMgr()
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            $this->pageTitle    = 'Bookmark Manager';
            $this->template     = 'bookmarkList.html';
            $this->_aAllowedActions = array(
                'add', 'insert', 
                'edit', 'update', 
                'delete', 'list', 
                'reorder', 'reorderUpdate',
                'redirect');
        }
    ?>

In the constructor you define some basics. Very important is _$this->_aAllowedActions _. If you want to add functionality to the class don't forget to add your new actions to this array.

Next the method _validate()_


    #!php
    <?php
        function validate($req, $input)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            $this->validated    = true;
            $input->error       = array();
            $input->pageTitle   = $this->pageTitle;
            $input->template    = $this->template;
            $input->action      = ($req->get('action'] ? $req->get('action') : 'list';
            $input->aDelete     = $req->get('frmDelete');
            $input->rightCol    = true;
            $input->linkListId  = $req->get('bookmarkId');
            $input->items       = $req->get('_items');
            $input->submit      = $req->get('submitted');
            $input->bookmark        = (object)$req->get('bookmark');
    
            if ($input->submit) {
                if (empty($input->Bookmark->url] {
                    $aErrors[] = SGL_Output::translate('Please fill in a url');
                }
                if (empty($input->bookmark->name] {
                    $aErrors[] = SGL_Output::translate('Please fill in a name');
                }
                if (empty($input->bookmark->text] {
                    $aErrors[] = SGL_Output::translate('Please fill in a text');
                }
            }
            ''  if errors have occured
            if (isset($aErrors) && count($aErrors] {
                SGL_Output::msgSet('Please fill in the indicated fields');
                $input->error = $aErrors;
                $input->template = 'linkListAdd.html';
                $this->validated = false;
            }
            return $input;
        }
    ?>

have a look at this line

    #!php
    <?php
            $input->action      = ($req->get('action'] ? $req->get('action') : 'list';
    ?>

It determines which action parameter has been passed from $_REQUEST and assigns it the $input->action, setting 'list' as the default action.

In this method you can also add some form valiadation.

Now two methods i just copied from the faq module


    #!php
    <?php
        function process($input, $output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
    
            ''  determine method implied by specified parameter
            $methodName = '_' . $input->action;
            $className = get_class($this);
    
            ''  build relevant perms constant
            $perm = @constant('SGL_PERMS_' . strtoupper($className . $methodName];
    
            ''  check if method allowed
            if (in_array($input->action, $this->_aAllowedActions) && 
                method_exists($this, $methodName] 
            {
                ''  and if user has perms for this method
                if (SGL_HTTP_Session::hasPerms($perm] {
                    $this->$methodName($input, $output);
                } else {
                    Base::raiseError('you do not have the required perms for ' . 
                        $className . '::' .$methodName, SGL_ERROR_INVALIDMETHODPERMS);
                }
            } else {
                Base::raiseError('The specified method, ' . $methodName . 
                    ' does not exist', SGL_ERROR_NOMETHOD, PEAR_ERROR_DIE);
            }
            return $output;
        }
    
        function display($output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            return $output;
        }
    ?>

Please note the process method is almost always the same for every module, it ensures that action methods can invoke only allowed actions, and that the current user has permissions to invoke the method.

The next two methods are for adding a new entity. The first outputs the form, the next receives the form input and saves it to the db. This is done with the PEAR module DB_DataObject. Cool, isn't it?


    #!php
    <?php
        function _add(&$input, &$output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            $output->template = 'bookmarkAdd.html';
            $output->pageTitle = $this->pageTitle . ' :: Add';
            ''  build ordering select object
            $output->bookmark = & new DataObjects_Bookmark();
        }
    
        function _insert(&$input, &$output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            ''  get new order number
            $bookmark = & new DataObjects_Bookmark();
            $bookmark->selectAdd();
            $bookmark->selectAdd('MAX(item_order) AS new_order');
            $bookmark->groupBy('item_order');
            $maxItemOrder = $bookmark->find(true);
            unset($ll);
            ''  insert record
            $bookmark = & new DataObjects_Bookmark();
            $bookmark->setFrom($input->link);
            $dbh = $bookmark->getDatabaseConnection();
            $bookmark->bookmark_id = $dbh->nextId('bookmark');
            $bookmark->last_updated = $bookmark->date_created = Base::getTime(true);
            $bookmark->item_order = $maxItemOrder + 1;
            $success = $bookmark->insert();
            if ($success) {
                ''  redirect on success
                SGL_Output::msgSet('bookmark saved successfully');
                SGL_HTTP::redirect('bookmarkMgr.php', array('action'=> 'list'];
            } else {
               Base::raiseError('There was a problem inserting the record', SGL_ERROR_NOAFFECTEDROWS);
            }
        }
    ?>

Same with editing and updating an entry...


    #!php
    <?php
        function _edit(&$input, &$output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            $output->template = 'bookmarkEdit.html';
            $output->pageTitle = $this->pageTitle . ' :: Edit';
            $bookmark = & new DataObjects_Bookmark();
            ''  get bookmark data
            $bookmark->get($input->bookmarkId);
            $output->bookmark = $bookmark;
        }
    
        function _update(&$input, &$output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            $bookmark = & new DataObjects_Bookmark();
            $bookmark->get($input->bookmark->bookmark_id);
            $bookmark->setFrom($input->bookmark);
            $bookmark->last_updated = Base::getTime(true);
            $success = $bookmark->update();
            if ($success) {
                ''  redirect on success
                SGL_Output::msgSet('bookmark updated successfully');
                SGL_HTTP::redirect('bookmarkMgr.php', array('action'=> 'list'];
            } else {
                Base::raiseError('There was a problem updating the record', SGL_ERROR_NOAFFECTEDROWS);
            }
        }
    ?>

And maybe you want to delete something you don't like any more.


    #!php
    <?php
        function _delete(&$input, &$output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            if (is_array($input->aDelete] {
                foreach ($input->aDelete as $index => $bookmarkId) {
                    $bookmark = & new DataObjects_Bookmark();
                    $bookmark->get($bookmarkId);
                    $bookmark->delete();
                    unset($bookmark);
                }
            } else {
                Base::raiseError('Incorrect parameter passed to ' . __CLASS__ . '::' . 
                    __FUNCTION__, SGL_ERROR_INVALIDARGS);
            }
            ''  redirect on success
            SGL_Output::msgSet('bookmark deleted successfully');
            SGL_HTTP::redirect('bookmarkMgr.php', array('action'=> 'list'];
        }
    ?>


This is for clicking on a link. The counter is made bigger and SGL redirects to the given url.


    #!php
    <?php   
        function _redirect(&$input, &$output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            ''  get entry from db
            $bookmark = & new DataObjects_Bookmark();
            $bookmark->get($input->bookmarkId);
            ''  update db: count +1
            $bookmark->count ++;
            $bookmark->update();
            ''  redirect to url...
            $url = $bookmark->url;
             SGL_HTTP::redirect($url,'');
        }
    ?>

And filnally, the list method is called if no action is defined in $_REQUEST.

First we determine the the user is an admin user, in which case you'll see the admin  menu, otherwise the normal user menu.


    #!php
    <?php
        function _list(&$input, &$output)
        {
            Base::logMessage(   __CLASS__ . '::' . __FUNCTION__ ,
                null, null, PEAR_LOG_DEBUG);
            if (SGL_HTTP_Session::getAuthLevel() == SGL_ADMIN) {
                $output->template = 'bookmarkListAdmin.html';
                $output->pageTitle = $this->pageTitle . ' :: Browse';
            } else {
                $output->pageTitle = 'Bookmarks';
            }
            $bookmark = & new DataObjects_Bookmark();
            $bookmark->orderBy('item_order');
            $result = $bookmark->find();
            $aLinks = array();
            if ($result > 0) {
                while ($bookmark->fetch(] {
                    $bookmark->text = nl2br($bookmark->text);
                    $aLinks[] = $bookmark->__clone();
                }
            }
            $output->results = $aLinks;
        }
    }
    ?>

Back to CreatingYourOwnModules...
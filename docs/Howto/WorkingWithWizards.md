<!-- Name: Howto/WorkingWithWizards -->
<!-- Version: 5 -->
<!-- Last-Modified: 2007/06/12 16:29:06 -->
<!-- Author: lyric -->

# Working with Wizards

FIXME: Malaney to update

Thanks to [wiki:User/TobiasKuckuck] for the following.  Note: Wizard functionality needs to be updated to work with 0.5.x code.

## Overview
In the docs/developer/exaples/modules/wizard the menu to jump to an arbitrary wizard step is  already included. Additional to this I added the following features in my project:

the wizard always runs in a popup window without the default 
navigation with help from a special navigation driver. In \_toHtml the 
following code ensures this:


	// cover $url with windowOpen if uri contains string 'Wiz'
	if (strstr($section->resource_uri,'Wiz'] {
	     $url = 'javascript:openWindow(\'' . $url . '\');';
	}

I used templates and action methods in the same way the other modules 
do. Besides some special methods in nearly every step there are the 
methods "edit", "update", "view" and in the first step additionally 
"add" and "insert" (for registering as a candidate). The default actions 
are "edit"/"update" and "add"/"insert" are only invoked if you start the 
wizard with the required action variables. Below you find the required 
code for the manager calling the first wizard step:


	// set action and actionId for editing, etc.
	SGL_Session::set('action',$req->get('action'];
	SGL_Session::set('actionId',$req->get('actionId'];

In the first step these session variables will be used to determine, if a 
special action should be performed:


	//set special action and usrId
	if (SGL_Session::get('action'] {
	     $defaultAction = SGL_Session::get('action');
	     $input->usrId = SGL_Session::get('actionId');
	     SGL_Session::remove('action');
	     SGL_Session::remove('actionId');
	}

Every step saves its data in the database before switching to another 
step. If the user exits the wizard prior to completing all steps, he is 
able to complete it later.

The data is derived from the session if available, else it is read from 
the database and saved in the session. Because of this you only have to 
read the data from database for each step only once (until exiting the 
wizard).

In the last step I close the popup window on finish and reload the 
main window with a special url to refresh the list.

I set exclusive locks for the edited database record (candidates in my 
project) und reset it after finishing the wizard regularly. Else a 
timeout ensures the lock reset after 30 minutes:

example for locking a row in method "edit":


	$user = & new DataObjects_Usr();
	$user->autocommit();
	
	// Lock Table
	// check action
	// if action = view => don't lock table
	if ($output->action != "view") {
	   require_once SGL_LIB_DIR . '/other/LockDB.php';
	   // instatiate LockDB-class
	   $lockDB = new LockDB($user->getDatabaseConnection(];
	   $lockOn = $lockDB->setLock($conf['table']['user'],$input->usrId);
	}
	
	if ($lockOn) {
	   // direct call, no data in session
	   // get user data
	   $user->get($input->usrId);
	   $output->candidate = $user;
	   $output->candidate->date_of_birth = SGL_Date::stringToArray($user->date_of_birth);
	} else {
	   $output->template = 'doc2Blank.html';
	   SGL::raiseMsg('table is locked');
	}
	$user->commit();

example for unlocking a row:


	// Unlock Table table_lock
	require_once SGL_LIB_DIR . '/other/LockDB.php';
	$dbh = &SGL_DB::singleton();
	$dbh->autocommit();
	
	// instatiate LockDB-class
	$lockDB = new LockDB($dbh);
	$lockOff = 
	$lockDB->releaseLock($conf['table']['user'],$output->candidate->usr_id);
	$dbh->commit();
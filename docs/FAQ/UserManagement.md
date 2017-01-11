<!-- Name: FAQ/UserManagement -->
<!-- Version: 18 -->
<!-- Last-Modified: 2007/06/22 17:44:43 -->
<!-- Author: lyric -->

# User Management
* TOC
{:toc}

[[SubWiki(FAQ)]]

## How do I auto-enable user registrations?
By default, users cannot login to the application after a successful registration.  To enable this and review registrations manually, edit the conf.ini file in seagull/modules/user/ and set the [registermgr][autoEnable] key to true.

## How do I disable registration confirmation emails being sent out?
Edit the conf.ini file in seagull/modules/user/ and set the [registermgr][sendEmailConf] key to false.

## Is there a way to *not* let users register on your site?

In the [source:trunk/modules/user/conf.ini conf.ini] file for the user module, find the *enabled* key under RegisterMgr and set it to false.

## Is there a way to redirect users to custom default module/manager/action ?

### 0.5.5 version

Yes, you can, but it's a bit tricky. You have to define your user redirects in the [source:trunk/modules/user/conf.ini conf.ini], admitting you put the role in the mask `logon_%s_Goto` like this :


	[LoginMgr]
	logonUserGoto          = default^default
	logon_root_Goto        = navigation^page
	logon_member_Goto      = user^account
	logon_subadmin_Goto    = mymodule^mymanager^myaction
	logon_subsubadmin_Goto = mymodule^myothermanager^myotheraction

Then in [source:trunk/modules/user/classes/LoginMgr.php LoginMgr.php] you have to replace this line :


	$type = ($res['role_id'] == SGL_ADMIN) ? 'logonAdminGoto' : 'logonUserGoto';

By this piece of code, eg. to catch user's role :


	$role = DA_User::getRoleNameById($_SESSION['rid']);
	if (array_key_exists('logon'.$role.'Goto', $this->conf['LoginMgr'])) {
	    list($mod, $mgr, $act) = split('\^', $this->conf['LoginMgr']['logon'.$role.'Goto']);
	    $aParams = array(
	        'moduleName'  => $mod,
	        'managerName' => $mgr,
	        );
	    if ($act && trim($act) != '')
	        $aParams['action'] = $act;
	} else {
	    SGL::logMessage('Could note find route for role ' . $role, PEAR_LOG_DEBUG);
	    $aParams = $this->conf['LoginMgr']['logonUserGoto'];
	}
	SGL_HTTP::redirect($aParams);
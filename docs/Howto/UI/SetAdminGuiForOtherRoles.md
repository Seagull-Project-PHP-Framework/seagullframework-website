---
layout: page
title: How to set Admin GUI for another role
permalink: /Howto/UI/SetAdminGuiForOtherRoles.html
---

<!-- Name: Howto/UI/SetAdminGuiForOtherRoles -->
<!-- Version: 14 -->
<!-- Last-Modified: 2007/08/10 14:23:38 -->
<!-- Author: demian -->
# How to set Admin GUI for another role
This is an extended version of this [/RFC/AdminGui][1] that tries to answer the same problem. All the code is made to work with SGL 0.6 RC3 found in 0.6-bugfix on 05 Jul 2006.

* TOC
{:toc}

## NOTE
As of Seagull 0.6.3 you just need to add additional roles to Config that require admin GUI viewing privileges.

In the <host>.conf.php file:

	$conf['site']['rolesHaveAdminGui'] = 'SGL_ADMIN,SGL_TRANSLATOR';

Logged in as admin:


	General -> Configuration -> General -> Roles that can access the admin GUI


## Problem
I want to create a site maintenance administrator role which only has access to certain modules within a site, where the user will also use the default Seagull admin GUI. 

## Step by step guide for adding the new user

1. Duplicate the member role and rename it to "Maintainer". In my case role\_id = 3;

2. Create a new user and assign the "maintainer" role to it

3. Add your new "maintainer" role to [source:/branches/0.6-bugfix/lib/SGL/Task/Init.php] below the existing Seagull user roles, so that your code looks like this:


		    //  Seagull user roles
		    define('SGL_ANY_ROLE',                  -2);
		    define('SGL_UNASSIGNED',                -1);
		    define('SGL_GUEST',                     0);
		    define('SGL_ADMIN',                     1);
		    define('SGL_MEMBER',                    2);
		    define('SGL_MAINTAINER',                3); //Your new maintainer role

4.  Replace the following piece of code in [gitlink:/branches/0.6-bugfix/lib/SGL/Task/Process.php]:

		         if ($userRid == SGL_ADMIN) {
		            $adminGuiAllowed = true;
		        }

with this piece of code, which also gives the maintainer role access to the admin GUI: 

	            // first check if userRID allows to switch to adminGUI
	            if ($userRid == SGL_ADMIN || $userRid == SGL_MAINTAINER) {
	                $adminGuiAllowed = true;
	            }

5. Replace the following getTemplate function code in [gitlink:/branches/0.6-bugfix/lib/SGL/Task/Manager.php]:

		 function getTemplate(&$input)
		{
		    $req = $input->getRequest();
		    $mgrName = $req->get('managerName');
		    $userRid = SGL_Session::getRoleId();
		
		    if (isset($this->conf[$mgrName]['adminGuiAllowed'])
		           && $this->conf[$mgrName]['adminGuiAllowed']
		           && $userRid == SGL_ADMIN) {
		        $this->template = 'admin_' . $this->template;
		    }
		}

with this function:

	function getTemplate(&$input)
	{
	    $req = $input->getRequest();
	    $mgr = $req->get('managerName');
	    $userRid = SGL_Session::getRoleId();
	
	    if (isset($this->conf[$mgrName]['adminGuiAllowed'])
	           && $this->conf[$mgrName]['adminGuiAllowed']
	           && ($userRid == SGL_ADMIN || $userRid == SGL_MAINTAINER)) {
	        $this->template = 'admin_' . $this->template;
	    }
	}

6. If you want the "maintainer" to be able to add and edit articles by using the Publisher Module, then replace [gitlink:/branches/0.6-bugfix/modules/publisher/classes/ArticleMgr.php]:

	&& SGL\_Session::getRoleId() != SGL\_ADMIN) {

with this line of code:

	&& SGL_Session::getRoleId() != SGL_ADMIN && SGL_Session::getRoleId() != SGL_MAINTAINER) {

> (Contributed by jpaszek on Fri Mar  2 17:13:28 2007)

## Manager specific steps
Steps 7,8 and 9 should only be performed if you want the "maintainer" to be able to use or have access to these specific Managers

7. Replace this line of code in [gitlink:/branches/0.6-bugfix/modules/publisher/classes/DocumentMgr.php]:

		  $input->isAdmin = (SGL\_Session::getRoleId() == SGL\_ADMIN)

with this:

	   $input->isAdmin = (SGL_Session::getRoleId() == SGL_ADMIN || SGL_Session::getRoleId() == SGL_MAINTAINER)
[[BR]]

8. Replace this line of code in [gitlink:/branches/0.6-bugfix/modules/publisher/classes/ArticleMgr.php]:

		   if (SGL\_Session::getRoleId() == SGL\_ADMIN) {

with this:


	   if (SGL_Session::getRoleId() == SGL_ADMIN || SGL_Session::getRoleId() == SGL_MAINTAINER) {

9. If you want the Section Manager [gitlink:/branches/0.6-bugfix/modules/navigation/classes/SectionMgr.php] to only show navigations sections which the maintainer is allowed to modify, replace this piece of code to the function `_cmd_list`:

		    //  get all sections
		    $aSections = $this->da->getSectionTree();
		    $output->results  = &$aSections;
		
		$userRid = SGL_Session::getRoleId();
		    if($userRid == SGL_ADMIN) {
		        //  get all sections
		        $aSections        = $this->da->getSectionTree();
		    } else {
		        // get only sections visible by this role
		        $aSections        = $this->da->getSectionTree($userRid);
		    }

And in the function `_editDisplay` in [gitlink:/branches/0.6-bugfix/modules/navigation/classes/SectionMgr.php] replace this line of code:

	      $aNodesForSelect = $this->da->getSectionsForSelect();

with this block of code:

	        $userRid = SGL_Session::getRoleId();
	        if($userRid == SGL_ADMIN) {
	            //  get all sections
	            $aNodesForSelect = $this->da->getSectionsForSelect();
	        } else {
	            // get only sections visible by this role
	            $aNodesForSelect = $this->da->getSectionsForSelect($userRid);
	        }

## Use the admin section to do change some preferences

1. In Blocks publish the following sections for the maintainer role
	 2. Admin Menu - AdminNav
	 3. Categories - AdminCategory

1. Allow navigation sections to be displayed for the "maintainer" role. In my case:
	 2. Navigation
	 3. My Account
	 4. Publishing

1. Allow perms for managers/actions for the "maintainer" role. In my case:
	 2. articlemgr
	 3. articleviewmgr
	 4. categorymgr
	 5. documentmgr
	 6. filemgr
	 7. sectionmgr

## Other Modifications
The following is not absolutely necessary, but you may still perform these actions if you see fit 

Alter the to function getSectionTree in  [gitlink:/branches/0.6-bugfix/modules/navigation/classes/NavigationDAO.php], so that the top few lines look like this:

	 function getSectionTree($roleId = null)
	        {
	            $this->nestedSet->setImage('folder', 'images/treeNav/file.png');
	            $sectionNodes = $this->nestedSet->getTree();
	
	            //  remove the nodes that are not visible by roleId
	            if(!empty($roleId)) {
	                foreach ($sectionNodes as $k => $aValues) {
	                    $aPerms = explode(',', $aValues['perms']);
	                    if(!in_array($roleId, $aPerms) && !in_array(SGL_ANY_ROLE, $aPerms)) {
	                        unset($sectionNodes[$k]);
	                    }
	                }
	            }

Much like in the function getSectionTree alter the function getSectionsForSelect [gitlink:/branches/0.6-bugfix/modules/navigation/classes/NavigationDAO.php] , so that the top few lines look like this:


	    function getSectionsForSelect($roleId = null)
	
	    +/- line 453
	        foreach ($sectionNodesArray as $k => $sectionNode) {
	
	         // show only sections visible by roleId
	             if(!empty($roleId)) { 
	                $aPerms = explode(',', $sectionNode['perms']);
	                if(!in_array($roleId, $aPerms) && !in_array(SGL_ANY_ROLE, $aPerms)) {
	                    unset($sectionNodesArray[$k]);
	                    continue; 
	                }
	            }       

## Patch files 

For a quick start apply the `maintainer_full_patch.txt` patch over a fresh SGL checkout from SVN and install. Login with user: maintainer pass: admin

The `maintainer_patch.txt` contains only the code modifications presented in points. 3,4,5 and 8.

## Conclusion

This guide is meant to show you how you can implement a quick maintainer role in SGL 0.6.3 but is not the best way to do it because the user role is hard coded in the php, but until a more efficient solution is presented, this is sufficient. The best and easiest part about this guide is that it uses the standard user/roles/perms that come with SGL.

*NOTE:* for changes to take effect don't forget to:
 * complete a sync for each user current role
 * clear the cache
 * logout and then login as the maintainer user

[1]:	/RFC/AdminGui.html
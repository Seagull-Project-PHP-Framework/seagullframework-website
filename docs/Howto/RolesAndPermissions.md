<!-- Name: Howto/RolesAndPermissions -->
<!-- Version: 13 -->
<!-- Last-Modified: 2009/01/10 08:35:45 -->
<!-- Author: demian -->
<!-- Status: Original -->

# Roles and Permissions
* TOC
{:toc}

## Introduction
The ability to access any Seagull resource is determined at the most basic level depending on whether the user is
 * an anonymous user
 * an authenticated user

## Authentication
Authentication is the process of identifying oneself to the system with a set of known credentials.  The Seagull authentication mechanism uses a database backend.  Future support for other backends is planned, when the LiveUser package is integrated authentication will be possible using LDAP, plaintext files, POP3 servers, RADIUS, etc.

Once authentication has taken place, Seagull can impose restrictions on resource access with its role-based authorisation scheme.

All screens or "pages" created by Seagull are allowed anonymous access by default.  This is determined by a config setting.  In each module's conf.ini file a list of all the module's managers appears, and set to each one:


	requiresAuth = false

If you don't want to allow anonymous access to the actions managed by each manager, set the manager's *requiresAuth* key to *true*.


	#!html
	<blockquote class="note">
	<h3>The cache must be cleared to activate the new restriction</h3>
	Login as admin, go to maintenance manager and clear the manager configs. All configs are cached under [your-site]/var/config/, so changing the config file in your module directory will not affect your site until you have cleared these files.  Cache clearing is done automatically for all changes made using the GUI.
	</blockquote>

## Authorisation
Once users are authenticated, a global perm check is done per page execution at the application controller level, in [source:/trunk/lib/SGL/Manager.php SGL\_Manager::\_authorise($mgrPerm, $mgrName, $input)] specifically.  You can invoke more specific perm checks in your own code, and also in the templates, but the app controller check ensures that the current authenticated user has permission to perform the requested action on the current manager, in the current module.

*NOTE:* It is not recommended to mix actions which require authentication with anonymously accessible actions within the same manager, enforcing such a division however keeps your code a lot easier to maintain.  If you have very similar functionality that requires both types of access, first create a manager with requiresAuth set to false, then create another requiring authentication which inherits from the first, and can therefore reuse all its actions.  For example:


	FaqMgr (requiresAuth = false)
	======
	 - list
	
	AdminFaqMgr (requiresAuth = true)
	===========
	 - add
	 - edit
	 - delete
	 - ...

## Overview
Seagull provides coarse-grained permission handling by default, with very little setup required by the site admin.  Coarse-grained in this sense means you can control which users can perform which actions within your application.  More fined-grained constraints are possible too, for example, limiting access to categories of content within an action, or limiting access to certain properties of a domain object, but developers must manage this themselves.

Everytime you create a new module that contains at least one manager, Seagull provides tools in the user module to automatically generate the permissions required.  If your manager has 5 actions, selecting 'Manage permissions' then 'detect and add' will create 5 new records in the permission table, and the corresponding constants you can use to test the permission in your code.


	perm records       id
	============       ==
	faqmgr_cmd_list    23
	faqmgr_cmd_add     24
	faqmgr_cmd_insert  25
	faqmgr_cmd_edit    26
	faqmgr_cmd_update  27
	
	
	
	perm constants
	==============
	SGL_PERMS_FAQMGR_CMD_LIST
	SGL_PERMS_FAQMGR_CMD_ADD
	SGL_PERMS_FAQMGR_CMD_INSERT
	SGL_PERMS_FAQMGR_CMD_EDIT
	SGL_PERMS_FAQMGR_CMD_UPDATE

Seagull uses the concept of roles to group permissions together.  Examples of typical roles would be member, editor, admin, etc.  When testing for the right to access a resource you can test the individual perm (or a custom one you create), or just test if the user belongs to the role you created which contains the target perm.  You can also assign _class perms_ which give users access to all actions within a manager.  Using this approach it's very easy to give users access to specific modules in your application, specific managers within a module, or indeed only certain actions within a manager.

So, to review:

  * *guests*: guests can access any content where the Manager's conf.ini file is set to 

	requiresAuth = false

  * *members*: A member role is supplied in the default install, it serves as a simple example.  Members get a copy of all the 'member' role's perms copied against their usr\_id in the user\_permission table.  Therefore you could consider a role as a template of perms, and each user's set of perms is a specialisation of the role they were created in.  This way each user's perms can be tweaked if necessary, in other words, they can have individual perms added or deleted from their role, so two people belonging to the 'member' or any other role don't necessarily have the same set of perms

  * *admin*: no perms set at all, s/he can access all action methods of all managers, you could call this a pseudo role

So as stated above:

  * permissions are mappings to action methods in Manager classes, ie, if you have a PiggyMgr class, and action methods such as: _goesToMarket(), _staysHome() and \_eatsRoastBeef(), each of the methods corresponds to a binary permission stored in the database and represented by a constant in the client code
  * there can be any number of perms/roles in the system
  * roles can be cloned


	> 2) Is the only need for loading all permissions (via
	> $dao-\>retrieveAllPerms()) so that the perm check in
	> Manager-\>process() can fetch the perm\_id value from the global perms array
	> and then check that it is in the user's perm array? e.g. 
	> $perm = @constant('SGL\_PERMS\_' . strtoupper($className . $methodName));
	> ...
	> if (SGL\_Session::hasPerms($perm) {
	> 	$this->$methodName($input, $output);
	> }


That's right, keep in mind that $dao-\>retrieveAllPerms() is called by the SGL\_Process\_SetupPerms() task which builds the global perms array on the first request, then caches the array to disk for subsequent calls providing you have caching enabled.

## Permissions
### Naming Convention
The naming convention is `managerclass_cmd_method`.
So in the faq module where there is one manager class, namely FaqMgr, there are permissions like
  * `faqmgr_cmd_list`
  * `faqmgr_cmd_add`
  * `faqmgr_cmd_insert`
  * `faqmgr_cmd_delete`
  * ...
ie, one permission per method.

### Class wide perm
If you want to grant some users general access to *ALL* methods of a manager class in a module, you need to grant a class perm, ie _classmgr_, and in the case of the faq module you would check

 * `faqmgr`

and there would be no need to check any of the other perms for this manager class.

### Checking custom permissions
Sometimes in your module you may need to check a custom permission, eg MEMBERSHIP\_GOLD. Based on the results you may  choose a different template to show/hide extra functionality according to users permissions, or a non-default theme. Permission can be checked with a static call to the SGL\_Session method


	boolean SGL_Session::hasPerms(int $permId)

All permission IDs are represented as constants, so in real world example you would use it like this:


	if (SGL_Session::hasPerms(SGL_PERMS_MEMBERSHIP_GOLD)) {
	   // specific privilege
	}


### Adding Perms
You can easily add permissions to the system using the *detect & add* functionality in the user module. It scans all installed modules and outputs a list of new permissions you can add with a few mouse clicks.

### Deleting Orphaned Perms
...is very easy using the *remove orphaned* functionality. It will show you a list of perms that don't have corresponding managers.
 
## Roles
### Creating Roles
A role is just a group of perms, when you create a new user it acquires all the perms of its role 'template'.  After that you can customise the user's privileges by adding/removing perms as required.

To create a new role for the press-only section of your site, just use the tools in the role section of the user module.  The easiest way to do this is:

  * duplicate the *member* role
  * rename it to what you need
  * hit *manage roles* then select the *change* button for your role
  * move your new perms from left box to right box and hit submit

### Syncing users after roles have been updated
Because the perms a role contains are saved on a per-user basis, you may want to sync the user's perms back to their role after modifying the role assignments.

This can easily be done using the *sync perms with role* functionality.  You can select the users you want to sync, if you want to sync them with the current or another role, and if you want to add missing perms, delete extra perms or both.

### Applying Permissions to Articles
Q: Is there a way to control permissions by article category? 

A: There is a perms manager that forms part of the Publisher module.  You need to decide which category your articles should appear in, then go to Publisher's *Categories* section, then remove perms from the roles you don't want to have access for the given category.

At the time of writing, this is just for viewing articles, not for editing.

## Extending the permission/roles functionality
Developers can specify a more scalable perms method, ie, that one set of perms (role) must be used for all members of that role, not  copies of the perms for each user.  To implement:

 1. in Config -\> Session set 'perms retrieval method' to getPermsByUser
 1. make a copy of the user module so you can customise UserDAO
 1. name your custom modules folder to, eg, modulesFOO
 1. set modulesFOO as the modulesOverride dir in Config -\> General
 1. in your custom UserDAO you need to comment out the code that  creates a new copy of role perms for each newly registered user. See UserDAO::addUser()

Implementing this modification means you can drop the user\_permission table.

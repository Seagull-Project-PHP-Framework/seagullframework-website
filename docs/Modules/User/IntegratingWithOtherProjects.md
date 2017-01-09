<!-- Name: Modules/User/IntegratingWithOtherProjects -->
<!-- Version: 3 -->
<!-- Last-Modified: 2008/06/05 10:35:30 -->
<!-- Author: demian -->
# Integrating Seagull with Other User Tables
[[TOC]]
## Overview
In this scenario let's say you have your user data in a table called foo_user.

 * copy the table and its contents to your Seagull database
 * look for the following line the global config file

    $conf['table']['user'] = 'usr';
 * change it to

    $conf['table']['user'] = 'foo_user';
   * or you can login as Admin, the navigate to General -> Configuration -> DB and update the value using the form.  
   * *warning*: until you make subsequent adjustments you will get errors
 * edit the Seagull mapping to the usr table (see modules/user/data/tableAliases.ini and modify the value for key "user" to "foo_user")
   * the tableAliases.ini stores the table mappings which will be used the next time you rebuild your environment
 * rename the primary key for the "foo_user" to usr_id
   * whereas Seagull lets you create any mapping for table names, this does not extend to field names, and the value "usr_id" is hard-coded in quite a few places in the user module
 * add a field "role_id"
 * rename the existing username, password and account_active fields to "username" and "passwd" and "is_acct_active"
   * if any of these fields don't exist, create them
 
## Refinement
In a typical scenario you will want to convert all your users to the role SGL_MEMBER which has an ID of 2, and create at least 1 SGL_ADMIN user with a role ID of 1.
 * update the field role_id to 2 for the whole imported table
 * update your existing admin user to a role_id of 1
 * hopefully you have your passwords stored as md5 hashes, if not create a new md5 hashed password at least for the admin user

    $ php -a
    > <?php
    > echo md5('my new password');
 * update the field is_acct_active to 1 for the whole table, providing you want all the users to have their accounts enabled
 * navigate to the user module and you should have a fully working Seagull installation but with the data from your external user table

    http://example.com/index.php/user/

## Adjusting the Permissions
By default Seagull maps a unique group of permissions to each user that is created, even if they are all the same role.  That is to say if your role FOO is associated with a collection of 23 perms identified by unique IDs, these 23 perms will be associated with each user ID in the table user_permission.  The advantage of this approach is each user's permissions can be customised, but this is at the cost of creating a lot of records.  If you have 10k or greater users, unique perms per user is not a great strategy.

You can bypass the unique permission strategy and have all users of the same role grab their set of perms from the permission table by making a config change.
 * Under General -> Configuration -> Session -> Permission Retrieval Method, supply the value "getPermsByUser"
 * this maps to UserDAO#getPermsByUser()

Save your choice, permissions will be much simpler to manage now.


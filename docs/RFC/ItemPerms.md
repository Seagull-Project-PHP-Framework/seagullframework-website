<!-- Name: RFC/ItemPerms -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/13 19:26:10 -->
<!-- Author: werner -->
# Universal Item Permissions module

*Module description:*

Create a table with this fields:
  * module_id    # Ex: publisher, shop, newsletter
  * class_id    # Ex. for shop module: shop, prices
  * item_id     # Ex 13
  * action_id  # Ex edit
  * users   #the users id separated by comma (like now)
  * roles   # the roles id separated by comma
  * grant   # allow or deny if the rule is matched

Of course for speed will use numbers for cols 1-4.

Provide a general function:
(boolean) hasPerms(module_id, class_id, item_id, action_id, usr_id = null, role_id = null);
that will return true/false if you have or not permission for that item.

Now for every item we add a rule to allow or deny access.

In the module action or module validate we check if we have the perms for performing the selected action. This is optional so only how wants it will use it. For example in list articles we can do a JOINed SELECT and list only articles that have the grant=1;

For example:
        `hasPerms('publisher','article',13,'edit',9,1);`

This will check if user 9 from role 1 has perms to edit article 13 from publisher module. Again, we use id's for publisher, article, edit.

Of course in this module we need the an admin part and to be abstracted from the framework so we can use something like addPerm(), updatePerm(), delPerm() methods in other code. BUT the hasPerms() must be the fastest method.

Maybe in the future we can delete the cols what are used for rights (from category for ex) and move all the item perms to this module.

Waiting for your replies.
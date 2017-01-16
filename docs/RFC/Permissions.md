<!-- Name: RFC/Permissions -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/13 19:35:54 -->
<!-- Author: werner -->
Currently Seagull uses a custom permissions system - there are however a number of 3rd party libaries available including LiveUser that are probably more flexible and powerful.

[LiveUser](http://pear.php.net/package/LiveUser)
[PHP Generic Access Control Lists](http://phpgacl.sourceforge.net/)

The seagull perms system has a very simple concept:
  * every manager exposes a number of actions
  * each action is mapped to a permissions constant
  * users can either access certain actions or they can't, based on their roles
  * a role is simply a collection (or template) of perms
  * when a user is assigned a role, s/he gets a copy of all perms in that role, that way individual roles can be customised
  * roles can by re-synched with their original 'master' once new perms are added to the master
  * currently a user can only have 1 role
  * when the user authenticates, an array of perm IDs is stored in the session (a datastructure with 150 elements is  <2kb in the session)
  * an organisation is a collection of users
  * each org has a default role

Recently Radek has integrated LiveUser with Seagull.  The goal of this page is to plan and prepare for its full integration with the main seagull code.  Some differences worth noting so far are:

  * a right is similar to seagull's concept of a role
  * a right can be a single perm or a group of perms and it's referenced by a constant
  * implied rights are possible, due to perm inheritance, so, eg: a user who has the right to edit a forum post also has the right to read it
  * collections of rights can be managed in terms of groups, this to me seems to conflict with seagull's concept of organisations - perhaps the concept of orgs is too limited?
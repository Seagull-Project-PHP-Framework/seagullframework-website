<!-- Name: RFC/SDN/Trac -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/08/21 12:59:22 -->
<!-- Author: werner -->
[[work in progress ~~~~]
## SDN Trac Hosting
If you want your procets hosted by the Seagull Developer Network please contact Demian Turner or AJ Tarachanowicz. They'll setup your account and you can go on using Trac (SVN, Wiki, Tickets etc...) for your projects.

[note on pricing]

### Trac Admin
After your trac account is setup you should login and go to the admin panel. Go to _Permissions_ and delete all anonymous permissions if you want everyone to view your data.

In the Admin Section you can do (surprise!) the administration of the current project.

[note: global admin per user possible or only per project?]

#### Adding new user
You can add new users on a per project basis 

[question: how to add globally?]

  * Logout
  * You should see the link _Register_. If not you have to enable registration. To do so
    * Go to ''Plugins'', ''TracAccountManager'' and enable the ''RegistrationModule'' checkbox.
  * Give in your new user and password
  * You automatically login as the new user (see below)

Newly added users have all perms of the _authenticated_ group. This is a known security issue and will be fixed in a future release of Trac. We recommend to disable registration after all users are registered. If you want to register a user later just enable it again temporarly.

Of course you have to add some permissions to the _authenticated_ group. Go to _General -> Permission_ on your admin screen. You see a block on the right called _Grant Permission_. There you can add a permission from the select box to the Subject _authenticated_. If you need the perms explained better please consult the built in Trac Help by clicking _Help/Guide_ in Trac menu and go to _TracPermissions_

### Projects
You can setup different reprositories for your different projects. Well, at the moment you cannot but the admins. So just ask AJ or Demian to add a new project for you.

You can view the list of your projects by accessing http://sdn.seagullsystems.com/trac/<yourName>/

### SVN
[how to access svn server from your IDE / editor / svn command line program]



<!-- Name: Integration/SingleSignOn -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/11/30 15:53:37 -->
<!-- Author: demian -->
# Single Sign On
[[TOC]]
## Overview
Since version 0.6.0RC3, Seagull allows you to implement a type of single sign on with other sites, this is actively used in the [FUDforum integration](/wiki:Integration/FUDforum/).

The onLogin event is now setup to support [Observers](/wiki:Howto/PragmaticPatterns/Observers/), so you can create login scripts to your other projects, register them with the onLogin event, and a Seagull authentication will simoultaneously authenticate with these sites.

The LoginMgr's onLogin event is handled by the User_DoLogin class in the same file.

The user module's config file looks like this:


    [LoginMgr]
    requiresAuth    = false
    logonAdminGoto  = default^module
    logonUserGoto   = user^account
    recordLogin     = true
    observers       = ;DoFudLogin

The DoFudLogin observer is disabled by default, to enable remove semi-colon.  To register additional observers, add them as a comma-separated list, where each name represents an observer class that can be found in seagull/modules/user/classes/observers.

## Further Reading
The process described above is a pseudo single sign on at best, for something more scalable take a look at http://openid.net/.

[[AddComment]]
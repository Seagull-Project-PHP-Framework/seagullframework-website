<!-- Name: RFC/SGL_UserRefactoring -->
<!-- Version: 10 -->
<!-- Last-Modified: 2006/10/26 20:57:51 -->
<!-- Author: demian -->
# Refactoring the User Model
[[TOC]]
## Overview
In 0.7 there will be a task to clean up the User model, it will include:

 * simplifying the base user object
 * factoring out the address data (i've already done this in a private project, just need to merge the work back in)
 * move to dynamic properties to make upgrades easier, see http://trac.seagullproject.org/ticket/765 (Steve)
 * separate out authentication and authorisation logic, see http://trac.seagullproject.org/ticket/1173 (Tamme)
 * refactor current authentication task SGL_Task_AuthenticateRequest to allow various auth backends

## Refactoring Authentication
What we need is a simple auth container that is lightweight and flexible enough to accomodate a number of backends.  If you look at the number of Horde backends (see [http://cvs.horde.org/framework/Auth/Auth/]) that's what we should be aiming for.

In terms of design I'd suggest we look at PEAR::Auth, Horde::Auth, and possibly Solar's implementation, and see what is the minimum amount of code we can get away with.

I'd suggest the File driver should be the default, so when a minimal seagull install is setup, no user module would be required, the admin's passwd would be stored, 1 way encrypted, in a file in seagull/var somewhere.

### What i don't like about PEAR::Auth (29kb)
This class takes the kitchen sink approach and is poorly focused on the simple task of authentication.  Methods which don't belong there are
 * removeUser (user related)
 * addUser (user related)
 * listUsers (user related)
 * setSessionName (session related)
 * login @%*! this  one is the worst and includes html to create a login form!

### Review of Horde::Auth (42kb)
That's one fat bastard - would be nice to get something in around 10kb.  This package supports many more drivers than PEAR, it would be great to keep same/similar API and just recycle their auth drivers.
 * i think the API and method names of this package are better
 * if we supported fewer encryption types that would make the class a lot lighter
 * these guys also mix up user management (add, remove user, etc) in auth which I don't think belongs
 * genRandomPassword() could be done more efficiently with Text::Password which we already use
 * this class has all the login screen stuff too which we don't want
 * hook stuff we don't need since we know about the observer pattern ;-)
 * isAdmin() should probably be removed since that's authorisation and we want to focus on authentication

### Proposed API for SGL_Authenticator


    class SGL_Authenticator
    {
        var $_authCredentials = array();
    
        function _loadStorage() {}
        
        function &_factory($driver, $options = '') {}
        
        function getDriver() {}
        
        function authenticate($userId, $credentials, $login = true, $realm = null) {}
        
        function _authenticate($userId, $credentials) {}
        
        function isAuthenticated($realm = null) {}
        
        function setAuth($userId, $credentials, $realm = null) {}
        
        function clearAuth($realm = null) {}       
    }


various comments:
 * PEAR::Auth has better use of template pattern, let's use that
 * Horde::Auth has nice support for composite auth, maybe we can integrate in future


### Comment of Tamme (07/09/2006)
A user consists of the following
 * an identity
 * a set of granted permissions
 * a set of personal preferences (if absent, a copy of default preferences)
 * optionally additional stuff like address, zip code or app specific data
For me, attempting to do completely without user module sounds curious since managing users, permissions and preferences is a must-have. Rather we should try to decouple and encapsulate as much as possible to allow for light-weight user handling as well as complex scenarios. In other words, logging in an admin via password file is still a job for the user module since an admin is a user as well. Maybe I misunderstood something so any insight would be appreciated :)

Btt, I'd like to divide user management into four decoupled parts: authorisation, authentication (permissions), preferences and contacts (optional). Accessing any of these areas will be done over a central composed user object, thus it'll be easy to replace any of these parts or add new ones. This will increase overall complexibility but it will provide great flexibility as well.

I'm looking forward to read your comments about my overall intentions. If things go well I'll provide some dirty code soon.

### Comment by Demian (11/09/2006)

> Maybe I misunderstood something so any insight would be appreciated 

It's much simpler than you think, the object is to be able to distribute Seagull with as few as possible.  It must still run, ie, render a default page, and render the admins screens used for loading modules. 

The user module could easily be removed because authentication (pls don't confuse this with authorisation) currently only happens in the LoginMgr which could be extracted from 'user' and moved to 'default', provided authentication was handled by SGL_Authenticator and the file backend was set as default.

That way a user could install a minimal Seagull, and only install the user module if required.

Regarding composing a user object, I actually think the current Data Access model is more flexible, this can also be used to compose a user object.

We can chat on irc if you like, code examples would help make the discussion clearer.

### Comment by Randy (24/10/2006)
Reference to Horde comments above:

> it would be great to keep same/similar API and just recycle their auth drivers.

It sure does look like LiveUser Authentication would support these drivers with very little adaptation.

> these guys also mix up user management (add, remove user, etc) in auth which I don't think belongs 

After looking at some of the Horde methods, do you think they discovered different access methods also required different user management methods?  

For instance, here's an excerpt from the Cyrus SQL driver file: "Most of the functionality is the same as for the SQL class; only what is different overrides the parent class implementations."  As it turns out, the user management methods are in the file along with the connect() method.

A quick look at the ldap driver will show ldap specific mechanisms in place in the user management methods.  These include
(dn, cn, sn)s and shadow update lookups, etc.  All very specific to ldap and not likely very easy to generalize. 
<!-- Name: Misc/Security -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/08 21:05:25 -->
<!-- Author: trac -->
FIXME: Look over new file... 

Seagull uses standard PHP sessions which propagate persistence of user data using cookies by default.  The PHP engine automatically detects whether the client returns healthy session cookies, if not the session is propagated in the URL.  Anti session-hijacking measures are in place to ensure the user session can not be compromised.  

Seagull works identically whether or not end users have cookies enabled in their browsers.

Any module in the application can be set to require authentication by setting the `requiresAuth` flag to true on a per-page basis.  Once users are authenticated, finer grained permissions can be controlled by testing for group membership.

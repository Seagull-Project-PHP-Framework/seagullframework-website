<!-- Name: RFC/AdminGui -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/04/03 16:05:12 -->
<!-- Author: werner -->
# Admin Gui for other roles
[[TOC]]

## problem
I want to create some kind of little-admin role with admin access only to a given module. And of course i want to use the great admin gui for them.

## current way of adding admin gui to other roles
ATM you have to tweak in /lib/SGL/Tasks/Process the class SGL_Process_SetupGui to allow other roles beside admin to see the admin gui.


    // first check if userRID allows to switch to adminGUI
            if ($userRid == SGL_ADMIN) {
                $adminGuiAllowed = true;
            }

This means you have to hack in a sgl lib file what i don't like

## proposal
How about creating a perm "allowAdminGui" which is checked? So you can add this perm to a role or user which is allowed to see the admin gui.

Is a perm the right way of doing this? Another way could be to allow some kind of flags or preferences for a role.


<!-- Name: RFC/Modules/Navigation2 -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/10/27 14:54:44 -->
<!-- Author: mstahv -->
During the first half of 2006 navigation was improved a lot by omniton. IMO navigation things aren't still at as good level as core feature like this should be. Solution is still quite complex (for extension or modifying) and big heavy from performance POV. To start refactoring process I made some prototyping on project called Navigation2. Patch will be here.

I dumped the old solution and db totally, not to inherit its complexity. Upgrade script will be must if decided to continue this project.

Current prototype really is a prototype, don't expect it to be ready for anything.

 * no caching
 * bad programming style
 * not even manager to modify navigation tree (make changes manually to db)

## Pros in this prototype

 * it works to some extend
 * adds nice/looking/urialiases/that/allow to use slashes
 * uses nested set querys (read one query for whole tree) without heavy SGL or PEAR NestedSet classes (good performance and simple table)*
 * performance propably good in finalized solution also, even without caching

## how-to test SGL navigation2 prototype

1. apply patch

2. don't care about error (it is from uristrategy)

3. Install navigation2 module ( you will get no such a table error, but don't care about that install is propably still ok)

4. configure navi blocks to use navigation2, rootnode for admin is now 2 and 1 for usermenu

5. Test:

 * aliases/with/slashes from usermenu
 * performance and query count vs. current implementation

Waiting for comments and requirements, for Navigation2

I don't have any resources for this during next weeks, so if anybody feels inspired - feel free to continue.

Requirements:

 * O(1) DB query during tree building even with very complex navitree
 * low loc in files needed for every request
 * low execution time in required files for every request (correlates quite a lot with the above in php)
 * API for other modules to handle navigation nodes (like from custom CMS modules )
 * navigation extension (like what omniton implemented in current navigation system)
 * SGL core loosely coupled to new solution

<!-- Name: RFC/MultipleSitesWithOneDB -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/08/16 20:49:16 -->
<!-- Author: werner -->
This RFC is for running multiple Sites with one Seagull installation which can share content and can be administrated from one backend.

### Problem
I run a platform about a town with much information about this town and what you can do in neighbour towns. Now i want to start a platform for neigbour town but don't want to input every artice again, but a single admin interface where i can do admin tasks for both / all sites.

### Solution

  * Implement site_id param to:
    * user (which user has access to which site, which site is "view only" or can you login to this site)
    * navigation (for better cloning of nav trees)
    * categories (so i can have one category tree for all sites, but single categories only on one site)
    * blocks
    * articles / items (which article should be shown on which site.

  * Create a siteMgr where you can make site specific settings
  * Site specific cache dir
  * Some params in conf.ini should be global (for all sites, e.g. db data) and globally manageable, some should be specific (site title, theme...)

### Comments
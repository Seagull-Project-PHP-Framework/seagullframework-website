<!-- Name: FAQ -->
<!-- Version: 9 -->
<!-- Last-Modified: 2006/03/29 00:55:45 -->
<!-- Author: demian -->
# Frequently Asked Questions
[[TOC]]
[[SubWiki(FAQ)]]

# About Seagull
## Where does the name Seagull come from?
I was in San Francisco for the PHP conf a couple of years ago and did the ferry around Alcatraz, and they have some neat seagulls out there, they hover about 10 ft off the deck of the boat, about 20 of them, coasting along with it and really twisting their necks to get a better view of the passengers.  I found this really interactive - for a bird - and i think a framework should be a lot about interactivity, getting lots of feedback from the various resources you're consuming, being able to discover services easily, etc ...


# General
## How can I delete the stuff from the /var/ dir on shared hosting accounts?
  * Easy way: ask your admin to delete this dir ;)
  * Elegant way: 
  If you add your IP to the $GLOBALS['_SGL']['TRUSTED_IPS'] array, then you can blow away the config file by issuing the following:
   
  _www.example.com/index.php?deleteConfig=1_
  
This will revert you to a clean setup and return you to an installer screen that won't delete your log files or caches.  There are some tools in the Maintenance Mgr that delete various caches without requiring webserver perms.

## Why does Seagull use nested sets instead of adjacency lists?
  * Nested sets: provide the capability to answer hierarchical queries with standard SQL syntax. In this model, the global position of the node in the hierarchy is "encoded" as opposed to an adjacency list of which each link is a local connection between immediate neighbors only.
  * Adjacency lists: the more typical node_id related to parent_node_id structure

Seagull uses nested sets because they are more flexible, the queries more efficient, and they afford the ability, eg, to reorder nodes at any level in the hierarcy.

To get an idea of the difference between the two read this article: http://www.sitepoint.com/article/hierarchical-data-database

## I get logged out unexpectedly
I moved my setup from working on a localhost setup to a domainname setup. Now I can barely login for 1 minute before being kicked out.  Quite possibly a cookie is being shared between your local install and live one - remedy for this:

  * delete all your local cookies for which ever browser you're using
  * in live site, delete all session files in _seagull/var/tmp/*_

If there is the option in `Configuration`, try to give your live session
name a different name to the default SGLSESSID.  Currently this is under

`Configuration > Cookie options > name`


# Modules
## Publisher
### What's the difference between Static HTML Article and HTML Article?
See Modules/Publisher for more information.

### I don't want Static HTML Article to show the title
Go to _lib/SGL/SGL_Item::generateItemOutput()_, and modify the article's dynamic 
part you want changed, in this case the 'title' case for 'Static Html Article' which is 
ID 5:


            case 5:                //   template type = static HTML article
                switch ($fieldName) {
                case 'title':
                    $outputHTML = ''; //don't show title 
                break;
                case 'bodyHtml':
                    $outputHTML = $fieldValue;
                    break;
                }
                break;
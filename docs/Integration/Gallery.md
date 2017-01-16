<!-- Name: Integration/Gallery -->
<!-- Version: 14 -->
<!-- Last-Modified: 2006/11/30 15:52:33 -->
<!-- Author: demian -->
# Gallery2 Image Gallery
by Matti Tahvonen and revised on 24 Aug 06 by Karl Tiedt.

## Introduction
Following these instructions you will be able to seamlessly integrate the well-known [Gallery2](http://gallery.menalto.com/) application into Seagull.

## Install
 1. Unzip gallery-2<version>-typical.tar.gz in seagull/www/
 1. Follow detailed installer instructions
 1. Create g2data folder either inside your gallery2 folder but preferably above webroot, ie seagull/g2data
 1. For the 'Admin User Setup' screen, name your admin 'g2_admin' to avoid username conflict (beware max length for username)
 1. Complete install as normal
 1. setup a test gallery with some images
 1. login to Seagull as an admin user
 1. register the gallery2 module
 1. customise seagull/modules/gallery2/conf.ini to conform with your setup (an example copy is provided below.)
 1. request the gallery screen, uri will be something like http://localhost/seagull/www/index.php//gallery2/

This example is from my test setup that is accessible from http://localhost/sgl/www/ for the seagull webroot. In its current form, the Gallery Module Mgr's options seem poorly named... just a quick glance at the below options should make a "base_path" option handy... 


    [Gallery2Mgr]
    requiresAuth=false
    g2Dir=/var/www/seagull/www/gallery2
    embedUri=/seagull/www/index.php/gallery2/action/list/
    g2Uri=/seagull/www/gallery2/
    loginRedirect=/seagull/www/index.php/user/login/

g2Dir is the physical path to your gallery2 files where you unzipped em.

embedUri is the address used to display images inside of Seagull.

g2Uri, I'm assuming is the path to normally view G2... not very sure on that one.

loginRedirect is pretty self explanatory... 

Have fun!

## Extra - clean URLs
 * difficult part; may break everything
 * enable clean url's on embedded side
 * replace created .htaccess with:


    <IfModule mod_rewrite.c>
        RewriteEngine On
        RewriteBase /
        RewriteRule ^index.php/gallery2em/action/list/v/(.+)$  index.php/gallery2em/action/list/?g2_view=core.ShowItem&g2_path=$1   [QSA,L]
    </IfModule>
 
## Download
  * http://gallery.menalto.com/downloads

## Todo
  * make custom header files (css, javascript) 0.5.x alike

## Links / Resources
  * http://codex.gallery2.org/index.php/Gallery2:Integration_Howto
  * http://gallery.menalto.com/node/24126
  * http://codex.gallery2.org/index.php/Gallery2:Integration:Available_Integrations

[[AddComment]]
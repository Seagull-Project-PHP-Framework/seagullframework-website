<!-- Name: Howto/EnableMaintenanceMode -->
<!-- Version: 1 -->
<!-- Last-Modified: 2007/07/02 17:57:47 -->
<!-- Author: demian -->
# Maintenance mode
[[TOC]]
## Requirements
 * >= Seagull 0.6.3

## How to enable Maintenance mode
 * login as admin
 * select General -> Configuration
 * set 'Maintenance mode' to Yes
 * set and admin secret key that will allow you to enter a URL parameter to bypass the maintenance mode screen
   * eg, set 'Admin key' to 'foo'
 * save your settings
 * hit the Seagull logo to get the front end view
 * logout

Maintenance mode is now activated.

## How to disable Maintenance mode
 * enter any Seagull URL and pass your secret key as the value of the parameter 'adminKey'

    http://foo.example.com/index.php?adminKey=foo
 * you will now see the default Seagull screen, and can login using the standard form

## Problems
If you run into problems, or have to deal with an installation where the admin forgot his/her key, simply edit the seagull/var/<mydomain>.conf.php directly and unset maintenance mode under the [site] group.

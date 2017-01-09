<!-- Name: Modules/GoogleMaps -->
<!-- Version: 4 -->
<!-- Last-Modified: 2007/03/03 11:38:24 -->
<!-- Author: demian -->
# Google Maps Module
[[TOC]]
## Introduction
The Google Maps module allows you to use the Google Maps API to map users that register on your SGL site.
## Geo Coding
Geo Coding is the process of converting a typical street address into a latitude/longitude pair. The lat/long pair is necessary for mapping with Google Maps.

Because Google's geocoding API will not work with any UK or Chinese addresses, the decision was made to use Yahoo's Geo Coding API [http://developer.yahoo.com/maps/rest/V1/geocode.html] instead.

Geo Coding is available in the Google Maps module via the following methods.

Observer in user module: GetUserGeoCode.php (disabled by default)
 * real time geocoding. when user signs up, attempts to get lat/long pair from submitted address

Admin accessible manager: GeoCodeMgr.php
 * allows you to select a range of user id's to geocode. good for geocoding users that have already signed up and don't have lat/long pairs.

The lat/long pairs for users are stored in the table 'googlemaps_user_geocode'

## Requirements
 * requires a working install of latest Seagull (>= 0.6.2), preferably with PHP installed as mod_php
 * a google maps api key

## Install
 * install the googlemaps module using the module manager.
 * enter your google maps api key in modules/googlemaps/conf.ini (get one here [http://www.google.com/apis/maps/])
 * view the map ([http://example.com/index.php/googlemaps/map/])

## Demo
 * you can view registered Seagull users here: http://seagullproject.org/googlemaps/map/
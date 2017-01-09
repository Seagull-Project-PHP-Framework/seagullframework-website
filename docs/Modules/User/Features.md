---
layout: page
title: User Module Features
permalink: /Modules/User/Features.html
---

<!-- Name: Modules/User/Features -->
<!-- Version: 3 -->
<!-- Last-Modified: 2007/01/04 03:18:16 -->
<!-- Author: demian -->
# User Module Features
* TOC
{:toc}

## General
 * Easy to use admin interface
 * GUI internationalised, including error messages, 23 languages supplied by default
 * Basic CSV user import functionality
 * Support for table prefixes in the db
 * Easy browsing for huge user sets, ie tens of thousands

## Roles and permissions
 * Ability to create unlimited roles comprised of unlimited permission sets
 * Roles can be duplicated to ease creation of new permission sets
 * User perms can be different from a Role's perms, and can also be re-synched at a later date
 * When adding new modules, permissions can automatically be detected and added to the system
 * Orphan perms can be removed when modules are removed

## Registration
 * Registration can easily be enabled and disabled
 * Template supplied with a generic set of attributes and field validations, easy to customise to your project's needs
 * Accounts can be set to auto-enable, or auto-login
 * Notifications sent when account status changes, also sent to admin and user when registration is completed

## Logins
 * Logins can optionally be recorded and details browsed on a per-user basis
 * Login object accepts listeners that can easily be linked to respond to custom events
 * Listeners can be setup to emulate single sign on to multiple PHP apps (forums, wikis), or to register statistical information
 * Custom redirects after login are easy to setup

## Accounts
 * Generic 'my account' template supplied, easy to customise
 * Passwords can be reset by users themselves, and by admins
 * Security questions stored for safe password retrieval
 * Accounts can be disabled by admins

## Sessions
 * Various security precautions in place to defend against session highjacking and bot attacks
 * Nuisance users can be blocked by IP without having to setup firewall or Apache rules
 * Session timeouts are customisable, and on re-login, correct browsing sequence reinstated

## Preferences
 * Sometimes referred to as 'settings' or just 'personalisation'
 * Each user can be assigned unlimited preferences, easily customisable

## Search
 * Advanced user search feature
 * Possibility to search against most user attributes, included range of registration dates and user statuses

## Organisations
 * Users can optionally be grouped into organisations that can be hierarchically nested
 * Each organisation can have default preferences and theme, which can be inherited by all users created in that organisation
 * Organisations hierarchies can be reordered
 * Organisation types can be created

## Coming soon
 * Integration of LiveUser library, can be installed as an optional plugin
 * Support for multiple authentication backends, ie LDAP, Active Directory, Yahoo, Google, etc
 * Support for multiple user persistence backends
 * Support for custom Object Relational Mappers
 * User customisations will be easier to maintain through upgrades
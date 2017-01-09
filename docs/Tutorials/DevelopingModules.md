---
layout: page
title: Developing Modules
permalink: /Tutorials/DevelopingModules.html
---

<!-- Name: Tutorials/DevelopingModules -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/06/06 02:54:10 -->
<!-- Author: demian -->
# Developing Modules
## Overview
There are a few tricks that will help speed up developing new modules:
 * use module generator to create scaffolding
 * create your module's navigation with navigation data file, use 'rebuild seagull' to process it
 * correctly setting up conf.ini, set requiresAuth to false for dev purposes
 * create your schema, and default and sample data files
 * as your adjust the schema adding new fields, regenerate dataobjects
 * if you create create new associations between dbdo's, these will be updated use regenerate dataobjects too, use dataobjectLinks.ini for this
 * to reset your default dataset, try 'rebuild seagull' which drops and recreates your database
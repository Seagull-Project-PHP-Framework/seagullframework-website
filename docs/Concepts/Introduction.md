---
layout: page
title: Seagull Concepts
permalink: /Concepts/Introduction.html
---

<!-- Name: Concepts/Introduction -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/07/11 23:33:20 -->
<!-- Author: demian -->

# Seagull Concepts
## Introduction
The Seagull project is a set of classes and suggested development approach that aims to make writing programs faster and easier.  The belief is that by providing a methodology, and the tools required to handle arbitrary resources and workflow demands, the programmer can dedicate more time and energy to solving the key problems at hand, whether they are refining algorithms, imposing domain-specific logic, creating an attractive and user-friendly user interface, and so on.

Many PHP users find themselves tasked with solving problems in the web domain.  Much of the code and tools distributed with the Seagull package indeed caters to these needs.  But the framework works equally well with commandline input, and therefore can be quite useful as part of a larger solution or chain of programs.

## Overview
The following types of operations are performed for each program invocation, described here from a high level point of view:
 * the program is initialised by executing an arbitrary list of tasks
 * the calling arguments, or request, are analysed and stored
 * a configurable set of filters are called on the program input (and later output)
 * the event implicit in the request is processed
 * observers listening to that event are triggered
 * the output is returned to the caller

## Tools
A range of tools required for typical application processing tasks are included with Seagull, here is an example of what's provided:
 * a taxonomy builder and nested set helper
 * configuration management
 * database abstraction
 * object relational mapping
 * object delegation
 * http upload/download helpers
 * an emailer
 * error management
 * filter management
 * front and application controllers
 * locale handling
 * configurable content objects
 * subject/observer mechanism
 * a parameter handler
 * a registry
 * request abstraction
 * session handling
 * string manipulation
 * task management
 * db and file-based translation tools
 * configurable URI parsing
 * a wizard helper

To use these tools effectively you first need to understand the building blocks of a Seagull application, explained in [Concepts/ModulesManagersAndControllers][1].

[1]:	/Concepts/ModulesManagersAndControllers.html
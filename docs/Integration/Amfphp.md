<!-- Name: Integration/Amfphp -->
<!-- Version: 3 -->
<!-- Last-Modified: 2007/01/10 21:41:39 -->
<!-- Author: demian -->
# AMFPHP
[[TOC]]

## What is AMFPHP?
_from http://www.amfphp.org/_

Amfphp is an RPC toolkit for PHP. It allows seamless communication between PHP and:
 * Flash and Flex with Remoting
 * JavaScript and Ajax with JSON
 * XML clients with XML-RPC
	 
	 
## What is RPC?
_from http://www.amfphp.org/_

RPC (Remote Procedure Call) is a way to communicate data between a client and a server. You call a method on a local object with various parameters, set a callback, and receive a result. You don't have to worry about how you're going to send and receive the data. The implementation details are abstracted away so that it looks as though you're calling a local method.

## Requirements
 * requires \>= Seagull 0.6.2

## Installing
  * Download - http://www.amfphp.org/
  * Decompress and put amf-core in <install-root>/lib

## Integration
To create a service, put the class in <module>/classes/amfservices/<service>.php

For example, if you want to call a service Example in default module, put the file Example.php in default/classes/classes/amfservices/Example.php
then in flash use the gateway url as:


	http://seagull/index.php/default/
 

## To Do
 * Create a ServiceBrowser module.
 * This installation has only been tested with Flash and Flex Remoting
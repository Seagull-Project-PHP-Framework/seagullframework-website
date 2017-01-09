<!-- Name: Modules/RandomMsg -->
<!-- Version: 3 -->
<!-- Last-Modified: 2005/11/15 15:31:06 -->
<!-- Author: aj -->
[[TOC]]

## Random Message Module

Allows you to create a list of messages and display them randomly (fortune).

### User Section

  * A block shows a random message

### Random Message Manager

You reach the Random Message Manager via `'Manage Modules -> Random messages'`. It allows you to:

  * add new messages/quotes using the text input field (multiple messages separated with a carriage return)
  * upload new messages/quotes using a plain textfile
  * remove messages/quotes

_Create your quotes *before* you activate a quote block!_

## Block Manager

You can create a block (using the [Block Manager](/wiki:Modules/Block/)) with a random message using _RndMsgBlock_ in the `name` field of the block.

(Another way to use the random quotes is to add a custom function (just copy the functionality from the random-message-block) which you use in your templates.)

_Note: Unless there is at least one quote in the database, you currently get some nasty-looking PHP errors when you try creating a _RndMsgBlock_ block._
<!-- Name: RFC/Modules/Comment -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/03/17 15:26:09 -->
<!-- Author: mstahv -->
## Universal Commenting module

## Table structure:
 


    CREATE TABLE `comment` (
      `comment_id` 	int(10) unsigned NOT NULL,
      `parent_id` 	int(11) default NULL,
      `seagull_uri` varchar(255) NOT NULL,
      `usr_id` 		int(11) NOT NULL,
      `guestname` 	varchar(32) NOT NULL,
      `subject` 	varchar(255) NOT NULL,
      `comment` 	text NOT NULL,
      `timestamp` 	timestamp NOT NULL default CURRENT_TIMESTAMP,
      PRIMARY KEY  (`comment_id`),
      KEY `parent_id` (`parent_id`),
      KEY `module` (`module`),
      KEY `module_2` (`module`),
      KEY `seagull_uri` (`seagull_uri`)
    )


Descriptions of non-selfexplaining fields:
seagull_uri identifies under which page comments will be added
parent_id comment to which this comment is reply to
usr_id set from session
guestname if public commenting allowed this will be used. Otherwise filled from usern

## INTERFACE FOR MODULES

Mostly via blocks:
 - CommentListing
 - New comment

Replys and saving comments from "New comment block" via "Comments" modules CommentMgr

Also "native" interface CommentContainer something like this:


    if(this->conf['commentsEnabled']) {
    	// create CommentContainer for uri
    	$commentcont = new CommentContainer('simpleblog/post/' . $id);
    
    	// if just want to display count on "summary page"
    	$output->countOfComments = $commentcont->getCount();
    	
    	// or grab comments and format/manipulate them self
    	$aComments = $commentcont->getAll();
    	// get all in tree childs like this: $comment->children[]
    	$commtTree = $commentcont->getTree();
    }
    

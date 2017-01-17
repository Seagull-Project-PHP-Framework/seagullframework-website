<!-- Name: Community/IRC/SeagullBot -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/05/03 15:21:54 -->
<!-- Author: aj -->

# Seagull Bot
[[TOC]]
## How to set it up
### Requirements
 * PEAR::Net\_SmartIRC (atm CVS version)
 * PEAR::mdb
 * PEAR::HTTP\_Request

### Config
The file that should be modified to set up the bot is *config.php*
The most important options are:
  * $config['channels']
  an array containing the name of channels that you want the bot to login to

  * $config['nick'], $config['ident'], $config['alt\_nick'] and $config['real\_name']
  these properties' purposes are self-describing, setting the nick, ident and so on

  * $config['irc\_server'] and $config['irc\_port']
  use these for your channel's IRC server and the port that it uses (like: irc.freenode.net and 6667)

If you want that identify your IRC bot to the NickServ in FreeNode add this line to the phpbitch.php after $irc-\>login()

	$irc->message(SMARTIRC_TYPE_QUERY, 'NickServ', 'identify password');

Now simply run phpbitch with: 


	$ php phpbitch.php

and that's it!

### Setting-up bot users
Make a database for phpbitch and set proper settings in config.php for database connection (phpbitch uses mdb for its DBAL) and now import two sql files (phpbitch.sql & update\_mysql.sql) to your newly created database.

In the users table add nickname, ident and host to register a new user, for example for *n=demian@81.1.83.225* you should fill nickname with *demian*, ident with *n=demian* and host with *81.1.83.225*.

Now you should add this new user to the *users\_levels* table too and give him/her a proper ACL: in the user field putthe nickname, in the channel set what channels you want to give that certain ACL to the user (\*\*\* for all channels) and finally into the level set his/her ACL (*5* for a _bot_, *4* for a _Master_, *3* for an _Operator_, *2* for a _Voice_, *1* for a _Friend_ and finally *0* for a _Normal_ user).

---- 

## Usage

### Commands

#### Basic
  * !help - Displays this help.
  * !time - Displats current time on my server.
  * !uptime - Displays how long bot has been running.
  * !search $keyword - Querys bot's brain for $keyword.
  * !up/!opme - Ops/voices you depending on level.
  * !down - removes your op/voice depending on level.
  * !op $nick - Grants operator status to $nick (lvl3 required).
  * !deop $nick - Revokes operator status to $nick (lvl3 required).
  * !topic $topic - Sets topic to $topic (lvl2 required).
  * !invite $nick - Invites $nick to current channel (lvl2 required).
  * !kick $nick - Kicks $nick from current channel (lvl2 required).
  * !ban $nick - Bans $nick from current channel (lvl2 required).
  * !kb $nick - Kick-bans $nick from current channel (lvl2 required).
  * !die - Disconnects bot from the network (lvl4 required).
  * !learn $keyword $response - Learns $keyword as $response. (lvl 3 required).
  * !forget $keyword - Erases $keyword from brain. (lvl 3 required).
  * !join $channel $key - Joins $channel with $key (lvl 3 required).
  * !part $channel - Leaves $channel (lvl 3 required).
  * !adduser $nick $ident $host - Adds user to bot's userlist (lvl 4 required).
  * !addlevel $nick $channe $level - Adds the level for user (lvl 4 required).
  * !deluser $nick - Removes user from bot's userlist and all his levels (lvl 4 required).
  * !dellevel $nick $channel - Removes the level of the user (lvl 4 required).
  * !cvs-checkout - rebuilds the bot from CVS and restarts it (lvl 4 required).
  * !mastah - all bots will reply if they are the current bot-master or not (lvl 4 required).
  * !who - looks up a user in the master bots databases.
  * !who\_all - looks up a user in all bots databases.




#### Trac module
*Note:* You should edit _modules/trac.php_ and set *$baseurl* to your Trac install's root URI with trailing / (for example: http://trac.seagullproject.org/)

  * !roadmap \<_roadmap_\>
  Will return the Active and Closed tickets with progress percentage of that roadmap

  * !ticket \<_ticket\_number_\>
  Will return status and description of _ticket\_number_

  * !changeset \<_changeset_\>
  Will return timestamp, author and reason of the _changeset_

#### Brain module

  * !learn \<_keyword_\> \<_definition_\>
  Will learn the *definition* of the *keyword*

  * !forget \<_keyword_\>
  Will forget about that *keyword*

  * !search \<_keyword_\>
  Will return the definition of *keyword* if it's in the brain database

  * !tell \<_user_\> about \<_keyword_\>
  Will tell *user* about that *keyword* as /msg

## Credits
Thanks a lot to Amir Mohammad from the Jaws project who set this up for us.
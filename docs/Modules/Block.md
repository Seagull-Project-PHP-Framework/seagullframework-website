<!-- Name: Modules/Block -->
<!-- Version: 6 -->
<!-- Last-Modified: 2006/10/11 23:15:46 -->
<!-- Author: demian -->
[[TOC]]

## Block Module

Use the `'Block'` module to easily add content blocks to your site.

### User Section

  * shows various blocks of content via a PHP file

### Admin Section (Block Manager)

You reach the Block Manager via `'Manage Modules -> Blocks'`. It allows you to:

  * add new entries
  * edit entries
  * change placement of blocks


You can add new block types by adding a new class to:

_modules/block/classes/blocks_

When editing the block information, the `'Name'` parameter of the block corresponds to the .php file you want to use for content rendering.
See /UsingTheBlockmanagerToCreateNewBlocks for a complete description of the process.

#### Configuration

You can enable/disable the blocks generation in the main configuration file :


    [site]
    blocksEnabled=true


### Various Blocks

Current valid blocks are (SGL 0.4):

  * RndMsgBlock (see [RandomMsg Module](/wiki:Modules/RandomMsg/))
  * SiteNews (provides lastest news)
  * LoginBlock (provides the loginform)
  * CalendarBlock (a nice JS calendar)
  * DirectoryNav (for Categories)

And various samples (see directory _modules/block/classes/blocks_/ for details).

Current valid block-classnames are (SGL 0.6):
  * Default_Block_Calendar
  * Default_Block_Debug
  * Default_Block_LangSwitcher
  * Default_Block_ThemeSwitcher
  * Navigation_Block_Breadcrumbs
  * Navigation_Block_CategoryNav
  * Navigation_Block_Navigation
  * Publisher_Block_Article
  * Publisher_Block_Html
  * Publisher_Block_RecentHtmlArticles
  * Publisher_Block_SiteNews
  * User_Block_Login
  * User_Block_OnlineUsers (you have to set the session-handler to "Database" and use "Extended Session" in Configuration > Session)


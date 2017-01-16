<!-- Name: RFC/Blocks -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/13 19:08:08 -->
<!-- Author: werner -->
# RFC Blocks
[[TOC]]

## General


An brief outline of phpSlash block functionality from [/PeterCruickshank PeterCruickshank], a starting point for a Seagull block RFC:

Block admin is covered here:
[http://php-slash.org/article.php3?story_id=29&topic_id=39&section_id=8#ss5.8]

Blocks are self-contained chunks of HTML. phpSlash has evolved to treat  all elements of a page as blocks (including header, navbar and main  panel) - but the basic block mechanism has been stable for a while now.

Blocks are linked to one or more sections - blocks generate their own  content (eg list of article titles or RSS headlines, content pulled in  from a template file, or from HTML input directly on the admin form.  Blocks can be linked to an arbitary URL. Other properties include:
  * title
  * type - defines the Rendering class that is used for creating block  content
  * Expire length (0 for no caching)
  * Column (Left and Right, but also Top, Center, Bottom)
  * Parameters (eg section=portfolio&topic=books) - to allow tailoring of  block content
  * Order
  * Framing template to wrap around block content (allowing eg choice of  whether block title is shown)
  * Perms - what class of user can see the block

The same block-type can generate different content depending on the  paramters passed to it - creating nice flexibility

When it comes to displaying a page:
The page-display code creates an instance of the block class that pulls in all blocks linked to the current section, sorted by column and order, checking whether this user has rights to see the block.

By default, the pre-generated content is used (this is stored in the DB along with the other block properties - not very clean I dont think -  caching could easily be file-based), but if the block has expired, a  new instance of the rendering class is instantiated, and used to create updated content, which is saved for the next time.

Once all the blocks are pulled in, sorted and generated, the page is output.

The implication is that it's the top level code uses the current section_id to decide how many columns a page has and what's in them, by checking whether none, either or both of the Left and Right columns  are used. AFAICT, Seagull currently works by the modules defining how what columns they will let display. Not sure which is best... 

## Corresponding Block for Top Navigation
The code's a bit of a hack:

The sub-nav 'block' is generated directly by an adapted version of SimpleNav. I fixed my TwoPartNav::_toHtml() to return an object with mainmenu and submenu html, so BC is broken unless default/banner.html is tweaked to allow for this.

blocksLeft.html then displays navigation.submenu if it's present. Of course, it means that the submenu doesnt appear if there are no other left-blocks, a slight issue in the admin parts of the site.  It would be nice if the submenu inserted itself as a left block where needed. 

## Main Goals
Just to pull out some of the main goals of the overhaul:

  * remove dependency on quick form, makes datahandling quit messy
  * correct broken assignment to single/multiple sections
  * blocks need to be able to take parameter
  * define block types: phpkitchen's geeklog has a good basic structure: 
    * php blocks
    * textual static blocks
    * RSS blocks
  * remove dependency on hard-coded classes for each block
  * caching for blocks would be great
  * Users should be able to switch on and off certain blocks which they are allowed to (e.g. a block with commercials should not be a choice)
  * Clicking on "enable" disables block with one click (and vice versa). So you don't have to acces the edit form and scroll down to the checkbox. [/WernerKrauss WernerKrauss] /19.09.2005 12:21/

## Comments
  * ATM blocks are only available if there is a navigation-entry available. maybe it's possilbe to add blocks to modules, too [/WernerKrauss WernerKrauss] /21.01.2005 11:09/
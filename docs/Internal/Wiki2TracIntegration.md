<!-- Name: Internal/Wiki2TracIntegration -->
<!-- Version: 24 -->
<!-- Last-Modified: 2006/12/31 00:31:52 -->
<!-- Author: demian -->
# A short Howto look over freshly imported wiki pages

  1. Choose a cluster to look through
  1. Create the parent page (if it's now in there). A good example is [wiki:Code]
  1. Check if everything is ok. Have a look at new wiki syntax of trac.
  1. If everything is fine, remove the "FIXME: Look over new file..." flag at the beginning of the wikipage
  1. go to every page in this cluster and check for correctness...

## Headings

every page should start with a H1 heading:


    = Heading = (Be sure to make sure there is a space before and after the heading.)
so some (very old) pages headings have to be reorganized.

the content of this H1-heading is a short "preview" in the subwiki makro (automatic overview list of a cluster)

## TOC
Add a Table Of Contents if needed:


    [[TOC]]

## Code Blocks
Automatic converting could also have damaged some code blocks. In doubt copy them manualy from the old wacko page http://seagull.phpkitchen.com/docs/

## Url

### Wiki Link

    [wiki:Internal/Wiki2TracIntegration]

### Displaying URL without a link

    !http://trac.seagullproject.org


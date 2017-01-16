<!-- Name: RFC/LookAndFeel/Testing -->
<!-- Version: 37 -->
<!-- Last-Modified: 2006/02/27 16:46:06 -->
<!-- Author: rm -->
## Testing the admin interface on various OS/browser configurations

The new admin GUI is in the process of being released. I would like it to be tested on a maximum of different OS/Browsers.

Could you please enable adminGui in <server-name>.config.php and give me a feedback on your environnement. I'm gonna put screenshots of the expected layout of various admin tasks pages for you to compare with the render you have on your machine.

## Global feedback

 * What do you think of global look and feel of adminGUI v1 ?

 * Do you have display bugs to report ?

 * What improvements would you suggest from an ergonomics point of view ?


## Displaying bugs

This is the place. Please put your comments in the appropriated OS/browser field (please indicate exact version of Browser):

|| Browser / OS || *Linux* || *Windows* || *Mac* ||
|| *Firefox* || optionLink-is-evil.jpg || || ||
|| *IE* || rm: Ie 6.0/Wine: in edit footer not at the bottom || || ||
|| *Opera* || rm: 8.52: default/module: highlighting not 100% height when hover; in edit footer is not at the bottom || || ||
|| *Safari* || || || ||

## rm humble opinion:
 * ~~the configuration should be the action done when clicking on modules list instead of edit;there should be an "edit" button before disable;~~

 * ~~again in the modules list title and module name seems redundant, we can remove module name and instead place here a short description without requiring to hover each entry seeking for the tooltip;~~ DONE

 * i'd like to give a try to manager-actions be below content;

 * We can reduce the numbers of colours, there are up to 5 different backgrounds for a content. I really don't care about the colors, what i mean is that manager-infos and .block .header have the same background color which is darker than .container and .content, then that manager-actions, the pager and the content have the same background color. See attached patch as example; UPDATED

 * the header is horrible :P ~~"Seagull Application Framework" should be real text and not an img~~

 * the module form is very nice, congrats for the good work!;

 * i think that the gecko / MSIE user agent check should be more precise with respect to IE 7.0, see http://blogs.msdn.com/ie/archive/2006/02/03/524256.aspx

 * ~~div naming, i think that using generic names is better. Instead of reference each div with its name block-container, block-header, block-content, block-footer use block (instead of block-container) and then reference each div by its parent like block header, block content, block footer. This is a cleanup that we've introduced in trunk times ago and we have discussed a bit about it;~~ DONE

 * pager should be moved out of the table.

 * ~~BUG, reported by the w3c checker against {foobar}/index.php/default/module/:~~ DONE
  ~~Line 223 column 5: end tag for element "div" which is not open.~~

 * ~~BUG: no gallery pic, See attached pictures.~~ DONE

 * BUG: optionsLinks is broken when there too much entries.


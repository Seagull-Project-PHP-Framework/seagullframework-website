<!-- Name: TipsAndTricks/GoodModuleToCopy -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/04/02 03:28:14 -->
<!-- Author: demian -->
# Good Example Module to Copy

For some time the FaqMgr class has been the best Manager class to model your work after.  It basically has no quirks/hacks and implements all the straight Seagull functionality:

  * validate/process/display implementation
  * good range of input validations
  * context specific, multi-lingual error messages
  * passing hidden action params in the form
  * multi-lingual text strings in the form
  * correct error raising the success redirection

Useful things that are missing are:
  * persisting combobox values in display()
  * more detailed validations
  * helper data access methods
  * pagination

These can all be found in UserMgr + RegisterMgr, which are slightly more complicated because the former extends the latter.
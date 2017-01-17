<!-- Name: Modules/ShoppingCart -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/11/15 15:38:10 -->
<!-- Author: aj -->
# Shopping Cart Module
Here you can find some data about the beta version of the Shopping Cart module. See also the [Shop RFC][1]

## 1. Description

Now you have a working version of the Shopping Cart code. There are 2 new modules: 'shop' - the product manager and 'Cart' and universal cart module.

To access the SC you have to login as 'admin' because no default perms are created by default. 

More docs will come soon but for the moment I think that the menu is self explanatory.


## 2. RFC and Bugs

This is the beta version so I'm waiting for your observations. [You can find the RFC here][2].

## 3. Code refactoring
  * do shop, cart and rate have to be separate modules?
  * we must remove & replace phpthumbs ASAP, sgl cannot ship GPL code by default
  * comply with PEAR coding standards
	* there is never a space between function name and opening parentheses, right: function foo(); wrong: function foo ()
	* static function calls - right: Foo::bar(), wrong Foo :: bar()
	* control structures - right: if ($condition) {}; wrong: if($condition) ... same applies for switch, foreach, etc
	* many Output methods are duplicated
	* method names never start with caps, ie, wrong:  SGL\_Output :: Translate($key);
	* in a function definition, the opening brace { always comes on the next line
	* use constants for product statuses, ie product statuses
  * \~\~what is formatLeuGreu ?\~\~
  * is SQL portable? IFNULL, CONCAT ...
  * remove non-FC code, ie:  array('action'=\> 'listProd', 'frmProdId' =\> $input-\>productId]; in PriceAdminMgr.php
  * keep lines to a MAX of 80 chars wide
  * make license header comments uniform with rest of sgl
  * all logical operators must be called as &&, ||, etc, 'and' and 'or' have different results and should be reserved for short-circuit situations like mysql\_connect() OR die()
  * generateSelect() should be done in the templates
  * errors should be translated in templates
  * remove hard-coded Romanian errors, ie: return 'In aceasta perioada nu sunt promotii';
  * replace hard-coded "fopen ("http://cursvalutar.kappa.ro/", "r")" with web service
  * \~\~i don't think RateMgr should be a manager, probably just Rate\~\~
  * \~\~why are all the cart-related blocks not listed in the block mgr?\~\~
  * what is this: $fd = @ fopen ("http://cursvalutar.kappa.ro/", "r"); - we should use a free rate exchange service like i suggested earlier
  * move \_mime2AssetType() to SGL\_Util
  * in ShopNav.php, $theme . '\~/css/DropDown.css" -\> file doesn't exist
  * SGL\_HTTP :: redirect() should take arrays as args, cleaner code
  * remove 'images' dir from www, it should be  in themes
  * in Newsletter, make section buttons (compose, subscribers, lists) highlighted to show selection

[1]:	/wiki:RFC/Modules/Shop/
[2]:	/wiki:RFC/Modules/Shop/
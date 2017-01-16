<!-- Name: RFC/Modules/Shop -->
<!-- Version: 7 -->
<!-- Last-Modified: 2006/06/08 01:06:53 -->
<!-- Author: demian -->
# RFC for Rares' shopping cart module
[[SubWiki(RFC/Modules/Shop)]]
See also: Modules/ShoppingCart
and: [wiki:RFC/Modules/Ecommerce/Ideas]

[[TOC]]

## Download
svn, branch shop

## Structure
The shop contains three modules:
  * shop: the basic shop module
  * cart: an universal cart
  * currency: if you have a shop with more than one currency this can be helpful

### Shop

#### shopAdminMgr: index.php/shop/shopadmin
You can manage discounts, products, batch processing, shop config and product category descriptions
===== Discounts =====
  * For Discounts on a per user basis
  * To change a discount per user, just Add Again the discount for that user
  _How about renameing it to 'Add/Edit'?_ [/WernerKrauss WernerKrauss] /20.08.2005 15:16/
  * For discount on a per user per product basis (special price for a user on a product) go to the product edit form and you'll find the "Edit discount list for this product" link under the price field.
  * To change a discount on a per user per product, just Add Again the discount for that user
===== Products =====
 For your products you sell in the shop. Each product has:
  * id (internal product ID)
  * cod1 (e.g. barcode, internal odering-number SKU, etc...)
  * cod2 (if you need two different codes)
  * name (product name e.g. deskjet)
  * short_description (product description that will be shown on category products listing)
  * description (product description shown on product details page)
  * manufacturer (e.g. hp)
  * price
  * currency (e.g. USD)
  * warranty (e.g. 1 Year)
  * status (for the moment only: In Stock, Short Supply and Phone Order)
  * BALANCE (amount in warehouses of products. can be used together with status field)
  * order_id (used to establish the listing product order if they are not already ordered by name, price etc..)
  * promo (if it should appear in the promotion block - RndProducts)
  * new product flag
  * bargain flag
  * datasheet and manufacturer link
  * image
===== Batch Processing =====
For exporting and importing the shop to/from csv file
===== Configuration =====
For configuration of the Shop module. Pretty self explanatory.
===== Product Category Description =====
guess what? (disabled for the moment, needs work)
### Cart
user can check out his basket.
admin can control orders, popular and payments
#### Order
  * list of orders
  * detailed info per order, incl user data
  * BUG: how to change status of an order? (this will be added in the future)
#### Popular
atm i only get a "your cart is empty" error, cause the button doesn't link to the correct url? [/WernerKrauss WernerKrauss] /18.08.2005 15:53/
#### Payment
atm i only get a "your cart is empty" error, cause the button doesn't link to the correct url? [/WernerKrauss WernerKrauss] /18.08.2005 15:53/
### Currency
have not used this yet. [/WernerKrauss WernerKrauss] /18.08.2005 15:53/

## Questions
### Shop
  * Q: where are manufactures managed?
  * A: they are not managed, we create a unique manufacturer list from the product table every time we need it (on product edit form). [/RaresBenea RaresBenea] /18.08.2005 19:56/
  
  * Q: is there the option to have prices incl VAT shown? In Gernamy e.g. normally shown end-user prices include all taxes. [/WernerKrauss WernerKrauss] /18.08.2005 16:20/
  * A: yes, you have the $product->price and $product->priceVAT variables in ShopMgr::_lister() so just pick the one you want to display and add it in templates like this (productList.html) {this.plugin(#formatPrice#,product.priceVAT):h} or {this.plugin(#formatPrice#,product.price):h} priceVAT is the price with VAT applied. [/RaresBenea RaresBenea] /18.08.2005 20:06/

## Bugs
  * Pager of product category description finds 34 items. but there are only 10??
  * Shop Configuration: After sending new configuration i get the error: Call to undefined function: dumpr() in [...]/ShopAdminMgr.php on line 1144 (rev. 1000) (2006/01/04)
  
## Todo
  * Product Categories: still romanian text hardcoded in Template (Kategorijø apraðymai: 11-20 ið 34 )

## Known Issues
  * none for the moment, please add them here.

## Ideas
  * every user should be able to have it's own shop. so if user foo has his "foo related shop" where he sells "foo products", the order should be sent to foo's emailadress.
  * sending orders can go to emailadress of user (-> use user-module for that)
  * to have the possibility to have variation for the same product (Ex: Product=Keyboard, Variations= Color: white/black). This can be selected in the cart before check out.
  * is it possible to make an easy "in stock" function? like artikles that are ready to send are marked "green", articles that aren't in stock are marked "red" [/WernerKrauss WernerKrauss] /09.02.2005 11:19/
  * optional shipping adress [/WernerKrauss WernerKrauss] /07.03.2005 12:36/

### Product Mgr
  * "No Image" text instead of an image (maybe in a div that is as big as the image should be?)
  * Make denoted fields for a product configurable
  * Move Product stuff to Publisher (when items are refactored in first quarter of 2006)
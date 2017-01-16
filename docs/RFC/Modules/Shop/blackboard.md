<!-- Name: RFC/Modules/Shop/blackboard -->
<!-- Version: 21 -->
<!-- Last-Modified: 2006/07/19 00:56:51 -->
<!-- Author: mwattier -->
# The brainstorming side, Our ideas, Our thoughts

[[TOC]]
## a couple of Mikes thoughts

*Product Pricing*
  * Multiple prices per product, currency can be determined at the price record and not the product record
  * Pricing based on groups
  * Timed discounts eg.. From 07/01/2010 through 07/25/2010 this product is 10% off, OR a flat 5.00 orr OR a fixed price. Once time expires, price goes back to normal.

    
Basic abstract module for Shipping & Payments. 

*Shipping*
  * Easy to interface with UPS, Fedex etc through plugins
  * Easy to modify rate returned prior to showing price to customer
  * Must also cover zones
  * Flat rate By state 
  * Flat rate By country
  * Allow custom calculations

*Payments*
  * Easy interface to multiple gatways through plugins
  * Cascading gateway
  * paypal of course
  * Remember recurant/periodic billing will come into play quickly


*Order Management*
  * Workflow order management where an order can move from restricted workstation to restricted work station
  * Unlimited order status's with events ( like customer notifications, or payment capture )when an order reaches a status.
  * Print PDF copy of single order, or all orders of a particular status
  * order comments both hidden and "public" to the shopper.
  * Line item status. Each product needs seperate status types. To enable a "Backorder" of one item and the rest of the order can ship.

*Checkout*
  * Zero down, partial down payments
  * Single page checkout process
  * Try before you buy

I'll try to think of more later :)

## Rolandas' thoughts


1) XML (Excel ???) import/export. Excel export can be done easily IMHO.

2) to make category descriptions and cattegory images. Currently we have descriptions stored in category_descriptions table. To move this to category table.

3) shop usage statistics. Most visited product, etc... 

4) creation of product categories from shop module. 

5) what do we do with products which do not have prices? (ask first products...)

6) product shipping. If client has 2 or 3 possible shipping locations he should set where to send prods (in cart module)

7) payment management.... who paid who not, most profitable client...

8) displaying of product versions in shopadmin::list. to display or not to display? to mark them specialy

9) associated products. user who bought this, also bought that. (like amazon shop)

10) product reviews.

11) more than 1 pic.

12) to use datagrid for shopadmin::list.

13) we should have catID placed in the session in order to know where user is going. a) In order to avoid problems like 
we had with advanced search b) in order not to do $output->catID = $output->product->cat_id; in every method...


*bugs we have...*

1) currency hardcoded in cart ($currency value dissapears???)

2) currency should be set in data\ary.currency.en.php

3)  to fix HTML/CSS everywhere...

4) fix translations. Everywhere...

5) deal with db::errors...

6) update shop ShopConfigMgr.php and its template and validate.

*Proposed functions for DA*

For getting data
-------
_getProductData($prodID, $userID) - working...
getPrice(userID, ProdID) - we have this one. GEts product price, user special price (with discount)

getProductsData(categoryID) - get product data in this category
getPrices(userID, categoryID) - we have this one. get product price in this category

_getProductVariant($prodID, $userID) - we have this

getCategoryDescription(catID) - gets category description... Needed for 1 project.

getManufacturerList() -- have this one...


---some logical
isProductVariant(prodID) checks if prodID is product variant



writing data
-------
saveProduct(ProdArray)
  uploadPicture(ProdID) - used by saveProduct ? upload, minimize, (download from WWW ???)

makeProductVariantFromExistingProduct(ProdID, ProdVariantID)

writeCategoryDescription(catID, descrText)


deleting data
-------
deleteProduct(ProdID)
deleteProductVariant(ProdID)


validation
-------
validateProduct(ProdArray, requiredFields)


CSV/Upload functions?
-------


Config functions?
-------

*other seagull ideas.*

  * to save configs in \var\sitename.modulename.conf.ini and make them editable form module using HTML form. 
lets say user goes to shop module and can edit shops conf.ini  /var/directory is system RW. 
if we keep conf.ini in module dir. and we want to edit them we must make them system RW. might be boring to do 
this for all modules.

  * to use this JS calendar for everything what needs to set date... 

## Werner's thoughts
  * Use publisher to store product item stuff (you'll have translation2 support out of the box and good functionality to store content) after aj has refacroed SGL_Item
  * Make fields for a product more flexible. in some shops you need a second product id, in other you won't need another field... when using publisher it's easy just to change the item "product".
  * Maybe a nice pattern can help us using different product-item types. If a product is an object, we can extend the object for our shop and it takes care of saving to db, form validation etc...
  * Interface to interact with other systems (e.g. Open Office Faktura scripts for managing bills etc...)
  * we could also use the order stuff for booking an apartment or hotel (we have one product, e.g. room 123 and it's available every day...)
  * introduce system like "product"-"size"-"color" for selling t-shirts e.g.
  * make search block also searching for all categories (werner, 2006/01/25)

## Rares' thoughts
1) first step I think is to make ver1.0 of the shop to work. This is the version from the shop branch, make it SGL 5.5 compatible, patch the bugs and make it available and not add new features to it. That's why category description is disabled and maybe don't add the product versions thing

2) make a list of features that we want to implement in V2.0 and remake the code to meet those features and try to stick to them.

3) make a road map and divide the work so we don't overlap

Comments

Rolandas
1) The export to XML is a good idea maybe include the image to uuencoded. We have export to excel as the price list or as CSV

5) Product without prices?  Make them 0? When do you need product without a price?
_Werners comment: I need it now for presenting products without possibility to order online. My customer need the "shop" module only for presenting the products and to make possible cusotumers being interested in his products. And often i see in catalogues special stuff with "ask for price" (expensive guitars e.g.)_

10) Product reviews: I think we can make a separated module for this so we can add review to articles, faq etc.

11) More than one pic: integration with gallery module?


1) the currency is suppose to take the currency variables from DB and put them in $conf every time RateMgr is initialized. We need to implement this somehow related to the user preferences as you select preferred language and timezone you select language. Also have a RateMgr_Singleton and a RateMgr::gateRate() that uses cache so we don't use the $conf anymore

1) the edit config files thing I think it's a global feature that we can implement on all files.  Maybe move part of the config to  DB and  use cashing for speeding things up.

Werner

1) There is a problem in shop module to use items.
Intro:

- price calculation scheme

price = price of product - if no discount

price = price with % discount - if general discount

price = special price from price table - if a discount is specified on a single product

Now, imagine that you want all the products from catId=13 from Sony and display first 10 products ordered  by price.

I think that this will make a big overload on the CPU if we store the product data with as items.

_Werners comment: has to be tested. We can use Item as extra data of a record (and for make the shop multilingual). Let's see what aj is cooking with new item stuff... Should be very flexible, so i hope we can use it for whole product_
 
BUT

We need to be able to add custom fields to product or series of products. Here we can  use  a combination of product in classic table with item extension OR at an field on the table 'Ex: extra_fields' and  serialize the  extra field there. In this way we can have text search done on that field too.


Other features that we can add

most of the ideas were expressed already so I'll just some

 * Stock control - how many products are there available
 * One product can be in more than one category - maybe a main category and other categories so we don't display twice in case of a search or multiple category listings _(+1 Werner)_
 * Make an advanced search form
 * Introduce filters in category listings (Ex: in category LCDs you can filter by size, refresh, brand: go to: [http://www.emag.ro/monitoare_lcd] the "Filtreaza thing"
 * Use UrlAlias for nice url's
 * Introduce the possibility to have 'Coupon discounts' in cart module with coupon authentication
 * Category discounts, Free gifts / purchase
 * Create boundled produs price (Ex: Buy one, get one free or Buy this and this half the price) maybe they'll be just another product
 * Introduce Fidelity points and addign points / product or points / sales
 * Store the product with a sales and a cost  price so we can  make a proffit  analisis of the sales
 * Add the 'Wish list' or 'Favorite products' block (maybe bookmark module derivated)
 * Integrate with shipment providers (UPS, DHL etc.) for packet tracking and also calculate shipping costs
 * Order histroy for every user
 * Insert orders that are processed by phone
 * Intregration with popular payment gateways

IRC meeting
 * [05/01/2006 10:28](http://trac.seagullproject.org/wiki/RFC/Modules/Shop/blackboard/irc_060105_1028)
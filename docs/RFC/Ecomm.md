<!-- Name: RFC/Ecomm -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/08/18 21:24:35 -->
<!-- Author: ed209 -->
# E-commerce Module
[[TOC]]
The e-commerce module, which is named ecomm for short, will provide all the basic components for running an online shop, from cart to fulfilment.  By default the ecomm module will cater for a multiple seller to multiple buyer scenario.

## Team
 * User/MikeWattier
 * User/DemianTurner
 * User/EdLea
 * User/ThomasGoetz

## Managers

### Cart
 * cart will be ajax enabled, optionally, same manager actions called regardless of GUI

### Catalog
The list of products.
 * must be abstracted, to allow integration with 3rd party catalogs.

### Inventory
To track the number of physical items in stock.

### Order
 * discounts are calculated only at checkout, accepting coupon IDs, etc.
 * orders consist of line items, each line item has its own status
 * If user doesn't have an account use the contact info they submit to create them an account via an observer.
 * Prevent double accounts check for user by email.

### Pricing
### Purchase
 * pass order ID only
 * look into using payment_process (pear)
 * can we lock products in the payment process to prevent selling the same item twice (very small probability of this happening)

### Fulfilment
Anything to do with shipping, and including things like product downloading.
 * ability to add notes to order history, scope = line item
 * alerts for change in order status, optional, subscribe
 * current order status - status table

### Shop History
 * list of purchases
 * favorite products
 * alert for new item in family of products

## Availability
A working version is expected to be ready for end of August 2006, and will be fully compatibly with Seagull 0.6.

## License
The module will be available under GPL and commercial licenses.

## Reference
 * http://shopify.com/
 * [wiki:RFC/Modules/Shop]
 * [wiki:RFC/Modules/Shop/blackboard]
 * [wiki:RFC/Modules/Shop/blackboard/irc_060105_1028] 
 * Modules/ShoppingCart 
 * [wiki:RFC/Modules/Ecommerce/Ideas]


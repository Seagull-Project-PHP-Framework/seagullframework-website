<!-- Name: RFC/Modules/Ecommerce/Ideas -->
<!-- Version: 28 -->
<!-- Last-Modified: 2006/05/31 11:45:46 -->
<!-- Author: thomas -->
# Ideas
Please list all ideas you have for the Ecommerce Module.

## General
  * As few page refreshes as possible (e.g. single page form)
  * Cronable
  * user search history and user interest indexes
  * PMS (Project Management System) type application to trace and process orders
  * User wishlists
    * Should not be limited to 1 list per user
  * I think there's a lot to be gained by studying the [API of the Amazon e-commerce web service](http://developer.amazonwebservices.com/connect/servlet/KbServlet/download/163-102-382/ecs-dg-20060308.pdf), the most successful e-com solution on the web
  * The shop module should be light, not to much extra features. The extra features should be put in separated files (when possible). I prefere a light modules and a help/demo code variations that show me what/how can I add the featurs. Otherwise we risk to get like the current status of the shop that is full with customer specific features - Rares
  * Maybe it's a different modules but permit the option of make a Ebay style shop. Where users can be sellers and/or buyers, tender, buy now price, comments about customers, .., 

## Localisation
  * Make sure the Module can easily be localized:
  * Issues like VAT, Sales Tax etc. should be easy to adapt.
  * Handle multiple currencies
  * The module should have a 'local' look: all strings, buttons etc. must be easy to translate

## Product
  * "Products" should be flexible. Not only material things to buy and download, also services, hotel-rooms, educational courses etc. (booking of  things)
  * should also handle different sizes and colors of a product (e.g. for clothes) and, more general, any number of option and option values
  * Product Requests (seller doesnt offer a product, allow requests)
  * Handling of physical products and downloadable ones, keep in mind downloadable products can be in different sizes/formats. 
  * searching and filter system to select and order products, AJAX type Google suggestions product search
  * system to store favorite products for each user 
  * products can be categorized and the categories can be managed by the administrator; each category may have some additional description, pictures, discounts and so on.
  * special offers, for example "20% discount until may 30th 2006": discounts may be laid out to a single product for a limited time period and this special offer also appears on the start page with some nice graphics.
  * discount code: a code that customer can obtain from any source (buy, gift, ...). Writing this code in the cart, the amount of total will be deducted.
  * when adding or editing new products, administrators also can upload any images and movies about that product. when users views the product, these images and movies are displayed. movies are wrapped either with quicktime player, windows media player or flash player depending on the file format. all automagically.
  * like ebay: instead of selling products for a fix price, we also could do an auction instead of sale. maybye an online shop wants to sell a single thing, i.E. an old computer, so it can start an auction instead of selling it at a fix price to make the best profit out of it.
  * Product inventory availability change the shipping time
  * Permit user comments on products
  * Allow user to ask directly to seller for especifications/doubts about a product.
  * Make sure to implement a good set of options, so any of the above features can be turned on/off.
  * Import products from .csv file (all or selected columns - e.g. only prices), add or replace item.

## Customer
  * Multiple customer groups, each comming with different prices, shipping and payment options
  * Registered and unregistered customers
  * email notification sent to customer when a new product in a chosen category is available. (perhaps some sort of more generic notification implementation, so sms and other things could be used.)
  * Administrator can send massive emails (to users that flags some check in the registration) about special offers or new products

## Orders
  * Order tracking (maybe becomes an Orders module?)
  * Handle multiple delivery methods
  * Keep track of last orders
  * Sending of orders by mail in html and text
  * Doing a new order based on an old one (to reorder a list of items)
  * Keeping multiple "caddies" accross sessions for a single customers
  * allowing customers to have "template" orders that they can load and buy quickly (for exemple for an ink shop)
  * Handle returning, damaged or missing items (refund or re-send order)
  * Administrator can changes orders (edit price or add/delete products etc.)

## Payment
  * Easy payment gateway use choice (authorize.net, verisign, paypal, credit card, etc)
  * Shipping providers (calculation service integration?)
  * handle fidelisation program (points, cash-back, rebate, ...)
  * Automatic Payments (for subscriptions and such)
  * Group Based (cost of subscription)
  * Block and notify users if payment is missed
  * Handle invoice and credit note
  * Support free products. 

## Statistics
  * Reports functionality (daily, weekly, YTD, etc)
  * Reports about most sold products, order in the week, every day sessions, customer searches, first page referer, exit pages

## Warehouse
  * Handle product inventory (maybe becomes an Warehouse module?)
  * Handle buy order
  * Inherit products from a "master shop". A master shop keeps a certain set of items and connected child shops must include the parents' shop items beside it's own items without the ability to manipulate the foreign items.

## Marketing
  * Affiliate Marketing support
  * Reselling Support
  * "Tell a friend"

## Good examples
  * See http://www.panic.com/goods/
<!-- Name: Modules/Ecomm/Api -->
<!-- Version: 1 -->
<!-- Last-Modified: 2008/09/02 18:07:05 -->
<!-- Author: demian -->
# Ecomm API
[[TOC]]
## Product
Product data can be stored in any backend, currently supported are CMS and simple 'product' table.


    // $config set for CMS product
    $myProduct = ECM_Product::getByType('t-shirt')
        ->title = 'Foo Shirt'
        ->description = 'this is my tshirt'
        ->save();

editing a t-shirt product instance, 

    // $config set for Simple product
    $myProduct = ECM_Product::getByProductId($productId)
        ->title = 'Foo Shirt'
        ->description = 'this is my tshirt'
        ->save();

removing a product

    $myProduct = ECM_Product::getById(23)
        ->remove();

## Product with variations
creating just new variations

    // $config set for CMS product
    $myProduct = ECM_Product::getByType('t-shirt')
        ->addVariation('colour', array('red', 'blue'))
        ->title = 'my tshirt'
        ->description = 'here is desc'
        ->save();

creating of new Product instance of type t-shirt

    // $config set for CMS product
    $myProduct = ECM_Product::getById($id)
        ->addVariation('size', array('L', 'XL'))
        ->save();

## Shopping for products

    // gets product from inventory with variation combinations
    $myProduct = ECM_Product::getByInventoryId($inventoryId)
        ->retrieve();


## Catalog
Catalog data can be stored in any backend, currently supported are CMS data store and records from the 'product' table.

get a collection of products of a certain type

    $aCatalog = SGL_Finder::factory('Product')
        ->addFilter('typeName',  't-shirt');
        ->retrieve();
        // returns an array of objects of type ECM_Prodcut

Returning a read-only collection of Products from the product table
get a collection of products of a certain type

    $aCatalog = SGL_Finder::factory('Product')
        ->addFilter('typeName',  't-shirt');
        ->retrieve();
        // returns an array of objects of type ECM_Prodcut

## Cart
Cart data can be stored in any backend, currently supported is the 'cart' table.

get all items from cart

    $aProducts = SGL_Cart::getById($userId)
        ->retrieve();

remove a product from the cart

    $ok = SGL_Cart::getById($userId)
        ->removeProduct($inventoryId)
        ->save();

adding an product to the cart

    $ok = SGL_Cart::getById($userId)
        ->add($inventoryId, $productId)
        ->setQuantity(3)
        ->save();

editing product quantity in cart

    $ok = SGL_Cart::getById($userId)
        ->getProduct($inventoryId)
        ->setQuantity(4)
        ->save();

## Data Access
get all inventory for a shop

    $oInventory = $da->getInventoryByProductId($id);

add an item of inventory

    $ok = $da->addInventoryByProductId($id);

update inventory

    $ok = $da->updateInventoryById($id);

adding variation names
API

    $myProduct = ECM_Product::getByType('t-shirt')
        ->addVariation('size')
        ->save();

DA

    $da->addVariationByProductId($productId, $name, $value = array());



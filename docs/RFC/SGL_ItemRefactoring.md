<!-- Name: RFC/SGL_ItemRefactoring -->
<!-- Version: 14 -->
<!-- Last-Modified: 2006/03/17 19:30:42 -->
<!-- Author: aj -->
# SGL_Item refactoring
[[TOC]]
## SGL Item
  * All item tables need the ability to be prefixed to allow for flexibility.
  * Item Types need the ability to have options (key/value pairs for things like check boxes, radio pairs, and selects)
  * Item Type Fields need the ability to specify the datatype (int, date, string, etc)
  * Item Type Fields need to be extended to support: check boxes, radio pairs/combos, selects, images, links, etc
  * All tables need to be prefixed.

## Item Data Types

Using the Complex Item Structure it is possible for Item Type's to have various options.

Simple Data Types
  * Date & Time
  * Int
  * Float
  * Varchar
  * Text 

Complex Data Types
  * Key/Value Pairs
    * Key - varchar
    * value - varchar 
  * HTML
    * Image 
      * Document ID - int
      * Label - varchar
    * Link
      * Label - varchar
      * Url - varchar
    * Form
      * Checkbox
      * Checkbox Group
        * Label
        * Options
          * Key/Value Pairs        
      * Input
        * Label
        * Type
        * Value
      * Textarea
        * Label
        * Value
      * Radio
        * Label - varchar
        * Options 
          * Key/Value Pair
      * Select
        * Label - varchar
        * Options
          * Key/Value Pair

Options:
  * Default Value
  * Key/Value Pairs
    * Min. Value
    * Max Value


## Item Types

  * Article
    * Title - varchar
    * Body  - varchar
    * Extended Body - text
  * Document
    * ID - int
    * Label - varchar
    * Description - text
    * Filename - varchar
    * No. Downloads - int
  * User
  * Permission
  * Preference
  * Faq
  * Role

## Item Structures
Structures allow for the ordering of items.

  * Simple - Use order_id
  * Complex - SGL_NestedSet

## Item Storage Containers
  * RDBMS - PEAR::DB, adodb, propel,
  * XML
  * LDAP
  * PEAR::Config

## Item View
Provide a way to display items in various ways.
 * Detailed - Display meta and item data.
 * Summary - Display minimal data.
 * Channel (Combination of various Views)

## Item Caching
We need a whole hellofalot of this.

## Item Relation Management
Items need to be related to each other allowing for things like articles belong to multiple categories, prefs and perms for a specific user &/or role, documents belong to an article &/or category.

## Ideas
  * Hide fields also behing objects. Store them to assoc array $item->aField with method called getDynaFields(), which may be called when item_type_id is set.
  * Access for additional fields with following methods:
  * $item->aField['fieldname']->getField(); // returns unformatted field value
  * $item->aField['fieldname']->setField(value); // sets value, operation may vary according to fields type.
  * Additional field accessing methods should by extending item_Field class, because those are mostly module spesific. They could be:
  * getFormattedField()
  * getHtmlInputField()
  * Overridden field class name could be provided as an attribute in Items constructor.
  * Dataobjects update method should be overridden to update also dynamic field values to DB.

### Item Data Types
#### RDBMS
  * rename item_addition to item_field_value to make it easier for developers
  * in item_type_mapping fieldName and item_type_id should form primary key

#### Item Meta Data
  * meta items modification in standard Dataobject way


### Other things
Something of which I have no strong opinion is should we provide an interface for relations between items. Most relations are module spesific so module developers can make their own tables parent_item_id, child_item_id. I vote not to keep core clean.

This will be addressed w/ SGL_Item_Structure for trees and SGL_Item_Relationship -- User/AjTarachanowicz 2006-02-11
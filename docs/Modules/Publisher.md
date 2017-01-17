<!-- Name: Modules/Publisher -->
<!-- Version: 12 -->
<!-- Last-Modified: 2007/01/31 12:42:29 -->
<!-- Author: demian -->

# Publisher (deprecated)
* TOC
{:toc}

Through the Publisher module Seagull allows you to create three types of content. This is easily customisable however only 3 types will be discussed here:
  * What you see when you click the `'Articles'` tab in the front end is a document collection. Creating articles of type `'Html Article'` allows you to place your content in a hierarchy that you build using the `'Categories'` button above. This can be useful for intranet applications, or if you have a large body of work that needs to be categorised. Document collection articles will be displayed with all articles from the same category appearing in the `'Related Articles'` box on the right. Similarly, all files uploaded to the same category with the _[Document Manager][1]_ will appear in the `'Related Documents'` box.
  * However, if you want to make standalone pages that will be linked to by their own tab, please use the `'Static Html Article'` type. In order to create the navigation that will link to these static pages, please use the [Navigation][2] module.
  * Finally, you can create news items by choosing the `News Item` type, these will appear in the left hand column in the `Site News` box. These articles (and all others) can be retired automatically according to the date constraints you set on the item.

## Content Item Management using SGL\_Item
This is only a couple examples on how to use SGL\_Item please look at [[lib/SGL/SGL\_Item.php][3]] for a complete overview of  functionality.

## Retrieving Content Items
### Retrieving a Single Content Item

See: Code Example [source:trunk/modules/publisher/classes/ArticleViewMgr.php ArticleViewMgr::\_view()], API Doc [source:trunk/lib/SGL/SGL\_Item.php SGL\_Item.php::getItemDetail()]

	    $ret = SGL_Item::getItemDetail($input->articleID, null, $input->articleLang);

### Paginated Content Items

See: Code Example [source:trunk/modules/publisher/classes/ArticleViewMgr.php ArticleViewMgr::\_summary()], API Doc [source:trunk/lib/SGL/SGL\_Item.php SGL\_Item.php::retrievePaginated()]



	$aResult = SGL_Item::retrievePaginated(
	    $input->catID,
	    $bPublish = true,
	    $input->dataTypeID,
	    '',
	    $input->from,
	    'start_date');


## Expanding Content Items
### Adding a new Content Item

It's very easy to add a new item category to publisher for storing content.

For example i recently had one for weather reports:

 * <title>
 * <minimum>
 * <maximum>
 * <prediction>

To do your own:

  1. add a new Content Type using ContentTypeMgr.php (disabled until 0.4)
   2. to enable it just go to _<your domain>/index.php/publisher/contentType/_
  1. you can add new content types using a comfortable form.
  1. if you want the category chooser to be displayed in add method you have to hack it in in [source:trunk/modules/publisher/classes/articleMgr.php] on line 181, e.g. if ($input-\>dataTypeID ==  2 `}|` $input-\>dataTypeID == 6  `||` $input-\>dataTypeID == 7)
  1. same in edit method (line 302) and to update method
  1. in [source:trunk/lib/SGL/SGL\_Item.php SGL\_Item::generateItemOutput()] create a case statement that corresponds to the id created in step 1, following the example of the others, you'll see it's quite simple. 

*note:* the number of line can be different depending on the version

*note:* your item must contain a field called "_title_", otherwise it won't be cought by _SGL\_Item::retrievePaginated()_

[1]:	/wiki:Modules/Document/
[2]:	/wiki:Modules/Navigation/
[3]:	http://seagull.phpkitchen.com/apidocs/SGL/SGL_Item.html
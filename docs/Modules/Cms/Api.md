<!-- Name: Modules/Cms/Api -->
<!-- Version: 37 -->
<!-- Last-Modified: 2009/11/02 16:09:06 -->
<!-- Author: demian -->
## CMS API
[[TOC]]
Here is a summary of the methods available in the CMS API.  For more traditional API docs see [here](http://api.cms.seagullsystems.com/).
### methods available

    // following notation borrowed from the Pragmatic Programmers
    // static object calls indicated in standard way, eg SGL_Content::getType()
    // object method calls indicated by SGL_Content#myMethod()
    
    // API according CMS v. 1.4 (update 2008-07-16)
    
    // SGL_Attribute
    
    SGL_Attribute::SGL_Attribute(Object $oData = null) : SGL_Attribute;
    SGL_Attribute::getById(Integer $id) : SGL_Attribute;
    SGL_Attribute#getType() : Integer;
    SGL_Attribute#changeType(Integer SGL_CONTENT_ATTR_TYPE_*) : Boolean;
    SGL_Attribute#rename(String $newName) : Boolean;
    SGL_Attribute#delete() : Boolean;
    SGL_Attribute#save() : Boolean;
    SGL_Attribute#getParams() : aData
    
    
    // SGL_Content
    
    SGL_Content::SGL_Content(Object $oData = null) : SGL_Content;
    SGL_Content::createType(String $name) : SGL_Content;
    SGL_Content::getById(Integer $id, Integer $version = null) : SGL_Content;
    SGL_Content::getByName(Integer $name) : SGL_Content;
    SGL_Content::getByType(String $type) : SGL_Content;
    SGL_Content#getLinkedContents(String $typeId = null) : aSGL_Content;
    SGL_Content#getType() : Integer;
    SGL_Content#getStatus() : SGL_CMS_STATUS_*;
    SGL_Content#setStatus(SGL_CMS_STATUS_* $status);
    SGL_Content#validate() : Boolean
    SGL_Content#save(Boolean $newVersion = false) : SGL_Content
    SGL_Content#rename(String $newName) : Boolean;
    SGL_Content#delete();
    SGL_Content#getAttribId(String $attribName) : Integer
    SGL_Content#addAttribute(SGL_Attribute $oAttrib) : SGL_Content;
    
    
    // SGL_Context
    
    SGL_Context::SGL_Context(Object $oData) : SGL_Context;
    SGL_Context#process() : SGL_Context_*;
    
    
    // SGL_Context_WebContentType
    
    SGL_Context_WebContentType#process(Object $oData) : SGL_Context_WebContentType;
    
    
    // SGL_Context_WebContent
    
    SGL_Context_WebContent#process(Object $oData) : SGL_Context_WebContent;
    
    
    // SGL_Finder
    
    SGL_Finder::factory(String $handler) : SGL_Finder; // of type SGL_FINDER_*
    SGL_Finder#addFilter(String $filterName, String $filterValue) : SGL_Finder;
    SGL_Finder#retrieve() : Boolean;
    SGL_Finder#getFilters() : aFilters;
    SGL_Finder#getAttributeFilters() : aAttributes;
    
    
    // SGL_Finder_File
    
    // not implemented
    
    
    // SGL_Finder_Contenttype
    
    SGL_Finder_Contenttype#retrieve() : aSGL_Content;
    
    
    // SGL_Finder_Content
    
    SGL_Finder_Content#retrieve() : aSGL_Content;
    
    
    // SGL_Finder_AssocContent
    
    SGL_Finder_AssocContent#retrieve() : aSGL_Content;
    SGL_Finder_AssocContent#addFilter(String $filterName, String $filterValue) : SGL_Finder_Assoccontent;
    

### calling in templates

    {oMyCV.attributeName} //   using a __get() method

### using finders

    SGL_Finder::factory('content')

## Usage examples
### create a new content type

    $oRestoReview  = SGL_Content::createType('RestaurantReview')
        ->addAttribute(new SGL_Attribute(
            array('name' => 'dateEaten', 'typeId' => SGL_CONTENT_ATTR_TYPE_DATE)))
        ->addAttribute(new SGL_Attribute(
            array('name' => 'dishName', 'typeId' => SGL_CONTENT_ATTR_TYPE_TEXT)))
        ->addAttribute(new SGL_Attribute(
            array('name' => 'overallRating', 'typeId' => SGL_CONTENT_ATTR_TYPE_FLOAT)))
        ->save();

### rename and modify a type

    $oRestoReview  = SGL_Content::getByType('RestaurantReview');
    $oRestoReview->rename('Foo');

### renaming and deleting an attribute

    $bar = SGL_Attribute::getById($id); // never by name, too ambiguous
    $bar->rename('fluux');
    $bar->delete();

### getting an attribute's type

    $bar = SGL_Attribute::getById($id);
    $type = $bar->getType();

### getting an attribute's parameters


    // eg for a choice attrib like RADIO
    $attr = SGL_Attribute::getById($id);
    if ($attr->getType() == SGL_CONTENT_ATTR_TYPE_RADIO) {
        $aParams = $attr->getParams();
    }


### changing an attribute's type

    $bar = SGL_Attribute::getById($id);
    $bar->changeType(SGL_ATTRTYPE_FLOAT);

### changing an attribute's value

    $bar = SGL_Attribute::getById($id);
    $bar->value = 'new value';
    $oAttrib = $bar->save();

### create new content based on an existing type

    $oRestoReview = SGL_Content::getByType('RestaurantReview');  
    
    // or by $typeId: $oRestoReview = SGL_Content::getType($typeId)
    $oRestoReview->dishName = 'Chicken Catchatorie';
    $oRestoReview->overallRating = 7.6;
    $oRestoReview->langCode = SGL::getCurrentLang();
    $oRestoReview->save();

### create new content of an existing type from form data

    //  pass data from post as arg
    $oContext = new SGL_Context($input);
    
    //  context strategy determines if Content or ContentType
    //  and maps data correctly
    //  SGL_Content object can be built from any input (php object, post, xml, etc)
    $oContent = new SGL_Content($oContext->process());

### update existing content

    $oRestoReview  = SGL_Content::getById($id);
    $oRestoReview->dishName = "Duck a l'Orange";
    $oRestoReview->save();

### get a content item

    $oExampleCV = SGL_Content::getById($id);
    print '<pre>';print_r($oExampleCV);
    
    /*
    SGL_Content Object
    (
        [id] => 7
        [name] => simple CV example
        [createdByName] => admin
        [createdById] => 1
        [updatedById] => 1
        [dateCreated] => 2006-04-19 11:43:45
        [lastUpdated] => 2006-04-19 11:43:45
        [typeId] => 15
        [typeName] => CV Format Simple
        [numLinks] => 3
        [aAttribs] => Array
            (
                [0] => SGL_Attribute Object
                    (
                        [id] => 33
                        [typeId] => 6
                        [name] => website
                        [alias] => Website
                        [value] => http://foo.com
                        [contentTypeId] => 15
                    )
    
                [1] => SGL_Attribute Object
                    (
                        [id] => 34
                        [typeId] => 1
                        [name] => email
                        [alias] => Email
                        [value] => demian@muse23.com
                        [contentTypeId] => 15
                    )
             )
    )  */

### delete a content item


    $oRestoReview  = SGL_Content::getById($id);
    $oRestoReview->delete();


### retrieve a collection of content
#### get content by type name and owner

    $aReviews = SGL_Finder::factory('content')
    	->addFilter('typeName', 'RestaurantReview')
    	->addFilter('createdBy', SGL_Session::getUid())
    	->retrieve();

#### get content by type ID (int) and owner with sort

    $aReviews = SGL_Finder::factory('content')
    	->addFilter('typeId', 123)
    	->addFilter('createdBy', SGL_Session::getUid())
    	->addFilter('sortBy', 'field_name')
    	->addFilter('sortOrder', 'ASC')
    	->retrieve();

#### get content and limit, offset and sort results

    $aReviews = SGL_Finder::factory('content')
            ->addFilter('sortBy', 'name')
            ->addFilter('sortOrder', 'ASC')
            ->addFilter('limit', array('offset' => 1, 'count' => 2))
            ->retrieve();

#### get content and limit and sort results

    $aReviews = SGL_Finder::factory('content')
            ->addFilter('sortBy', 'name')
            ->addFilter('sortOrder', 'ASC')
            ->addFilter('limit', array('count' => 2))
            ->retrieve();

#### get content and limit and sort results

    $aReviews = SGL_Finder::factory('content')
            ->addFilter('sortBy', 'name')
            ->addFilter('sortOrder', 'ASC')
            ->addFilter('limit', 2)
            ->retrieve();

#### get content and sort results by attribute value

    $aReviews = SGL_Finder::factory('content')
            ->addFilter('sortBy', array('attribute' => 'title'))
            ->addFilter('sortOrder', 'DESC')
            ->retrieve();

#### get content for a date range

    $aReviews = SGL_Finder::factory('content')
              ->addFilter('typeName', 'Glossary term')
              ->addFilter('status', SGL_CMS_STATUS_PUBLISHED)
              ->addFilter('dateCreated', array('operator' => '<', 'value' => '2009-09-01 00:00:00'))
              ->addFilter('lastUpdated', array('operator' => '>=', 'value' => '2009-08-01 00:00:00'))
              ->retrieve();

#### get content by category ID and order results

    $aReviews = SGL_Finder::factory('content')
            ->addFilter('sortBy', 'name')
            ->addFilter('sortOrder', 'ASC')
            ->addFilter('categoryId', 6)
            ->retrieve();

#### get content matching an attribute condition

    (The operator below can be any allowable in SQL, for example:
    '=','<>', '<', '>', '<=', '>=', 'LIKE' or 'IN', to match a list of possible values. 
    
    Currently, when using IN, the list must be passed in the format "('value1','value2','value3')" to function properly.)
    
    
    $attribFilter = array(
            'name'     => 'letter',
            'value'    => 'a',
            'operator' => '='
    );
    $aReviews = SGL_Finder::factory('content')
            ->addFilter('typeName', 'Glossary term')
            ->addFilter('status', SGL_CMS_STATUS_PUBLISHED)
            ->addFilter('attribute', $attribFilter)
            ->retrieve();

#### get content matching the name/title

    $aReviews = SGL_Finder::factory('content')
            ->addFilter('nameSearch', array('operator' => 'like', 'value' => 'foo'))
            ->retrieve();

#### multiple attribute filtering

    $aReviews = SGL_Finder::factory('content')
            ->addFilter('typeName', 'Article')
            ->addFilter('attribute', array(
                'name'     => 'introduction',
                'operator' => '=',
                'value'    => 'foo'))
            ->addFilter('attribute', array(
                'name'     => 'body',
                'operator' => '=',
                'value'    => 'bar'));

#### get associated child contents

    $aContents = SGL_Finder::factory('assocContent')
            ->addFilter('typeId', SGL_CMS_CONTENTTYPE_REVIEW_PRODUCT_USER)
            ->addFilter('status', SGL_CMS_STATUS_PUBLISHED)
            ->addFilter('sortBy', 'date_created')
            ->addFilter('sortOrder', 'desc')
            ->addFilter('assocContents', array('parentId' => $contentId))
            ->retrieve();

#### get associated parent contents

    $aRet = SGL_Finder::factory('assocContent')
            ->addFilter('sortBy', 'content_id')
            ->addFilter('sortOrder', 'DESC')
            ->addFilter('assocContents', array('childId' => 8))
            // get articles only
            ->addFilter('typeId', 1)
            ->retrieve();

#### other associated content methods


    $links = SGL_Content::getById($id)
            ->hasLinks();


    $count = SGL_Content::getById($id)
            ->getLinkCount();


    $aAssocContents = SGL_Content::getLinkedContents();


    $aComments = SGL_Content::getByType(SGL_CMS_CONTENTTYPE_ARTICLE)
            ->getLinkedContents($typeId = SGL_CMS_CONTENTTYPE_COMMENT);


#### get associated media contents
First your content type must include an attribute of type 'file'.  Then in the admin view you must associate the media with the content instance by uploading it.  Then when you want to access the media, specify the name of the field you created in your Finder query like this:

    $aRet = SGL_Finder::factory('content')
            ->addFilter('categoryId', 6)
            ->addFilter('mediaInfo', array('name' => 'myProfilePic'));
            ->addFilter('mediaInfo', array('name' => 'myProfileThumb'));
            ->retrieve();

You can then iterate through the resultset like this:

    foreach ($aContents as $oContent) {
        // to access file name
        $oContent->media->myProfilePic->file_name
    
        // to access mime type
        $oContent->media->myProfileThumb->mime_type
    }



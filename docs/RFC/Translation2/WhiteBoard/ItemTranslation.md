<!-- Name: RFC/Translation2/WhiteBoard/ItemTranslation -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/07/14 19:19:34 -->
<!-- Author: lakiboy -->
# SGL_ItemTranslation
## Purpose
IMO current _SGL_Translation_ functionality is poor to build real multilingual sites easy. In every manager a developer has to identify same languageID, pageID, fallback language etc. *SGL_ItemTranslation* tries to ease the development of multilingual managers and is built on top of _SGL_Translation_.

Sure, the _SGL_ItemTranslation_ implementation is not the best way to extend _SGL_Translation_ and must be improved or maybe totally discarded, but it still could help to simplify your life.


## Idea
Building new manager a developer identifies:
 * pageID for a manager's item,
 * table fields (also required fields) to translate by translation class,
 * validation code (in ManagerName::validate()).

Other operations (_SGL_Translation_ methods calls) are left to _SGL_ItemTranslation_.
Typically a website requires just several languages to operate with. This class provides a way to edit all content versions (all languages) at the time.    But that doesn't mean, that a webmaster has to fill all content (for all languages) at one time. The only values, that must be identified, are required fields of fallback language. In case a webmaster entered just a fallback translation, but current language is not a fallback one, all required fields values will be replaced with values of fallback language. Non-required fields are left blank.

## Install
The best way to understand how it works is to try.
 * unpack attached archive,
 * copy files to SGL dirs,
 * (re)install SGL,
 * login as admin,
 * go to http://path-to-sgl/index.php/transdemo/testadmin/.

_Transdemo_ is a demo module, which was created by ModuleGenerator with custom generation templates and then extended to use _SGL_ItemTranslation_ functionality. The demo table has two fields to work with: name and description, where name field is required.

## Notes
 * All languages have to be converted to same encoding i.e. UTF-8, otherwise there is no sense to use this class.
 * *SGL_ItemTranslation::translateAll()* method is quite heavy one for DB, 'cos it queries database as many times, as many records it tries to translate (I have an idea how to avoid this.)
 * This class is experimental and may not solve your problems. As mentioned above, SGL gurus may totally reject the way it works and suggest another solution. Anyway all comments are welcome.
 * Do not use provided _transdemo_ module if my _imagedemo_ module is installed. Both modules have the same test manager. :)

## Comments




## Code examples
### Initialization

    require_once dirname(__FILE__) . '/DA_Transdemo.php';
    
    ...
    
        function validate($req, &$input)
        {
            ...
            $this->init(); // init lang translation support
            if ($input->submitted) {
                ...
            }
        }
    
        function init()
        {
            $this->tr = & new SGL_ItemTranslation(SGL_MODULE_TRANSDEMO_TRANSID_ITEM);
            $this->tr->setFields(array('name', 'description'));
            $this->tr->setFields(array('name'), $required = true);
        }
### List data


        function _cmd_list(&$input, &$output)
        {
            ...
            $aPagedData = SGL_DB::getPagedData($this->dbh, $query, $pagerOptions);
            if (!empty($aPagedData['data'])) {
                $this->tr->translateAll($aPagedData['data']); // HEAVY !!!
                $output->aPagedData = $aPagedData;
                $output->pager      = $aPagedData['totalItems'] > $limit;
            }
            ...
        }
### Insert data


        function _cmd_insert(&$input, &$output)
        {
            ...
            // prepare translation / set fallback
            $this->tr->prepare($input->oItem);
           
            SGL_DB::setConnection();
            $oItem = &DB_DataObject::factory($this->conf['table']['trans_demo']);
            $oItem->setFrom($input->oItem);
            ...
            $oItem->insert();
            
            // insert translation
            $this->tr->insert($oItem->trans_id);
            ...
        }

### Delete data


        function _cmd_delete(&$input, &$output)
        {
            ...
            foreach ($input->aDelete as $id) {
                $oItem = &DB_DataObject::factory($this->conf['table']['trans_demo']);
                $oItem->get($id);
                
                // delete translation
                $this->tr->remove($oItem->trans_id);
                
                $oItem->delete();
                unset($oItem);
            }
            ...
        }




<!-- Name: RFC/Translation2/ImportMgr -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/04/12 17:17:17 -->
<!-- Author: werner -->
*USE AT YOUR OWN RISK*

  1. Put this in modules/custommaintenance/classes. 
  2. Add to module table using ModuleMgr
  3. Disable authenication
  4. Run action

  * _add - add's languges to translation2 lang table
  * _list - imports languages from file to db
  * _import - adds missing keys from english to target language


    
    <?php
        
    class CustommaintenanceMgr extends SGL_Manager
    {
        function CustommaintenanceMgr()
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            $this->module       = 'custommaintenance';
            $this->pageTitle    = 'maintenance';
            $this->template     = 'docBlank.html';
    
            $this->_aActionsMapping =  array(
                'list'      => array('list'),
                'add'       => array('add'),
                'import'    => array('import'),
            );
        }
    
        function validate($req, &$input)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            $this->validated    = true;
            $input->error       = array();
            $input->pageTitle   = $this->pageTitle;
            $input->masterTemplate = $this->masterTemplate;
            $input->template    = $this->template;
            $input->action      = $req->get('action');
        }
         
        function _list(&$input, &$output) 
        {
            $translation = &SGL_Translation::singleton('admin');
            
            $data['langs'] = array('en_iso_8859_15' => 'english-iso-8859-15.php', 
                    'de_iso_8859_1' => 'german-iso-8859-1.php', 
                    'ru_win1251' => 'russian-windows-1251.php', 
                    'es_iso_8859_1' => 'spanish-iso-8859-1.php');
            $aModuleList = array('custommaintenance');
        
            foreach ($data['langs'] as $langID => $globalLangFile) {
            //  interate through languages adding to langs table
                                        
                
                //  interate through modules                    
                foreach ($aModuleList as $module) {
                    $modulePath = SGL_MOD_DIR . '/' . $module  . '/lang';                    
        
                    if (file_exists($modulePath .'/'. $globalLangFile)) {
                        //  load current module lang file
                        require $modulePath .'/'. $globalLangFile;
            
                        //  defaultWords clause
                        $words = ($module == 'default') ? $defaultWords : $words;                            
                        
                        //  add current translation to db
                        foreach ($words as $tKey => $tValue) {                                                                                          
                                $string = array($langID => $tValue);
                                $translation->add($tKey, $module, $string);                                    
                        }
                        unset($words);                            
                    }
                }
            }
        }
    
        function _add(&$input, &$output)
        {
            require_once 'Translation2/Admin.php';                
    
            $conf = &$GLOBALS['_SGL']['CONF'];
            //  get dsn
            $dsn = SGL_DB::getDsn('SGL_DSN_ARRAY');
            
            //  set translation2 params
            $params = array(
                'langs_avail_table' => 'langs',
                'lang_id_col'       => 'lang_id',
                'string_id_col'      => 'translation_id',
            );
    
            //  set tranlsation2 driver
            $driver = 'DB';
            
            //  instantiate translation2_admin object
            $translation = & Translation2_Admin::factory($driver, $dsn, $params);
    
            
            $langs = array('sv_iso_8859_1' => array('name' => 'Swedish (sv-iso-8859-1)', 'encoding' => 'sv-iso-8859-1'));
    
            foreach ($langs as $langID => $langVal) {
                $langData       = array(
                                    'lang_id' => $langID,
                                    'table_name' => $conf['table']['translation'] .'_'. $langID,
                                    'meta' => '',
                                    'name' => $langVal['name'],
                                    'error_text' => 'not available',
                                    'encoding' => $langVal['encoding']
                                    );
                $result = $translation->addLang($langData);
            }        
        }
        
        function _import(&$input, &$output)
        {
            require_once SGL_MOD_DIR . '/default/classes/ModuleMgr.php';
            $output->aModules = ModuleMgr::retrieveAllModules(SGL_RET_NAME_VALUE);
    
            $translation = &SGL_Translation::singleton('admin');
            
            foreach ($output->aModules as $module => $moduleName) {
                //  retreive source array
                $aSourceLang = SGL_Translation::getTranslations($module, 'en_iso_8859_15');
        
                //  retreive target array
                $aTargetLang = SGL_Translation::getTranslations($module, 'sv_iso_8859_1');                                    
    
                $aDiff = array_diff(array_keys($aSourceLang), array_keys($aTargetLang));
                foreach ($aDiff as $key => $value) {
                    $string['sv_iso_8859_1'] = '';    
                    $translation->add($value, $module, $string);                                    
                }
            }
        }
    }
    ?>
    
    
<!-- Name: Howto/Navigation/ModifyingCategoryMenu -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/01/30 12:37:17 -->
<!-- Author: werner -->
[[TOC]]
# How to modify the category menu block

## Structure
In the category-menu Block (e.g. CategoryNav.php or ShopNav.php) you can define the look and feel of your navigation block.
It works like this:


    #!php
    <?php
        function getBlockContent()
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            $theme = $_SESSION['aPrefs']['theme'];
            require_once SGL_MOD_DIR . '/navigation/classes/MenuBuilder.php';
            $menu = & new MenuBuilder('ExplorerBsd');
            $menu->setStartId(0);
            $html = '<img src="' . SGL_BASE_URL . '/themes/' . $theme . '/images/pixel.gif" width="165" height="1" alt="" /><br />';
            $html .= $menu->toHtml();
            return $html;
        }
    ?>

Now you can change the look and feel by making a new class for the menu builder.

Change in the block the line 

"$menu = & new MenuBuilder('ExplorerBsd');"

to 

"$menu = & new MenuBuilder('Example');"

Next you have to add this class to _modules/navigation/classes/MenuBuilder.php_ line 102:

    #!php
    <?php
            case 'menu_explorerbsd': case 'menu_example':
                $ret = $this->GUI->render($this->_startId);
                break;
    ?>

## Custom Menu Builder

Last but not least you have to create your menubuilder class _modules/navigation/classes/menu/Example.php_ where you can specify your special look and feel of the categories:


    #!php
    <?php
    class Menu_Example
    {
        var $module = 'navigation';
    
        function Menu_Example($options, $conf)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
            $this->conf = $conf;
        }
    
        function render($id = 0)
        {
            $menu = $this->getGuruTree($id);
            $html = $menu->printMenu();
            return $html;
        }
    
        function getGuruTree($id = 0)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
    
            //  style definition .treeMenuDefault in <head>
            $tree = &$this->createFromSQL($id);
    
            //  initialise the class options
            require_once 'HTML/TreeMenu.php';
    
            //  build url for current page
            $req = & SGL_Request::singleton();
            $url = SGL_Url::makeLink(   '',
                                        $req->get('managerName'),
                                        $req->get('moduleName')
                                        );
            $url .= 'frmCatID/';
            $nodeOptions = array(
             'text'          => '',
             'link'          => $url,
             'icon'          => 'exampleFolder.gif',
             'expandedIcon'  => 'exampleExpandedFolder.gif',
             'class'         => '',
             'expanded'      => false,
             'linkTarget'    => '_self',
             'isDynamic'     => 'true',
             'ensureVisible' => '',
             );
            $options = array(   'structure' => $tree,
                                'type' => 'heyes',
                                'nodeOptions' => $nodeOptions);
    
            $menu = HTML_TreeMenu::createFromStructure($options);
    
            require_once SGL_MOD_DIR . '/navigation/classes/HTML_TreeMenu_DHTML_SGL.php';
            $theme = $_SESSION['aPrefs']['theme'];
            $treeMenu = & new HTML_TreeMenu_DHTML_SGL($menu, array(
                'images' =>  SGL_BASE_URL . "/themes/$theme/images/imagesAlt2",
                'defaultClass'  => 'treeMenuDefault',
                'noTopLevelImages' => 'true',
                ));
            return $treeMenu;
        }
    
        /**
        * This method imports a tree from a database using the common
        * id/parent_id method of storing the structure.
        *
    
        */
        function &createFromSQL($id = 0)
        {
            SGL::logMessage(null, PEAR_LOG_DEBUG);
    
            require_once 'HTML/Tree.php';
    
            $dbh = &SGL_DB::singleton();
            $query = '  SELECT  category_id as id, parent_id, label AS text
                        FROM    ' . $this->conf['table']['category'] .'
                        ORDER BY parent_id, order_id';
            $tree     = &new Tree();
            $nodeList = array();
    
            // Perform query
            $result = $dbh->query($query);
            if (!PEAR::isError($result)) {
                while ($row = $result->fetchRow(DB_FETCHMODE_ASSOC)) {
    
                    // Parent id is 0, thus root node.
                    if (!$row['parent_id']) {
                        unset($row['parent_id']);
                        $nodeList[$row['id']] = &new Tree_Node($row);
                        $tree->nodes->addNode($nodeList[$row['id']]);
    
                    // Parent node has already been added to tree
                    } elseif (!empty($nodeList[$row['parent_id']])) {
                        $parentNode = &$nodeList[$row['parent_id']];
                        unset($row['parent_id']);
                        $nodeList[$row['id']] = &new Tree_Node($row);
                        $parentNode->nodes->addNode($nodeList[$row['id']]);
    
                    } else {
                        // Orphan node ?
                    }
                }
            } else SGL::raiseError('problem with getGuruTree query');
    
            // jump into the cat tree at a predefined depth
            // if $id = 0 return the hole tree OR if $id != 0 return from $id branch
            $result = ($id) ? $nodeList[$id] : $tree;
            return $result;
        }
    }
    ?>

This is just a slightly modification of ExplorerBSD menu builder. Of course you can add whatever you want.

## Options

When calling "$treeMenu = & new HTML_TreeMenu_DHTML_SGL();" you can pass some options to the class:

 *  o images            -  The path to the images folder. Defaults to "images"
 *  o linkTarget        -  The target for the link. Defaults to "_self"
 *  o defaultClass      -  The default CSS class to apply to a node. Default is none.
 *  o usePersistence    -  Whether to use clientside persistence. This persistence is achieved using cookies. Default is true.
 *  o noTopLevelImages  -  Whether to skip displaying the first level of images if there is multiple top level branches.
And also a boolean for whether the entire tree is dynamic or not. This overrides any perNode dynamic settings.

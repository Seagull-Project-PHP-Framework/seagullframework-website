<!-- Name: Howto/Navigation/HtmlMenu -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/03/02 12:16:14 -->
<!-- Author: demian -->
# How To Create A Navigation Driver with PEAR's HTML_Menu

[Taken from http://www.phpmag.net/itr/kolumnen/psecom,id,26,nodeid,207.html - sorry guys, there doesn't seem to be any direct link to the article :-/]

## Package in Focus: DB_NestedSet

In this week's package in focus we look at the DB_NestedSet package. NestedSet is useful for creating a data tree and storing it in a relational database. The difference between DB_NestedSet and Tree, is that NestedSet focuses on using a relational database as the storage container and uses different algorithms to increase performance.

The downside of DB_NestedSet is the lack of good documentation. The API documentation is quite complete and even tries to go through the available sample documentation to create a simple example. Because DB_NestedSet doesn't provide any native data outputting mechanism, I've chosen to use the HTML_Menu driver for our example.

Before getting started, you will need to create a database, and create a couple tables to store our data. Following is the SQL for the basic tables: 


    DROP TABLE IF EXISTS `nested_set`;
    CREATE TABLE `nested_set` (
      `id` int(10) unsigned NOT NULL default '0',
      `parent_id` int(10) unsigned NOT NULL default '0',
      `order_num` tinyint(4) unsigned NOT NULL default '0',
      `level` int(10) unsigned NOT NULL default '0',
      `left_id` int(10) unsigned NOT NULL default '0',
      `right_id` int(10) unsigned NOT NULL default '0',
      `name` varchar(60) NOT NULL default '',
      PRIMARY KEY  (`id`),
      KEY `right` (`right_id`),
      KEY `left` (`left_id`),
      KEY `order` (`order_num`),
      KEY `level` (`level`),
      KEY `parent_id` (`parent_id`),
      KEY `right_left` (`id`,`parent_id`,`left_id`,`right_id`)
    ) TYPE=MyISAM;
    
    ~~
    ~~ Table structure for table `nested_set_locks`
    ~~
    
    DROP TABLE IF EXISTS `nested_set_locks`;
    CREATE TABLE `nested_set_locks` (
      `lockID` char(32) NOT NULL default '',
      `lockTable` char(32) NOT NULL default '',
      `lockStamp` int(11) NOT NULL default '0',
      PRIMARY KEY  (`lockID`,`lockTable`)
    ) TYPE=MyISAM COMMENT='Table locks for comments';

The table 'nested_set' will contain the data for the structure that we will create. You can name the table fields anything you like, but I've chosen to use these verbose table names to give you an idea of what the field does. We will "tell" DB_NestedSet to use these fields in the code.

In our example we will be making a simple menu for our site, so we will need a new row called "title", which will hold the title of the page associated with that node. To do this, simply run the following sql: 


    alter table nested_set add url varchar(255);

Now it's time for the code. I'm not going to go through everything that DB_NestedSet can do, but hopefully the comments in the code will put you on the path to understanding. I will be using some code from HTML_Menu to display our example, but will not go into detail to explain what it is doing.


    require_once('HTML/Menu.php');
    require_once('DB/NestedSet.php');
    require_once('DB/NestedSet/Output.php');
    
    // We start out by defining our database table specifications
    $dsn = 'mysql://root:@localhost/nested';
    
    // next we let the point our the table fields to the expected fields
    $table = array(
        'id'        => 'id',
        'parent_id' => 'rootid',
        'left_id'   => 'l',
        'right_id'  => 'r',
        'order_num' => 'norder',
        'level'     => 'level',
        'name'      => 'name',
        'title'     => 'title'
    );
    
    // And then create a NestedSet instance using the DB driver with our Database and Table
    
    $nestedSet =& DB_NestedSet::factory('DB', $dsn, $table); 
    
    // Then set the names of our table, and how to sort the nodes.
    // We have chosed to sort on 'name' but you can use any field from the table.
    $nestedSet->setAttr(array(
            'node_table' => 'nested_set', 
            'lock_table' => 'nested_set_locks', 
            'secondarySort' => 'name'
            ];
            
    // If a nodeID is specified we print out the name and title
    if(isset($_GET['nodeID']]{
        $node_data = $nestedSet->pickNode($_GET['nodeID']);
        echo "<title>$node_data->name :: $node_data->title</title>";
    }
    
    // Now we define our data set. This only needs to be done one time.
    // Once the information is in your database you will only need to touch it if
    // You want to edit it.
    
    // Create a parent item
    
    $parent = $nestedSet->createRootNode(array('name'  => "Menu",
                                               'title' => "Example Menu"), false, true);
    
    // The $parent var now holds the ID of the root node, we will use this when 
    // createing subnodes
    
    // Create a Subnode for our Italian recipes. We define the parent node first,
    // and then capture the id of this node to use when creating subnodes from this
    // node
    $italian = $nestedSet->createSubNode($parent, array('name'  => 'Italian',
                                                        'title' => 'Great Food'];                                                      
    $greek = $nestedSet->createSubNode($parent, array('name'  => 'Greek',
                                                      'title' => 'My Favorite'];
                                                          
    
    $indian = $nestedSet->createSubNode($parent, array('name'  => 'Indian', 
                                                      'title' => 'The spicier the Better'];
          
    $pizza = $nestedSet->createSubNode($italian, array('name' =>'Pizza'];
    $nestedSet->createSubNode($pizza, array('name' =>'Pepperoni'];
    
    $nestedSet->createSubNode($pizza, array('name' => 'Quattro Stagioni'];    
        
    // Now create some leaf nodes of our indian recipes
    $nestedSet->createSubNode($indian, array('name' =>'Butter Chicken'];
    $nestedSet->createSubNode($indian, array('name' =>'Tandoori Chicken'];
    $nestedSet->createSubNode($indian, array('name' =>'Masala Dosa'];
    
    // And Greek recipes
    
    $nestedSet->createSubNode($greek, array('name' => 'Moussaka'];
    
    // Now our structure is created, if you look in the database you'll be able
    // to see how the DB_NestedSet stores this information. The next step is to
    // retrieve the data and present it in a pretty format. From now on we will 
    // be working with HTML_Menu for the presentation.
    
    // Fetch the entire tree into an array
    $data = $nestedSet->getAllNodes(true);
    
    // Create the links
    foreach ($data as $id => $node) {
         $data[$id]['url'] = $_SERVER['PHP_SELF'].'?nodeID=' . $node['id'];
    }
    
    // Send HTML_Menu the structure, title and URL for the items in our tree
    $params = array(
        'structure' => $data,
        'titleField' => 'name',
        'urlField' => 'url');
    
        
    // Create the output driver object	
    
    $output =& DB_NestedSet_Output::factory($params, 'Menu');
    
    // Fetch the menu array
    $structure = $output->returnStructure();
    
    // We'll create the new HTML_Menu object, using the 'sitemap' display driver
    $menu = & new HTML_Menu($structure, 'sitemap');
    
    // Create the current URL and send it to HTML_Menu
    $currentUrl = $_SERVER['PHP_SELF'].'?nodeID=' . $_GET['nodeID'];
    $menu->forceCurrentUrl($currentUrl);
    
    // Show the menu
    
    $menu->show();
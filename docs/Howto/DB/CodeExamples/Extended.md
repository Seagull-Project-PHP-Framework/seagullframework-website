<!-- Name: Howto/DB/CodeExamples/Extended -->
<!-- Version: 5 -->
<!-- Last-Modified: 2007/01/31 16:06:59 -->
<!-- Author: lakiboy -->
## DB Queries
[[TOC]]

### Intro
Seagull uses [PEAR::DB](http://pear.php.net/manual/en/package.database.php) as its abstraction layer which gives the developer a considerable amount of flexibility to migrate SQL across DB vendors.  It also makes querying the database quite a bit easier/cleaner than using just the PHP native SQL functions.

### Overview
A few of the main things to keep in mind with Seagull's use of PEAR::DB are:
  * sequences must always be used, forget about MySQL's auto-increment feature, this will make it much more difficult to move your SQL to another DB, say Postgres, in the future
  * Seagull overrides the standard PEAR::DB way of getting the next id from a given sequence.  The API is the same, however we put all sequences in a single table to avoid huge numbers of tables in the DB (Choose MySQL_SGL at installation).  You can however revert back to the standard PEAR approach to MySQL sequences, this is left up to the user
  * PEAR::DB gives three choices for the format of returned results: DB_FETCHMODE_ORDERED, DB_FETCHMODE_ASSOC, DB_FETCHMODE_OBJECT - Seagull uses the object mode by default however you can override this at any time
  * Seagull allows you to query multiple databases from a single 'page', just pass a unique $dsn to each SGL_DB::singleton() instance.
  * all queries (unless multiple DBs are used) are run against a single database resource, this is enforced by using a Singleton connection resource

### Returning Appropriate Data Structures

#### Array of Objects
the following query:


        $dbh = & SGL_DB::singleton();
        $query = '
            SELECT
                b.block_id, b.name, b.title, b.title_class, 
                b.body_class, b.is_onleft
            FROM    block b, block_assignment ba
            WHERE   is_enabled = 1
            AND     b.block_id = ba.block_id
            AND     ( ba.section_id = 0 OR ba.section_id = ' . 
                    $this->_currentSectionId . ' )
            ORDER BY blk_order
        ';
        $aResult = $dbh->getAll($query);

returns the following data structure



     Array
    (
        [0] => stdClass Object
            (
                [block_id] => 10
                [name] => SampleRightBlock1
                [title] => Sample Right Block
                [title_class] => 
                [body_class] => 
                [is_onleft] => 0
            )
    
        [1] => stdClass Object
            (
                [block_id] => 1
                [name] => SiteNews
                [title] => Site News
                [title_class] => 
                [body_class] => 
                [is_onleft] => 1
            )
        [...] 

#### Single Value
the following query:


        $dbh = & SGL_DB::singleton();
        $query = "  SELECT label 
                    FROM " . $conf['table']['category'] . "
                    WHERE category_id = '$id'";  
        $result = $dbh->getOne($query);

returns the following data structure


    myLabel

#### Single Row
the following query:


        $dbh = & SGL_DB::singleton();
        $query = "
            SELECT  usr_id, user_group_id
            FROM " . $conf['table']['user'] . "
            WHERE   username = " . $dbh->quote($username) . "
            AND     passwd = '" . md5($password) . "'
            AND     is_acct_active = 1
            AND     user_group_id <> " . SGL_UNASSIGNED;
        $aResult = $dbh->getRow($query, DB_FETCHMODE_ASSOC);

returns the following data structure


    Array
    (
        [usr_id] => 2
        [user_group_id] => 2
    )

#### Array of Arrays
the following query:


        $dbh = & SGL_DB::singleton();
        $query = "  SELECT category_id, label 
                    FROM " . $conf['table']['category'] . "
                    WHERE parent = $id";
        $result = $dbh->query($query);
        $count = 0;
        $aChildren = array();
        while ($row = $result->fetchRow(DB_FETCHMODE_ASSOC)) {
            $aChildren[$count]['category_id'] = $row['category_id'];   
            $aChildren[$count]['label'] = $row['label'];
            $count++;
        }

returns the following data structure


    Array
    (
        [0] => Array
            (
                [category_id] => 1
                [label] => Sample Category
            )
    
        [1] => Array
            (
                [category_id] => 4
                [label] => General
            )
    
        [2] => Array
            (
                [category_id] => 115
                [label] => Javascript tutorial
            )
    
        [3] => Array
            (
                [category_id] => 118
                [label] => Planets
            )
    
        [4] => Array
            (
                [category_id] => 300
                [label] => Continents
            )
    
    )

#### Hash
the following query:


        $dbh = & SGL_DB::singleton();
        $query = "
             SELECT  i.item_id,
                     ia.addition
             FROM    item i, item_addition ia, item_type it, item_type_mapping itm
             WHERE   ia.item_type_mapping_id = itm.item_type_mapping_id
             AND     it.item_type_id  = itm.item_type_id
             AND     i.item_id = ia.item_id
             AND     i.item_type_id = it.item_type_id
             AND     itm.field_name = 'title'
             AND it.item_type_id  = '5'         /* Static Html Article */
             AND i.status  > " . SGL_STATUS_DELETED . "
             ORDER BY i.last_updated DESC
        ";
        $res = $dbh->getAssoc($query);

returns the following data structure


    Array
    (
        [1] => Content Reshuffle
        [3] => Little
        [6] => Mary
        [11] => Had a Lamb
    )

### Prepared Queries

Prepared queries can save you a lot of time, for example:


        $dbh = & SGL_DB::singleton();
        $sth = $dbh->prepare("  UPDATE " . $conf['table']['user'] . "
                                SET user_group_id = $gid
                                WHERE usr_id = ?");
        foreach ($aUsers as $uid => $username) {
            //  if attempt to remove admin (uid = 1), silently ignore
            if ($uid == 1) {
                continue;
            }
            $dbh->execute($sth, $uid);
        }


=== Object Relational Mapping === 
with DB_DataObject

Many PEAR developers prefer to use the [DataObject](http://pear.php.net/manual/en/package.database.db-dataobject.php) library which abstracts away the SQL into an object interface, there are [many tutorials](http://www.phpkitchen.com/index.php?/archives/668-PEAR-Tutorials.html) on the subject, but a short example would be:


        $oUser = & new DataObjects_Usr();
        //  get limit and totalNumRows
        $totalNumRows = $oUser->count();
        $limit = $_SESSION['prefs']['resPerPage'];
        $oUser->orderBy($input->sortBy . ' ' . $input->sortOrder);
        $oUser->limit($input->from, $limit);
        $oUser->orderBy('date_created DESC');
        //  execute query
        $numRows = $oUser->find();
        $aUsers = array();
        if ($numRows > 0) {
            while ($oUser->fetch()) {
                $oUser->getLinks('link_%s');
                $aUsers[] = clone($oUser);
            }
            return $aUsers;
        }

### Checking for DB errors
If an error occurs, PEAR::DB will return an error Object. You can easily check for it like:


    // $aRes is the result of the query
    
    if (!PEAR::isError($aRes)) {
        return $aRes;
    } elseif (PEAR::isError($aRes, DB_ERROR_NOSUCHTABLE)) {
        SGL::raiseError('You have a Seagull database with no tables ...',
            SGL_ERROR_NODATA, PEAR_ERROR_DIE);
    } else {
        SGL::raiseError('Unknown DB error occurred, pls file bug',
            SGL_ERROR_NODATA, PEAR_ERROR_DIE);
    }


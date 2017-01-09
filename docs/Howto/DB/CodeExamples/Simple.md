<!-- Name: Howto/DB/CodeExamples/Simple -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/09/12 17:58:07 -->
<!-- Author: demian -->
## DB
[[TOC(Code/Examples/DB)]]

### Initialising a Connection
You only need to initalize a database connection on rare occasions. The property $this->dbh is automatically initialized for use in Managers (ie. DefaultMgr) when parent::SGL_Manager(); is executed in your manager's constructor.


        $locator = &SGL_ServiceLocator::singleton();
        $dbh = $locator->get('DB');
        if (!$dbh) {
            $dbh = & SGL_DB::singleton();
            $locator->register('DB', $dbh);
        }

### Direct database queries
Database queries using PEAR::DB 


        $query = "
            SELECT  id, gid
            FROM " . $conf['table']['user'] . "
            WHERE   username = " . $dbh->quote($input->username) . "
            AND     passwd = '" . md5($input->password) . "'
            AND     is_acct_active = 1
            AND     gid <> " . SGL_UNASSIGNED;
        $aResult = $dbh->getRow($query, DB_FETCHMODE_ASSOC);

=== Prepared queries === 
using PEAR::DB

        $sth = $dbh->prepare("  
            UPDATE " . $conf['table']['user'] . "
            SET gid = $gid
            WHERE id = ?");
        foreach ($aUsers as $uid => $username) {
            if ($uid == SGL_ADMIN) {
                continue;
            }
            $dbh->execute($sth, $uid);
        }

=== Adding a user object === 
Adding a user object with 15+ fields to permanent storage (DB_DataObject), taking into account config variables, using locale-specific time values, employing sequences for database independence

        $oUser = DB_DataObject::factory($this->conf['table']['user']);
        $oUser->setFrom($input->user);
        $oUser->passwd = md5($input->user->passwd);
        if ($conf['registermgr']['autoEnable']) {
            $oUser->is_acct_active = 1;
        }
        $dbh = $oUser->getDatabaseConnection();
        $oUser->id = $dbh->nextId('usr');
        $oUser->date_created = $oUser->last_updated = SGL::getTime();
        $success = $oUser->insert();


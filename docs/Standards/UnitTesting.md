<!-- Name: Standards/UnitTesting -->
<!-- Version: 18 -->
<!-- Last-Modified: 2009/10/03 22:07:25 -->
<!-- Author: lyric -->
# Unit Testing
[[TOC]]
## Using SimpleTest

There is a suite of unit tests available for Seagull, they are being completed in order of importance:
 * unit tests for core libs in [source:trunk/lib/SGL]
 * web tests to ensure screens load without errors for each module
 * unit tests for each module

File Types
 * .wdb.php - DB with tables
 * .wdd.php - DB with tables and data
 * .ndb.php - PHP only
 * .web.php - a web test, ie parse results from an HTTP request

Included in a default Seagull install is a test runner tool that runs the test suite in a browser.  In order to run the tests you need to use the PEAR package manager to install the SimpleTest library:

  * *PREREQUISITES*:
    * installed PEAR package manager ([http://pear.php.net/manual/en/installation.getting.php How do I install the PEAR package manager?])
    * your system PEAR repository should be included in the include_path directive of your php.ini (usually done during PEAR package manager installation)
    * if you installed PEAR just now, don't forget to restart your webserver (include_path, you know) before trying to run some tests
  * *STEP 1*: install the following packages in your system PEAR repository.  They should not go in Seagull's PEAR repo.


    $ pear channel-discover pear.phpkitchen.com
    $ pear install phpkitchen/SimpleTest
    $ pear install HTML_TreeMenu

  * *STEP 2*: navigate to http://myhost/seagull/tests/ in a browser[[BR]]
    ''Note: This doesn't work on virtual hosts where the webroot is ''seagull/www''. Install a seperate virtual host instead and create a new ''conf.ini'' for it''
  * *STEP 3*: The first time you run the test runner a test.conf.ini file is copied to your seagull/var directory it should contain something similar to the following:


    ;------------------------------------------------------------------------------------------;
    ; Test Environment Settings - Make Sure The Following Are Correct!                         ;
    ;------------------------------------------------------------------------------------------;
    
    [database]
    type                = mysql_SGL
    host                = localhost
    port                = 3306
    user                = root       ; enter the appropriate user of your database
    pass                =            ; enter the appropriate password of that user
    name                = simpletest ; Don't set this to be your project database - the test
                                     ; database is often dropped during testing!
    [project]
    name                = My Project
    projectMnemonic     = _SGL       ; use this to override global vars
    
    [general]
    directoriesToScan   = modules,lib/SGL
    initBridge          = /etc/sglBridge.php
    
    ; Path should be relative to the tests install directory
    [schemaFiles]
    file1               = etc/sequence.my.sql
    file2               = modules/default/data/schema.my.sql
    file3               = modules/user/data/schema.my.sql
    file4               = modules/block/data/schema.my.sql
    
    [dataFiles]
    file1               = modules/default/data/data.default.my.sql
    file2               = modules/user/data/data.default.my.sql
    file3               = modules/block/data/data.default.my.sql

*Note:* _If you are using Seagull with PostgreSQL, you will have to copy manually tests/test.pg.conf.ini-dist to var/test.conf.ini.php_

That should be enough to get you up and running.  In the browser, clicking on any node in the left hand nav will execute the tests for the current node and all its children. The framework should work to simplify Unit Testing for any PHP project. In a browser the [attachment:wiki:Standards/UnitTesting:unit_tests.png test suite should look like this].

*Note:* _Make sure that you have the module installed in module manager or else you will not see the test files in the menu tree!_

### Customising Your Test Suite
As you create new modules, you will want to add unit and web tests for them.  The test runner depends on a test database which is built for each suite execution based on data specified in your ini file, as per above.  The test database is always called simpletest.  When you create a new module you should create 3 SQL files in the /modules/$module/data folder, the schema, default data, and sample data respectively.  Add these lines to the config above for the data to be loaded by the test runner, eg:


    [schemaFiles]
    file5               = modules/foo/data/schema.my.sql
    
    [dataFiles]
    file4               = modules/foo/data/data.default.my.sql
    file5               = modules/foo/data/data.sample.my.sql

## Using Selenium for Acceptance Tests
 * grab Selenium core http://www.openqa.org/selenium-core/download.action
 * unzip and place folder in www directory, renaming to selenium-core
 * launch Seagull test running in the normal way
 * Launch Selenium Acceptance Test Runner
 * hit the 'run all tests' button, top right frame, leftmost button

To write your own acceptance tests
 * record tests with Selenium IDE in Firefox http://www.openqa.org/selenium-ide/

## Using PHPUnit in Zend Studio 7
Despite being a rubbish IDE, Zend Studio 7 has done a nice job of integrating unit tests with PHPUnit.  But it's not that obvious how to create a folder of tests for your libs.  Here's an approach I took that worked quite well:
 * create a new 'tests' folder on same level as your lib folder
 * right click, select "new unit test"
 * add phpunit 3.x to path, under the tab 'libraries' in the 'customise paths option'
 * name test manually, autonaming is not great
 * ideally create a test suite running file
 * from the UI, click "Run all tests", the green button.  You can even create a key mapping to be able to run all tests in one keystroke.

## Further Reading

 * [SimpleTest Documentation from lastcraft.com](http://www.lastcraft.com/simple_test.php)
 * [Developer's API for SimpleTest](http://simpletest.sourceforge.net)
 * http://qa.php.net/write-test.php
 * [wikipedia site on Test Driven Development](http://en.wikipedia.org/wiki/Test_driven_development)
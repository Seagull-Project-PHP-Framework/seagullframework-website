<!-- Name: Standards/CodingStandards -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/02/13 13:28:00 -->
<!-- Author: demian -->
# Seagull's Coding Standards
[[TOC]]
Seagull follows [PEAR coding standards](http://pear.php.net/manual/en/standards.php) as much as possible, you can view our coding standards doc here [source:trunk/CODING_STANDARDS.txt]  

Some highlights from the coding standards that you may not be used to are:

## PHP
  * all variables follow the bumpy caps notation, ie $thisIsMyVariable and not $this_is_my_variable 
  * all class names start with a capital letter and are then followed with bumpy caps 
  * like Java, each file should contain one class and the filename must be the same as the class name 
  * constants are capitalised with multiple words separated by underscores
  * things like 'true' and 'null' are always lower case 
  * uninitialised variables must be tested in order to avoid PHP giving off E_NOTICE errors.
  * the majority of classes are commented with PHPdoc notation which is essentially the same as Javadoc 
  * require_once and include_once do not use parentheses as they are not functions 
  * code is commented as much as possible for the benefit of others 
  * php short tags, ie <?, are not used as they are incompatible with XML processing 
  * descriptive variable names are used whenever possible to increase readability, descriptive but not verbose like you often seen in java 
  * double quotes, ie "string", are not used unless it is unavoidable as they incur extra overhead by required PHP to parse their string contents
  * the ampersand, & is always used for instantiating new objects, this of course will not be necessary in PHP5
  * lines of code must not be longer than 80 characters

## SQL
As with the php coding style, there are also a few rules you should follow when designing a new table:
  * all table & column names in lowercase
  * primary key name is: tablename_id
  * don't use MySQL's autoincrement, use sequences provided by PEAR::DB instead
  * entities must be referred to in the singular, not plural, so use 'book' instead of 'books'
  * table name relations must be separated by underscores, ie, user_preference
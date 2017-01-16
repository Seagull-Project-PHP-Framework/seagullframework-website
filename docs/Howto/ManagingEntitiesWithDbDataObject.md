<!-- Name: Howto/ManagingEntitiesWithDbDataObject -->
<!-- Version: 5 -->
<!-- Last-Modified: 2009/07/28 10:48:45 -->
<!-- Author: demian -->
<!-- Status: Original -->

# Working With Entities
* TOC
{:toc}

Entities are simply the code representations of the main concepts in your application, the data structures you're going to be working with.  If you already know about entities, please skip down to the DB\_DataObjects section.

## What are Entities?
If you're building a basic web app, let's say a blog, then you're likely dealing with some of the following entities:
  * article
  * user
  * comment

What about article date, or user name?  These are *attributes*.  Note that entities are always referred to in the singular. For more detail on the difference between entities and attributes see [this article][1].  Another concept that goes hand-in-hand with entities is collections, which are lists or collections of entities.  Examples of collections: all the articles on your homepage, all the comments associated with an article, all the results from a particular search query.

When designing an app, you probably have a set way of doing things, but chances are you're going to:

  * decide on which entities your app will be processing
  * make sure the attributes and entities are correctly distinguished 
  * create the tables in the database 
  * build the business logic that manages the data

OO design makes complex application design more manageable by providing the following concepts:

  * *data encapsulation* - each object 'holds' all the various attributes for an entity; if your objects are complex, let's say like a Car object, the data is more manageable then if you had the properties scattered all over the place.  Don't forget that your object can be comprised of other objects (composition), like in the Car example the engine attribute could be an Engine object.  All the operations associated with the Car object would be methods in the Car class. 
  * *data persistence* - your application would be pretty useless if you were not able to preserve your object modifications.  If you wanted to persist your data from page to page, say in the case of a User object, you'd just need to put it in the session.  To persist the object between application logins, like User account info, you'd need to store it in the appropriate persistent store: as a record in a DB, a file on the system hard disk, an entry in an LDAP directory, etc. 
  * *object state* - any time you use a method to modify the attributes of an object,  you are changing its state.  In OO terms all object attributes are private and can never be accessed directly from other objects, only through getter/setter methods.  In PHP4.x however, this is not enforced and indeed it is often more convenient to modify properties directly.

## DB\_DataObject
[DB\_DataObject][2] is a package from PEAR that takes care of entity management for you.  In the words of the author [Alan Knowles][3]:

DataObject performs 2 tasks:
  1. Builds SQL statements based on the objects vars and the builder methods.
  2. Acts as a datastore for a table row.

The core class is designed to be extended for each of your tables so that you put the data logic inside the data classes. Included is a generator to create your configuration files and your base classes.

At one time or another your web application is going to require you to manage your entities in some way, usually modifying the object state by performing one the the following basic operations:

  * add (INSERT) 
  * modify (UPDATE) 
  * remove (DELETE)

The term in parentheses is the SQL analogy.  What DataObjects (DO) does is to encapsulate the way you interact with your data, making it a lot easier to manage object state.  The package has the following workflow:

  * To start off, install the package in your PEAR repository (its included by default in the Seagull distro).
  * Using the entities you decided on during the design stage of your project, you create the appropriate tables in the DB.
  * Execute the DO createTables.php script which creates the entity classes in PHP for you which are analogous to the tables in your DB (This can be done in Seagull through the web interface in the Maintenance section).
  * Use the DO API to manage all your objects (entities) in a uniform way, regardless of how many attributes you add/modify/delete.


	$myObj = new DataObjects\_User();  //  instantiate a User object 
	$myObj-\>get(12);  //  get a specific User object (row in the db) 
	$myObj-\>username = 'your username here';  //  getting/setting an attribute 
	$myObj-\>update();  //  updating the object state in the DB
	$myObj-\>insert();  //  insert if it's a new object 
	$myObj-\>delete();  //  delete the object

## The Advantages
Actually DO does a lot more, but now you have the basic concept.  For more details see the [DO documentation][4]. To illustrate the usefulness of this way of working, let me give a typical before/after DataObjects scenario:

## Before DO:
  * My app would have 5 main objects in it, each object would have around 10 properties.
  * I would create the entity classes, list all the class vars, create all the add/mod/delete methods.
  * On Tuesday the client would ask me to modify 9 of the 10 attributes for an object he asked for on Monday, and add another 5;  this means I would have to: 
	* modify the table in the DB;
	* modify the class vars for the object;
	* modify all the get/set methods for each attribute;
	* modify all the add/mod/delete methods to reflect the changes.

## After DO:
  * It's Wednesday and the client is out for blood, he asks for all the attributes to be changed in the last 2 entities, and in addition wants 2 new entities, one with around 50 attributes.
  * You tell him, "no sweat", because now you're using DataObjects, so you modify your DB tables, which can be done quickly with phpMyAdmin, and regenerate your classes with the DO script.
  * Total time taken: about 10% of what it would have taken you without DataObjects.

## Summary
DB\_Dataobject lets you manage complex entities in your web applications by encapsulating the object data and allowing you to modify object state more easily.

## Linking and Joining Data Entities
DB\_DataObject Version 0.3 introduced the ability to create link ini files so you can map rows to other database columns using an ini file. This ini file should have the name '(databasename).links.ini', and be placed in the (Seagull)/var/cache/entities/ folder.

The (databasename).links.ini file contains a section for each table, then the primary and foreign key mappings for each.

If you use a 'full stop' in the key (link from column), getLinks() will look up in the table with the field name matching the string to the left of the 'full stop', and replace the 'full stop' with an underscore and assign the object variable to that name. Or you may wish to use the selectAs() method to decide how you want columns from different objects to be returned, when using joinAdd().

## Example (databasename).links.ini File

	;                       for table person
	[person]
	;                       link value of eyecolor to table colors, match name column
	eyecolor = colors:name
	;                       link value of owner to table grp, match id column
	owner = grp:id
	;                       link value of picture to table attachments, match id column
	picture = attachments:id
	
	;                       for a sales example with multiple links of a single column
	[sales]
	;                       for autoloading the car object into $sales->_car_id
	car_id = car:id
	;                       for autoloading the part number object into $sales->_car_id_partnum
	car_id.partnum = part_numbers:car_id

## Fetching Linked Entities

	$person = new DataObjects_Person();
	$person->eyeColour = 'red';
	$person->find();
	while ($person->fetch()) {
	
	    // use the links.ini file to automatically load
	    // the car object into $person->_car
	    $person->getLinks();
	
	    echo "{$person->name} has red eyes and owns a {$person->_car->type}\n";
	}
## Joining Entities for a SELECT

	// and finally the most complex, using SQL joins and select as.
	// the example would look like this.
	
	$person = new DataObjects_Person();
	$person->eyeColour = 'red';
	
	// first, use selectAs as to make the select clauses match the column names.
	$person->selectAs();
	
	// now lets create the related element
	$car = new DataObjects_Car();
	
	// and for fun.. lets look for red eys and red cars..
	$car->colour = 'red';
	
	// add them together.
	$person->joinAdd($car);
	
	// since both tables have the column id in them, we need to reformat the query so
	// that the car columns have a different name.
	$person->selectAs($car,'car_%s');
	
	$person->find();
	while ($person->fetch()) {
	    echo "{$person->name} has red eyes and owns a {$person->car_type}\n";
	}

## Managing Database Changes
It can happen that you update your database, but the Dataobjects entities don't get synced therefore simple things, like logging in, become impossible.  In such a case you need to login as admin, go to the maintenance screen, and run 'rebuilt entities'.

Since a regular login is impossible you can get around this as follows:
 1. temporarily disable authorisation on your site, this means anyone will be able to get to authorised pages just knowing the correct URL

	$conf['debug']['authorisationEnabled'] = '0';

 2. add SGL\_GUEST to the list since you will be a guest when you reach this screen

	$conf['site']['rolesHaveAdminGui'] = 'SGL\_ADMIN,SGL\_ROLE\_MODERATOR,SGL\_GUEST';
	 
 3. type in the url of the maintenance screen

	http://example.com/index.php/default/maintenance/

Now you can rebuild the Dataobjects entities.  Be sure to return the above settings to their original values when done.

[1]:	http://www.phpkitchen.com/index.php?/archives/367-Identifying-data-objects-and-relationships.html
[2]:	http://pear.php.net/package/DB_DataObject/
[3]:	http://www.akbkhome.com/
[4]:	http://pear.php.net/manual/en/package.database.db-dataobject.php
---
layout: page
title: Building a TODO list module with Ajax
permalink: /Tutorials/BuildingAnAjaxEnabledModule.html
---

<!-- Name: Tutorials/BuildingAnAjaxEnabledModule -->
<!-- Version: 12 -->
<!-- Last-Modified: 2008/04/21 15:15:40 -->
<!-- Author: demian -->
<!-- Status: In Progress -->

# Building a TODO list module with Ajax
> requires \>= Seagull 0.6.2

* TOC
{:toc}

## Objectives
 * to create an interactive TODO list module that is fully self-sufficient and can be installed in one click
 * to learn to use module generator
 * to integrate AJAX functionality
 * to learn the range of services supported by modules

Browsing around the web the other day I saw this [great drag and drop example][1] by a chap called [Greg Neustaetter][2] - he took the standard [Scriptaculous][3] drag and drop example you see all over the place and improved it so draggable elements could be grouped in addition to being sorted.

The objective of this tutorial is to build on top of Greg's work and tweak the javascript/CSS so it becomes possible to save the state of the list elements.  I'm calling them TODOs because when I saw this [productivity video][4] I thought it was a great idea, why not move it to the web?

## Requirements
You need to be using Seagull 0.6.2 or greater to take advantage of the features discussed in this tutorial.  At the time of writing this is only available in the [svn bugfix branch][5], but a snapshot is available [here][6].  You can check your version by logging in as admin, and viewing the version number in the page footer.

## Starting Out
Once you have Seagull installed you need to login as admin.  In order to get started with the TODO module, please select the module generator screen which can be found under the General -\> Maintenance menu .  You may be more familiar with the Rails buzzword 'scaffolding' - the module generator does exactly that, creates the classes, configuration and SQL data you need to get started, except it's browser driven so you don't need to use a command line console.  Seagull let's you interact with all aspects of the application and its maintenance from the browser so command line work is never necessary.

## The Module Generator
![][image-1]

Fill in the module generator form with the following values:

	Module name: todo
	Manager name: todo
	Actions: [x] list
	Create Templates: [x]
	Create language files: [x]
	Create ini file: [x]

and hit the 'create module now' button.  This is just a trial run.  If you're running on a Mac or Linux machine, you should get an error that says the webserver doesn't have write perms on the modules directory.  Use your favourite tool to give 'others' read and write permissions on the directory, this is where the wizard will create your module files.  If you're on windows you shouldn't have any errors.

When you get the success message it will say something like 



	Files for the todo module successfully created ...

If you click on the provided link to the module, you should get a DB error as we haven't created the relevant tables yet.  You can navigate to the modules directory and see that Seagull created a substantial number of files for you.

Let's revert back to a clean slate and start over taking the database into account.  Go to General \> Manage modules and click the uninstall link (the green dash) next to the 'todo' item in the list.  This will just disable or de-register the module.  Next you should see the module at the end of the list, grayed out and with a red X next to it.  Clicking this X will remove the physical files from the directory, please go ahead and do this.

## The Module Generator: Round 2
Now fill in the form as before, but this time tick the CRUD box:



	Module name: todo
	Manager name: todo
	Actions: [x] list
	Create CRUD Actions: [x]
	Create Templates: [x]
	Create language files: [x]
	Create ini file: [x]

Don't submit the form yet though, first we're going to build the table it's expecting.  As managing TODOs is a fairly straightforward task, let's go with a simple schema:


	CREATE TABLE `todo` (
	  `todo_id` int(11) NOT NULL,
	  `description` text NOT NULL,
	  `order_id` int(11) NOT NULL,
	  `status` int(11) NOT NULL
	);

You can use phpMyAdmin or some similar tool to create the table.  Continue the wizard and submit the form, you should get a success page now.  You can click on the link provided and you will see a screen displaying a form with no records.

Now attempt to add a record, this should work without surprises and give you a success message.  Next edit the same record you just created.  If you get a message like 



	DB_DataObject Error: No Keys available for foo


it means you forgot (if you copied the above schema) to set the primary key in your schema.  Please do so now and remember you have to rebuild Seagull for the table update to take effect.  The Seagull rebuild feature is a tool that resets your Seagull environment to its initial state.  It does this by dropping your database, re-running all the SQL schema and data files, and rebuilding your modules and config data.  To get an idea of the tasks run to achieve this see the [browser:/modules/default/classes/MaintenanceMgr.php#L157 rebuild method].

## Running 'rebuild Seagull'
[[Image(maintenance.png,right)]]To rebuild your application go to General -\> maintenance and locate the rebuild section at the bottom of the screen.  Keep in mind that this will drop your database so it's not advisable to run this on a live machine.  Because the todo table you just created will be dropped, we need to store it where Seagull can find it for the rebuild.

So let's store the schema we just created.  Create a file called schema.my.sql in the todo module's data directory, and paste your schema definition there. 

Hit 'rebuild Seagull' and don't worry about the sample data checkbox for now.  

 * *Note:* If your MySQL user doesn't have drop database privileges you will have problems
 * *Note:* The functionality is only implemented for MySQL at the time of writing

Now navigate back to http://yourhost.local/seagull/index.php/todo/ and you should be able to edit records no problem.

## Setting up the Module's Default Data

For starters you will notice there's a file already present in the data directory called data.default.my.sql.  This contains a single record which gets inserted in the module table during install, it effectively registers the todo module so it's listed as enabled.  Hitting disable simply removes the record, and as discussed above you can delete the module's files altogether.

## Creating Navigation
Next let's create some navigation data.  We need at the very least a navigation element to request to todo screen.  Let's copy an existing example from the faq modle.  Copy seagull/modules/faq/data/navigation.php to the todo data directory.  Now let's edit it, Seagull stores navigation data in simple PHP arrays and the file looks like [this][7]:

The Seagull navigation module, which is enabled by default, allows you to create complex hierarchies of navigation elements using nested sets, this means you can easily reorder nodes at any depth level.  The default navigation data is split into 2 branches, what the user sees, and what the admin sees.  In the following example we're going to simplify the faq data and add a single navigation element in the user branch, but that only admin has access to.  That's because we're developing the module logged in as admin, but we want to see the same results and theme that members and anonymous users will see.

Edit the file so it now looks like this:


	<?php
	
	$aSections = array(
	
	    //  users
	    array (
	      'title'           => 'TODO',
	      'parent_id'       => SGL_NODE_USER,
	      'uriType'         => 'dynamic',
	      'module'          => 'todo',
	      'manager'         => 'TodoMgr.php',
	      'actionMapping'   => '',
	      'add_params'      => '',
	      'is_enabled'      => 1,
	      'perms'           => SGL_ADMIN,
	        ),
	    );
	?>

The array structure should be fairly self-explanatory, you can make the title value whatever you want, the only tricky element is SGL\_NODE\_USER which can optionally be SGL\_NODE\_ADMIN is you want to locate it in the admin branch, or SGL\_NODE\_GROUP if you want it to appear in a sub-group, see admin menus and submenus for an example.


 * *Tip:* If you're logged in as admin you get the admin view, click on the Seagull logo to get the user view

To finish off this section run rebuild again to ensure your schema, default data and navigation data are picked up correctly.  You should get a tab in the user view that says 'TODO'.


## Adding Custom Javascript
Next we need to call a javascript include in the <head> section of our page.  Let's first create the file, copy the code [here][8] into a file called todo.js and store it in your module as follows:


	|-- todo
	|   `-- www
	|       `-- js
	|           `-- todo.js


We must tell Seagull to load the file, this is done in the Manager's display() method.  So open TodoMgr.php which was created for you in the classes directory, find the display method, and call the js file as follows:


	$output->addJavascriptFile(array(
	    'todo/js/todo.js',
	    ));

The addJavascriptFile() method also accepts string arguments, but we're going to be including more js files shortly, so we'll use an array.

Now observant readers will have noticed that the module's javascript dir is above the webroot, so you're wondering how will a browser be able to request it?



	js dir:  seagull/modules/todo/www/js/todo.js
	webroot: seagull/www

When you do a rebuild, Seagull symlinks web resources (CSS and javascript files in the modules/$module/www dir) into the webroot.  Symlinks are only supported on Macs and Linux, so for windows users Seagull copies the web resources directory into the webroot.  This is a hassle because small changes can only be picked up after a rebuild.  The good workaround is described [here][9].

So fire off a rebuild to setup the symlink or directory copy.  The easiest way to test if the js file is being included correctly, also taking into account the path, permissions, etc, is to put a little


	alert('foo');

at the top of the file and refresh your browser.

Next let's copy and paste most of Peter's html code into our TODO template.  We're only going to be using the default list action, so edit todo/templates/todoList.html and replace the contents with [this][10].

So now with the custom js being correctly loaded in the <head> of the document, and the page being initialised with the Sortable.create() call just before the <body> close, we just have to ensure the Scriptaculous libs are being loading correctly.

The Scriptaculous libs are supplied in the default distribution of Seagull, so it's just a matter of including them from your manager.  Scriptaculous loads the modules it needs with a comma-separated argument list.  Using [Firebug][11] it is quite easy to get the arguments right, when lib deps are wrong you're given an informative message.  The correct loading sequence turns out to be

	'js/scriptaculous/src/scriptaculous.js?load=builder,effects,dragdrop',

So the code that loads your required js libs should now look like:


	$output->addJavascriptFile(array(
	    'js/scriptaculous/lib/prototype.js',
	    'js/scriptaculous/src/scriptaculous.js?load=builder,effects,dragdrop',
	    'todo/js/todo.js',
	    ));

## Adding Custom CSS
To get the ball rolling we're just going to use the CSS straight from Peter's page.  Create the file todo.css and paste in [this code][12] as follows:

	|-- todo
	|   `-- www
	|       `-- css
	|           `-- todo.css


To have Seagull include it in the <head> tag in your page you need to call

	$output->addCssFile('todo/css/todo.css');

Just place this code below your js includes, anywhere in the display() method in the manager is fine.

Refresh the page and ensure that your javascript and CSS is loading correctly.

## Cleanup
As mentioned above, only the list action is going to be used in this tutorial, this is because all the operations are going to be done with Ajax. 

So go into TodoMgr.php and remove all the other action methods that were automatically created.

 * *Tip*: an action method is any method prepended with _cmd_

## Adding the todo\_group Table
Since our TODOs are being stored in groups, we're going to need a table to represent the group relation.  Here's a suggested table definition:


	CREATE TABLE `todo_group` (
	  `todo_group_id` int(11) NOT NULL,
	  `name` varchar(128) NOT NULL,
	  `order_id` int(11) NOT NULL,
	  PRIMARY KEY  (`todo_group_id`)
	);


Add this to your schema file (schema.my.sql)

## Setting up Sample Data
[[Image(files.png,right)]]Often it helps do have some sample data in the Db while developing your module or any application.  Using phpMyAdmin or equivalent, add a record or two of sample TODOs, and some sample groups, and save the inserts in a file called data.sample.my.sql.  Save this in - you guessed it - the data directory.

Rebuild to have the sample data loaded, remember to tick the 'with sample data' option.

## Query for Groups
As you're probably already familiar with [DB\_DataObject][13], I won't go into too much detail for this next section.

 * in the \_cmd\_list method of TodoMgr.php, delete everything after $output-\>pageTitle
 * to get a basic groups resultset, we'll copy the code from FaqMgr's list method:  


		$faqList = DB_DataObject::factory($this->conf['table']['faq']);
		$faqList->orderBy('item_order');
		$result = $faqList->find();
		$aFaqs  = array();
		if ($result > 0) {
		while ($faqList->fetch()) {
		 $faqList->question = $faqList->question;
		 $faqList->answer   = nl2br($faqList->answer);
		 $aFaqs[] = clone($faqList);
		}
		}
		$output-\>results = $aFaqs;

and change it as follows:

	$groupList = DB_DataObject::factory($this->conf['table']['todo_group']);
	$groupList->orderBy('order_id');
	$result = $groupList->find();
	$aGroups  = array();
	if ($result > 0) {
	    while ($groupList->fetch()) {
	        $aGroups[] = clone($groupList);
	    }
	}
	$output->results = $aGroups;

When your refresh your browser you should get an error

	Undefined index: todo_group

This means we need to add the new table name to our tableAliases.ini file as so

	todo_group = todo_group

 * *Tip*: tableAliases.ini is in the data folder

Now rebuild Seagull again.  If you want to change the table name in the future, you can do so, and just change your config without touching the SQL and your code will still work.

Now the value "results" will be available to the template, so let's iterate through the list of groups.  In the template at the top add the following:

	{foreach:results,k,oGroup}
	{oGroup:r}
	{end:}


 * *Tip*: The Flexy :r modifier has the same effect as running a variable through print\_r();

Refresh your browser, and remember that your default action has been set to list for you in the validate() method, and you should get a set of results similar to


	DataObjects_Todo_group Object
	(
	    [__table] => todo_group
	    [todo_group_id] => 1
	    [name] => Today
	    [order_id] => 1
	    [_DB_DataObject_version] => 1.8.4
	    [N] => 3
	    [_database_dsn] => 
	    [_database_dsn_md5] => 4a290d26f308959cbefed881d8c78719
	    [_database] => seagull
	    [_DB_resultid] => 1
	    [_resultFields] => 
	    [_link_loaded] => 
	    [_join] => 
	    [_lastError] => 
	)
	
	DataObjects_Todo_group Object
	(
	    [__table] => todo_group
	    [todo_group_id] => 2
	    [name] => my first sample group
	    [order_id] => 2
	    [_DB_DataObject_version] => 1.8.4
	    [N] => 3
	    [_database_dsn] => 
	    [_database_dsn_md5] => 4a290d26f308959cbefed881d8c78719
	    [_database] => seagull
	    [_DB_resultid] => 1
	    [_resultFields] => 
	    [_link_loaded] => 
	    [_join] => 
	    [_lastError] => 
	)
	
	DataObjects_Todo_group Object
	(
	    [__table] => todo_group
	    [todo_group_id] => 3
	    [name] => my second sample group
	    [order_id] => 3
	    [_DB_DataObject_version] => 1.8.4
	    [N] => 3
	    [_database_dsn] => 
	    [_database_dsn_md5] => 4a290d26f308959cbefed881d8c78719
	    [_database] => seagull
	    [_DB_resultid] => 1
	    [_resultFields] => 
	    [_link_loaded] => 
	    [_join] => 
	    [_lastError] => 
	)

What we need to do now is output the results in divs, so we can work on getting the todo items for each group.  Replace the template code with:


	{foreach:results,k,oGroup}
	    <div id="group{k}" class="section">
	        <h3 class="handle">{oGroup.name}</h3>
	    </div>
	{end:}

 * *Tip*: In Flexy anything between {curlyBraces} is evaluated as a variable.

Refresh your browser, and the results should be 3 boxes with the group names from the sample data.  Flexy supports adding custom HTML element attributes, so you can rewrite the above as follows which is cleaner:


	<div flexy:foreach="results,k,oGroup" id="group{k}" class="section">
	    <h3 class="handle">{oGroup.name}</h3>
	</div>

This is a great trick for keeping your templates clean so they can be opened in Dreamweaver or equivalent by designers.

## Getting the TODOs from the Groups
What we want to be able to do is get a list of all groups, then for each group, iterate through its todos.  We're going to build a helper method that will get all todos by group id.  Rather than build the method into the Todo\_group entity generated by DB\_DataObject (see seagull/var/cache/entities/\*), we'll keep all SQL logic local to the manager and later we can factor it out to a Data Access object.  So let's create the method getTodosByGroupId($groupId)


	function getTodosByGroupId($groupId)
	{
	    $query = 
	        "SELECT todo_id, description
	         FROM {$this->conf['table']['todo']}
	         WHERE group_id = {$groupId}
	         ORDER BY order_id";
	     $aRes = $this->dbh->getAll($query);
	     return $aRes;
	}


Notice how $this-\>dbh, a reference to the database object, and $this-\>conf, a reference to the global config array, are _just available_.  This is true of any class that inherits from SGL\_Manager - these resources are used in almost every action.

So the new resultset now contains dataobjects that look something like this:

	DataObjects_Todo_group Object
	(
	    [__table] => todo_group
	    [todo_group_id] => 1
	    [name] => Today
	    [order_id] => 1
	    [_DB_DataObject_version] => 1.8.4
	    [N] => 3
	    [_database_dsn] => 
	    [_database_dsn_md5] => 4a290d26f308959cbefed881d8c78719
	    [_database] => seagull
	    [_DB_resultid] => 1
	    [_resultFields] => 
	    [_link_loaded] => 
	    [_join] => 
	    [_lastError] => 
	    [aTodos] => Array
	        (
	            [0] => stdClass Object
	                (
	                    [todo_id] => 1
	                    [description] => my first sample todo
	                )
	
	            [1] => stdClass Object
	                (
	                    [todo_id] => 2
	                    [description] => my second sample todo
	                )
	
	        )
	
	)

which will be easy to integrate into our templates.

## Tweaking the Javascript using Scriptaculous
I modified Peter's code a little because I wanted each group box to have it's own 'Add Todo' form.  This means users will be able to add todo items directly to the group they want them to appear in, eg, "call plumber" in the "domestic" group.  The new group boxes and add todo widgets were built using Scriptaculous' Builder object.  If you haven't used it yet it's a fairly straightforward DOM builder with a few gotchas.


## Saving the TODOs with Ajax
This is the easy part - you can use the same syntax in Seagull you'd use in standalone Prototype scripts.  It was decided the Scriptaculous/Prototype API was elegant enough in it's own right, no good reason to wrap it, force devs to learn another syntax, and incur additional overhead.

A recent change in Seagull means request headers are now analysed as the SGL\_Request object parses $\_REQUEST and if an Ajax call is detected, a much lighter filter chain of tasks is executed.  No URL aliases are searched for (for alias think keyword: route) as standard Seagull URLs are fine for Ajax calls.  See the code [browser:branches/0.6-bugfix/lib/SGL/FrontController.php#L142 here].

So we just use a typical Prototype Ajax.request() method for the `onclick` event as follows:

	<input  type="button" 
	        onClick="
	            createNewTodo($('new_todo_{oGroup.todo_group_id}').value, $('group{oGroup.todo_group_id}').id);
	            new Ajax.Request(
	                '{makeUrl(#addTodo#,##,#todo#)}', 
	                {asynchronous:true, 
	                 method:'post',
	                 parameters:'todo=' + $('new_todo_{oGroup.todo_group_id}').value + '&groupId='+ {oGroup.todo_group_id},
	                 onSuccess:handlerFunc}); 
	            return false;" 
	        value="Add Todo" /> 

The first method builds the todo in the DOM, then the Ajax request saves it in the database.  The only thing unusual to notice in the second URL parameter is the makeUrl Flexy method doesn't need the second argument.

	{makeUrl(#addGroup#,##,#todo#)}

 * *Reminder:* makeUrl(#action#,#manager#,#module#)

The reason the second argument isn't needed is that for Ajax calls, a single Data Access style object is used for the whole module, so just knowing the action name and module name is enough.  I say Data Access style, because it's similar to existing Seagull Data Access objects, but also allows HTML to be returned, in short it provides any data that could be requested by an Ajax call.

## Creating the Ajax Provider
As long as your AjaxProvider is prepended with the name of your module, it will be loaded automatically for Ajax calls.  So for the todo module we need to create a file called TodoAjaxProvider.php and save it in the classes directory.

Copy the following code into your new TodoAjaxProvider.php file:

	<?php
	require_once 'HTML/Template/Flexy.php';
	require_once SGL_CORE_DIR . '/Delegator.php';
	require_once SGL_CORE_DIR . '/AjaxProvider.php';
	
	class TodoAjaxProvider extends SGL_AjaxProvider
	{
	    function TodoAjaxProvider()
	    {
	        SGL::logMessage(null, PEAR_LOG_DEBUG);
	
	        parent::SGL_AjaxProvider();
	        $this->responseFormat = SGL_RESPONSEFORMAT_JSON;
	    }
	}

We won't be outputing Flexy template content for this tutorial, but usage example will follow later.  DB\_DataObject is initialised in the constructor, but will be factored out shortly.  So now it's just a question of grabbing the posted variables, and performing database operations.

## Creating the addTodo() Ajax Method
![][image-2]
As you can see in the above Prototype code, the parameters 'todo' and 'groupId' are sent via post to the AjaxProvider.  So the first thing we need to do is retrieve the params.  We can test what's being retrieved since the javascript callback function `handlerFunc` outputs the returned Ajax value in a hidden div with an id of 'response'.  This makes for easy debugging, so modify your CSS file by adding the following definition:

	#response {
	    line-height: 10px;
	    background-color: green;
	    padding: 10px;
	    width: 400px;
	    margin: auto;
	    color: #fff;
	    font-weight: bold;
	}

When you make the Ajax request, which results from the 'Add Todo' button being clicked, output from the response will be sent and displayed in the response div.  Any problems with typos, variable values or includes can be resolved easily.

So the TodoAjaxProvider::addTodo() method is going to be very simple: we'll retrieve the post params, and save them as a DataObject.  The code looks like this:

	    function addTodo()
	    {
	        $todoTxt = SGL_Request::singleton()->get('todo');
	        $groupId = SGL_Request::singleton()->get('groupId');
	
	        $nextId = $this->dbh->nextId($this->conf['table']['todo']);
	        $todo = DB_DataObject::factory($this->conf['table']['todo']);
	        $todo->todo_id = $nextId;
	        $todo->description = $todoTxt;
	        $todo->status = 1;
	        $todo->order_id = $nextId;
	        $todo->group_id = $groupId;
	        $success = $todo->insert();
	
	        //return SGL_Registry::singleton()->getRequest()->debug();
	        if ($success) {
	            $ret = 'Todo added successfully';
	        } else {
	            $ret = 'There was a problem adding the todo';
	        }
	        return $ret;
	    }

 * *Tip*: Notice the commented out debug call, this is handy for checking your request values.

## Conclusion
In this tutorial we covered the following aspects of the Seagull framework:
 * creating a module using Seagull's module generator
 * learning about the rebuilding the application environment
 * adding custom javascript and CSS files to a manager
 * creating and saving navigation, default and sample data
 * using `DB_DataObject` to query the database
 * Using `Scriptaculous` and `Prototype` to integrate Ajax and web 2.0 effects

I hope you enjoyed this tutorial and found it useful, and look forward to your feedback.

## Download Code
A few rough corners were smoothed out for the final product which you can download [todo.zip here][14].  There are many areas for improvements including saving the order of the todos and groups, and the state of each item, ie finished or not.  You can also view the results [online][15].



{: align="right"}
{: align="right"}

[1]:	http://www.gregphoto.net/sortable/advanced/
[2]:	http://www.gregphoto.net/index.php/about/
[3]:	http://script.aculo.us/
[4]:	http://www.youtube.com/watch?v=jcIkygt3G48&eurl=
[5]:	/Installation/FromSVN.html
[6]:	/files/0.6.2-snapshot.r2888.tar.gz
[7]:	/Tutorials/BuildingAnAjaxEnabledModule/Navigation.html
[8]:	/Tutorials/BuildingAnAjaxEnabledModule/Javascript.html
[9]:	/Howto/WorkingWithModules.html#LinkinginWebAssets
[10]:	/Tutorials/BuildingAnAjaxEnabledModule/Template.html
[11]:	http://getfirebug.com/
[12]:	/Tutorials/BuildingAnAjaxEnabledModule/Css.html
[13]:	http://pear.php.net/package/DB_DataObject/
[14]:	/images/Tutorials/BuildingAnAjaxEnabledModule/todo.zip
[15]:	http://demo.seagullproject.org/index.php/todo/

[image-1]:	/images/module_generator.png
[image-2]:	/images/Tutorials/BuildingAnAjaxEnabledModule/debug.png
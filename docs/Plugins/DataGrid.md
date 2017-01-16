<!-- Name: Plugins/DataGrid -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/12/31 01:48:12 -->
<!-- Author: demian -->
# Universal Data Grid Plugin
[[TOC]]

See: source:branches/datagrid/

In our company we have developed a DataGrid control - I think it can be
interesting for you. It is quite powerful and easy to use. We move that code to SVN branch.

Here is some documentation about it and demo.

DataGrid class gives possibility for fast programming of lists with
powerful features. Thanks to this class
all DataGrids in application may have the same look and behaviour and it
is easy to add new features to the class.
DataGrid is integrated with Seagull MVC standard.

## Demo
http://www.varico.poznan.pl/beta/DataGrid/seagull/www/index.php/tools/DataGrid/

## Features
  * sorting on every column
  * filtering on every column
  * fast filtering and sorting (using modified SQL query to sort and filter, so it is fast and reliable)
  * paging, navigation on pages, possibility to choose different number of rows on one page
  * export to !Excel/Word
  * select/deselect all rows and actions on multiple rows
  * actions for every row
  * automatical selection of added/modified row after adding/modifying
  * colours and font attributes for each row/column
  * flexible template to change look & style
  * many DataGrids on one page
  * sum for columns: sum of page, sum of whole list
  * average for columns: average of page, average of whole list
  * data source types: sql query and adding rows manual by programmer from PHP
  * column types:
   * id: id for DataGrid, used to select rows and to make specificactions on a row. It can be not visible, rows are chosen using JavaScript?
   * text: text (aligned to left)
   * html: HTML (aligned to left)
   * user: programmer may set own setting for column (alignment, colour etc)
   * colour: column will be coloured with colour from data
   * integer: integer (aligned to right)
   * real: real number (aligned to right)
   * date: date field (aligned to middle)
   * hour: hour field (aligned to middle)
   * image: image from file (aligned to middle)
   * thumbnail: thumbnail from file (aligned to middle)
   * enclosure: enclosed file with icon (aligned to middle)
   * mail: e-mail with possibility to send e-mail (aligned to left)
   * action: field where may be defined actions for row of data (i.e. button with edit possibility, delete etc)
  * it may be used not only for active lists but also for reports (only to change template of DataGrid)

## Programming
  * MVC standard
  * 3 main objects:
    * !DataGrid
     main object: actions dispatching, columns and datasource managing
    * !Column
     object for every column in !DataGrid, with possibility to change its properties, type etc.
    * !DataSource
     object with data (rows) for !DataGrid: sorting, filtering and page navigation functions

  * template to use & change: DataGrid.html
   it is quite complicated but powerful - here you may change the look & feel of DataGrid
   Why these strange functions: {getArrayValue(valueObj,column.dbName)}:  because of limitation of FLEXY (valueObjcolumn.dbName is not possible in FLEXY)

## Examples
### Very easy DataGrid, manually filled:

    #!php
    <?php
            require_once SGL_LIB_DIR . '/SGL/DataGrid.php';
            require_once SGL_LIB_DIR . '/SGL/DataSource.php';
    
            //create new dataGrid
            $dataGrid = & new SGL_DataGrid('one');
            //add columns to dataGrid
            $col = &$dataGrid->addColumn(array(
    					    'type' => 'text',
    					    'name' => 'name',
    					    'dbName' => 'name',
    					    'filterable' => true,
    					    'sortable' => true
    					  ));
            $col->setTransformInColumn(array('Adamus' => 'ad'));
            $col2 = &$dataGrid->addColumn(array(
    					    'type' => 'text',
    					    'name' => 'surname',
    					    'dbName' => 'surname',
    					    'filterable' => true,
    					    'sortable' => true
    					  ));
            $col2->setTransformInColumn(array('Mielcarus' => 'mtest'));
            $dataGrid->addColumn(array(
    				    'type' => 'text',
    				    'name' => 'city',
    				    'dbName' => 'city',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
    	$dataGrid->addColumn(array(
    				    'type' => 'integer',
    				    'name' => 'age',
    				    'dbName' => 'age',
    				    'filterable' => true,
    				    'sortable' => true,
    				    'avgTotalable' => true,
    				    'avgable' => true
    				  ));
            //create instance of data source
            $dataSource = & new SGL_DataGridDataSource();
            //add manually data to source
            $dataSource->addRow(array
                ('name' => 'Adamus', 'surname' => 'Mielcarus', 'city' => 'Olimp', 'age' => 33));
            $dataSource->addRow(array
                ('name' => 'Grzegorzus', 'surname' => 'Dabrawus', 'city' => 'Sparta', 'age' => 21));
            $dataSource->addRow(array
                ('name' => 'Robertus', 'surname' => 'Borkowus', 'city' => 'Ateny', 'age' => 22));
            $dataSource->addRow(array
                ('name' => 'Piotrus', 'surname' => 'Skrzypus', 'city' => 'Troja', 'age' => 24));
            $dataSource->addRow(array
                ('name' => 'Tomaszus', 'surname' => 'Przybyszus', 'city' => 'Saloniki', 'age' => 25));
    	
            //get all data, fill prepared data to dataGrid and display the dataGrid with data
            $dataGrid->validate($input->inputReq);
            $dataGrid->setDataSource($dataSource);
            $dataGrid->display($output);
    ?>
### SQL DataGrid

    #!php
    <?php
            require_once SGL_LIB_DIR . '/SGL/DataGrid.php';
            require_once SGL_LIB_DIR . '/SGL/SQLDataSource.php';
    
            $dataGrid = & new SGL_DataGrid('two');
            $dataGrid->addColumn(array(
    				    'type' => 'id',
    				    'name' => 'id',
    				    'dbName' => 'id',
    				  ));
            $dataGrid->addColumn(array(
    				    'type' => 'text',
    				    'name' => 'name',
    				    'dbName' => 'name',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $dataGrid->addColumn(array(
    				    'type' => 'text',
    				    'name' => 'description',
    				    'dbName' => 'description',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $dataGrid->addColumn(array(
    				    'type' => 'real',
    				    'name' => 'salary',
    				    'dbName' => 'salary',
    				    'filterable' => true,
    				    'sortable' => true,
    				    'sumable' => true,
    				    'sumTotalable' => true
    				  ));
            $dataGrid->addColumn(array(
    				    'type' => 'enclosure',
    				    'name' => 'file',
    				    'dbName' => 'fileattached',
    				  ));
            $dataGrid->addColumn(array(
    				    'type' => 'image',
    				    'name' => 'picture',
    				    'dbName' => 'picture',
    				  ));
            $dataSource = & new SGL_DataGridSQLDataSource("
                SELECT id, name, description, salary, fileattached, picture
                FROM people WHERE #_FILTER#
            ", 'id');
            $dataGrid->validate($input->inputReq);
            $dataGrid->setDataSource($dataSource);
            $dataGrid->display($output);
    ?>
### Two DataGrids on one page

    #!php
    <?php
            require_once SGL_LIB_DIR . '/SGL/DataGrid.php';
            require_once SGL_LIB_DIR . '/SGL/SQLDataSource.php';
    
            $dataGrid = & new SGL_DataGrid('three');
            $dataGrid->dataGridHeader = 'Sample title';
            $dataGrid->addColumn(array(
    				    'type' => 'user',
    				    'name' => 'name',
    				    'dbName' => 'name',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $dataGrid->addColumn(array(
    				    'type' => 'colour',
    				    'name' => 'colour column',
    				    'dbName' => 'colour',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $dataGrid->addColumn(array(
    				    'type' => 'email',
    				    'name' => 'email',
    				    'dbName' => 'email',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $dataGrid->addColumn(array(
    				    'type' => 'link',
    				    'name' => 'www',
    				    'dbName' => 'www',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $dataSource = & new SGL_DataGridSQLDataSource("
                SELECT name, birthdate, colour, email, www
                FROM people WHERE #_FILTER#
                ORDER BY name
            ", 'id');
            $dataGrid->validate($input->inputReq);
            $dataGrid->setDataSource($dataSource);
            $dataGrid->display($output);
    
            $dataGrid2 = & new SGL_DataGrid('four');
            $dataGrid2->emptyTitle     = 'DataGrid with no data - only special title for this situation is shown';
            $dataGrid2->addColumn(array(
    				    'type' => 'id',
    				    'name' => 'id',
    				    'dbName' => 'id'
    				  ));
            $col = &$dataGrid2->addColumn(array(
    				    'type' => 'text',
    				    'name' => 'name',
    				    'dbName' => 'name',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $col->setFilterSelect(array('Jan', 'Andrew', 'Stephen'));
            $dataGrid2->addColumn(array(
    				    'type' => 'text',
    				    'name' => 'surname',
    				    'dbName' => 'surname',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $dataGrid2->addColumn(array(
    				    'type' => 'date',
    				    'name' => 'birth date',
    				    'dbName' => 'birthdate',
    				    'filterable' => true,
    				    'sortable' => true
    				  ));
            $dataGrid2->addColumn(array(
    				    'type' => 'real',
    				    'name' => 'salary',
    				    'dbName' => 'salary',
    				    'filterable' => true,
    				    'sortable' => true,
    				    'sumable' => true,
    				    'sumTotalable' => true
    				  ));
            $dataSource2 = & new SGL_DataGridSQLDataSource("
                SELECT id, name, surname, birthdate, salary
                FROM people WHERE #_FILTER# AND 1=0
                ORDER BY id
            ", 'id');
            $dataGrid2->validate($input->inputReq);
            $dataGrid2->setDataSource($dataSource2);
            $dataGrid2->display($output);
    ?>

## Feedback
Hiya Krzysztof

Great work on the datagrid, looks a lot cleaner than before, i think it's really coming together.  I have a few comments:
### API
*File Structure*
  * pls put the DataGrid and associated classes in lib/SGL, do it PEAR style so the namespace makes sense:
/lib/SGL/DataGrid.php
/lib/SGL/DataGrid/Column.php
/lib/SGL/DataGrid/DataSource.php
... etc

  * why not use a similar API for adding columns as for datasources, i much prefer code that you can read right away rather than having to reference API docs to know what all the args are:
*ok*

    #!php
    <?php
    $dataGrid->addColumn('text', 'name', 'name', true, true);
    ?>
*better*

    #!php
    <?php
    $dataGrid->addColumn(
        array('type' => 'text', 
                 'label' => 'name', 
                 'fieldName' => 'name');
                 // etc
    ?>
  * i would rename DataGrid::dataGridValidate() to DataGrid::validate() to avoid name duplication
  * the *addAction()* stuff needs to be updated to 0.5.x way of doing things, we haven't used those ?foo=bar querystrings for almost a year now.  Can I suggest:

    #!php
    <?php
    $dataGrid->addAction(
        array('label' => 'Delete', 
                 'module' => 'foo', 
                 'manager' => 'bar'),
                 'action' => 'baz'),
                 'id' => {id}),
                 'name' => {name});
                 // or something similar
    ?>

### Code
  * you are globalling a lot of values into methods, this is not done anywhere is seagull and is bad programming practice, pls pass by arg, see SGL_DataGridDataSource::setSort()

### General
  * Can I suggest you look at the Unit testing framework built into Seagull, it's quite essential to have the code unit tested when you're building functionality of this level of complexity, give it a try, will make your life a *lot* easier.
  * pls move your demo code to Seagull 0.5.x when above changes are implemented
[/DemianTurner DemianTurner] /27.10.2005 10:32/


## Changes and improvements
Hi Demian,

Thanks for your advices. 

We've made some code and API changes, like:
 * API for addAction,
 * API for addColumn, both using array('label'=>'zzz', 'icon'=>'a.gif') convention,
 * we've move the DataGrid and associated classes in lib/SGL, and made PEAR style namespace,
 * we removed all global vars,
 * we changed DataGrid::dataGridValidate() to DataGrid::validate(),
 * we moved code to Seagull 0.6.0


We also add some new functionality, like:
 * average in column summary,
 * saving filters and sorting in session, so if you back to any dataGrid you have your filters filled, and sorted like previous,
 * javascript calendar to select date in filters in data or hour type column,
 * javascript calculator to operate with values in filters in integer or real column type,
 * dataGrid title,
 * special dataGrid title, which is shown when dataGrid is empty,
 * function setInitialOrder(..) to set initial (after display) column sorting


[/KrzysztofKempinski KrzysztofKempinski] /13.04.2006/

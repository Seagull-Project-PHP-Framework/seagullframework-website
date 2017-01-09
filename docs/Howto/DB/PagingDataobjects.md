<!-- Name: Howto/DB/PagingDataobjects -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/09/08 18:32:53 -->
<!-- Author: demian -->
# Paginating dataobjects
[[TOC]]
## Overview
Somo auto-explained code to paginate a resultset using dataobjects and pear pager instead of SQL-SGL_DB method ...

In manager:
        

    $output->pageTitle = $this->pageTitle . ' :: Browse';
    
    $categoryList = DB_DataObject::factory($this->catTableName);
    
    $result = $categoryList->find();
    
    // Paginate code if needed
    $limit = $_SESSION['aPrefs']['resPerPage'];
    
    if ($limit < $categoryList->count()) {
        require_once 'Pager/Pager.php';
        $pagerOptions = array(
            'mode'     => 'Sliding',
            'delta'    => 3,
            'perPage'  => $limit,
            'httpMethod' => 'POST',
            'importQuery' => false,
            'path'=>SGL_Output::makeUrl('list','categories','companies'),
            'totalItems'=> $categoryList->count()
        );
        
        $pager = & Pager::factory($pagerOptions);
        $output->pager = $pager;
        
        $categoryList->limit(($pager->getCurrentPageID()-1)*$limit,$limit);
        $result = $categoryList->find();
    }
    
    $aCategories = array();
    if ($result > 0) {
        while ($categoryList->fetch()) {
            $categoryList->cat_name = $categoryList->cat_name;
            $categoryList->cat_desc = nl2br($categoryList->cat_desc);
            $aCategories[] = clone($categoryList);
        }
    }
    $output->results = $aCategories;

In template:


    {if:pager}
       <flexy:include src="pearPager.html" />
    {end:}

And the "pearPager.html" code:


    <div class="pager">
    <table>
        <tr>
            <td>
                {pager.links:h}
            </td>
        </tr>
    </table>
    </div>

## Alternatives
Be sure to take a look at this tutorial that shows you how to use PEAR's Structures_DataGrid with DB_DataObject to create paginated dbdo resultsets:

 * see http://www.samalyse.com/code/pear/dgdo/

Example code:



    <?php
    class DataObject_Fruits extends DB_DataObject 
    {
        var $__table = "fruits";
        var $id;
        var $name;
        var $stock;
        var $price;
    }
    
    $dataobject = new DataObject_Fruits();
    $datagrid =& new Structures_DataGrid(10);
    $datagrid->bind($dataobject); // the Magic
    $datagrid->render();
    ?>



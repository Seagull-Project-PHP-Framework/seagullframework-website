<!-- Name: Howto/DB/PagingRecordsets -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/09/12 18:23:53 -->
<!-- Author: demian -->
<!-- Status: Original -->

# Paging Recordsets

Here is an example of paging recordsets:

In your manager:  


	    $query = "SELECT preference_id, name, default_value FROM {$conf['table']['preference']}";
	    $limit = $_SESSION['aPrefs']['resPerPage'];
	    $pagerOptions = array(
	        'mode'      => 'Sliding',
	        'delta'     => 3,
	        'perPage'   => $limit,
	        'totalItems'=> $input->totalItems,
	    );
	
	    $aPagedData = SGL_DB::getPagedData($this->dbh, $query, $pagerOptions);
	    $output->aPagedData = $aPagedData;
	    if (is_array($aPagedData['data']) && count($aPagedData['data'])) {
	        $output->pager = ($aPagedData['totalItems'] <= $limit) ? false : true;
	    }

Then in the template simply call the pager subtemplate which only gets included if you have more than a pageful of records (ie, 11+ records where you specify 10/page):



	{if:pager}
	    <flexy:include src="pager.html" />
	{end:}


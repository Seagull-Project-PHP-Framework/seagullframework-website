<!-- Name: Howto/Templates/BuildingForms -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/11/30 15:48:55 -->
<!-- Author: demian -->

# Form Generation
* TOC
{:toc}

## Important Note
When you create a form in your template and you're using the default Flexy template engine, if you want any dynamic values, ie using any of the conventions listed below, the form element must contain the `flexy:ignore` attribute, ie


	<form name="myName" id="myId" flexy:ignore>

## HTML select boxes

	$aOptions = array(1 => 'news', 2 => 'travel', 3 => 'commentary');
	
	<select name="category">
	        {generateSelect(aOptions):h}
	</select>


produces ...

	<option value="1">news</option>
	<option value="2">travel</option>
	<option value="3">commentary</option>

## Select box, preserving last selected item

	<select name="category">
	        {generateSelect(aOptions,category):h}
	</select>

_note: variable 'category' is from $input-\>category_

produces ...

	<select name="category">
	<option value="1">news</option>
	<option value="2">travel</option>
	<option value="3" selected="selected">commentary</option>
	</select>

## Multiple Select Boxes

	<select name="category[]" multiple="multiple" size="5">
	     {generateSelect(aOptions,category,#true#):h}
	</select>

first and second parameter need to be arrays!


## Automatic Date / Time selector

	list($day, $month, $year, $hour, $minute, $second) = 
	            explode('/', date('d/m/Y/H/i/s'));
	        //  initialise input array with current date/time
	        $aDate = array( 'day' => $day,
	                        'month' => $month,
	                        'year' => $year,
	                        'hour' => $hour,
	                        'minute' => $minute,
	                        'second' => $second);
	        $output->dateSelectorStart = 
	            SGL_Output::showDateSelector($aDate, 'frmStartDate');

## Automatic Date selector
nearly the same like above, but:

	list($day, $month, $year) = 
	            explode('/', date('d/m/Y'));
	        //  initialise input array with current date/time
	        $aDate = array( 'day' => $day,
	                        'month' => $month,
	                        'year' => $year);
	        $output->dateSelectorStart = 
	            SGL_Output::showDateSelector($aDate, 'frmStartDate' ,false);

in detail: $output-\>dateSelectorStart = 
	        SGL_Output::showDateSelector($aDate, 'frmStartDate','''false''');

## WYSIWYG editor
Adding a Wysiwyg editor is a two-liner:

in your manager class:

	$output->wysiwyg = true;

in your template:

	<textarea cols="40" rows="7" name="event[text]" id="frmBodyName" class="wysiwyg">
	    {event.text}
	</textarea>

note: the class of the `textarea` must be `wysiwyg`. It's automatically replaced by the editor.  See also: [Howto/Templates/IntegrateWYSIWYG][1]

[1]:	/Howto/Templates/IntegrateWYSIWYG.html
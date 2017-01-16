---
layout: page
title: Wackowiki to Trac Wiki conversion
permalink: /Internal/Wiki2Trac.html
---

<!-- Name: Internal/Wiki2Trac -->
<!-- Version: 4 -->
<!-- Last-Modified: 2006/03/11 14:34:52 -->
<!-- Author: demian -->
<!-- Status: Updated -->

# Wackowiki to Trac Wiki conversion
* TOC
{:toc}

[http://wackowiki.com/WackoDocDeutsch/Formatierung][1]
[http://trac.seagullproject.org/wiki/WikiFormatting][2]

## Currently working on / Todo
  * Move all Pages inside Clusters:
	* Tutorials
	* Howtos
	* User
  * Port pages to trac (can be done automatically)
  * Go over every page in trac for formatting issues etc...
  * Make SubWiki macro (or something similar) work in trac !/DemianTurner 


## Formatting

|---
|**Format**|**Wacko**|**Trac**|
|bold|\`|'''|
|italic|_|_|
|underline|__|__|
|strike trough|\~\~|""\~\~""|
|markup|\`|nothing similar|
|note|''|nothing similar|
|heading|2-7 =, on the right side only == needed, no space needed in between|1-5 =, equal number on both sides, space between = and heading text|
|tables|tables with and without cell borders. line begins with ""|"", in between cells: "|" |only table with border, no special markup for beginning and ending of a table. line begins with ""|"" and this token is in between all cells.|



### Misc

|---
|hiding wikinames|""WikiName""|WikiName|
|Images|Urls ending with image extension are converted to image tags. works also with uploaded files  |Urls ending with image extension are converted to image tags|
|Links| ""[WikiLink Link Text] or [WikiLink Text] or WikiLink""|[Edgewall Software][3] [Title Index][4] also direct link to ticket or svn possible|
|Interwiki Links|e.g. wacko:WackoDocDeutsch| nothing found for this yet|


### Makros

|---
|Table of Contents|""[[TOC]]""|""[TOC]""|
|Subwiki / Tree|""{{tree  root="/Internal" style="ul" nomark="1"}}""|nothing similar yet. [SubWiki] should do this job...|



### Formatter
For preformatting text (e.g. code etc)
see also: [http://trac.seagullproject.org/wiki/WikiProcessors][5]

seems like formatters for IRC or email are missing in trac

## Converting 
I think we need to follow these steps:
  * write a script to export each wacko wiki page from DB to !PageName.txt
  * run another script converting each !WackoWiki.txt -\> !TracWiki.txt
	* todo: images, '', 
  * run trac import commandline tool on dir

## export from wacko 
you can easily export a single wiki page or a whole cluster (unfortunately not the whole wiki) 

[http://seagull.phpkitchen.com/docs/wakka.php?wakka=Internal/export.xml][6]

So first we have to move all pages that are not inside clusters to several clusters...

## convert
write a convert script that grabs the xml file and outputs the trac-file as plain text containing wiki formatting

mark each file with "FIXME: Import to trac", cause i believe we have to control every single file (for content and at least for formatting)

start with clusteer /Internal/ first for testing... make a /Internal/Sandbox file with demo markup

## import to trac
wiki list                                          List wiki pages
wiki export \<page\> [file]()                          Export wiki page to file or stdout
wiki import \<page\> [file]()                          Import wiki page from file or stdin
wiki dump \<directory\>                              Export all wiki pages to files named by title
wiki load \<directory\>                              Import all wiki pages from directory
wiki upgrade                                       Upgrade default wiki pages to current version

## Drawbacks 
We have many ways of formatting in wacko, guess not all can be converted with a single script

## First Script 
This is a php-cli program. Save an export.xml file to disk and pass it to the program.

e.g. type `php wacko2trac.php -f=export.xml`

	<?php
	echo "Wacko2Trac Converter\n";
	
	require_once "XML/RSS.php";
	require_once "Console/Getopt.php";
	
	/*
	Displays the usage of this script
	Called when -h option specified or on illegal option
	*/
	function usage() {
	 $usage = <<<EOD
	Usage: ./test.php [OPTION]
	for converting an exported wacko wiki cluster to trac syntax.
	 -f=path        path to wacko rss file to parse
	 -h             Display this help
	
	file path    synonym for -f
	help         synonym for -h
	
	EOD;
	 fwrite(STDOUT,$usage);
	 exit(INVALID_OPTION);
	}
	
	// Define exit codes for errors
	define('NO_ARGS',10);
	define('INVALID_OPTION',11);
	define('NO_FILE',12);
	
	$args = Console_Getopt::readPHPArgv();
	
	// Reading the incoming arguments - same as $argv
	$args = Console_Getopt::readPHPArgv();
	
	// Make sure we got them (for non CLI binaries)
	if (PEAR::isError($args] {
	   fwrite(STDERR,$args->getMessage()."\n");
	   exit(NO_ARGS);
	}
	
	// get console options:
	$shortoptions = "f:h";
	$longoptions = array('file=','help');
	
	// Convert the arguments to options - check for the first argument
	if ( realpath($_SERVER['argv'][0]) == __FILE__ ) {
	   $options = Console_Getopt::getOpt($args,$shortoptions,$longoptions);
	} else {
	   $options = Console_Getopt::getOpt2($args,$shortoptions,$longoptions);
	}
	
	// Check the options are valid
	if (PEAR::isError($options] {
	   fwrite(STDERR,$options->getMessage()."\n");
	   usage();
	}
	
	$filename = NULL;
	
	// Loop through the user provided options
	 foreach ( $options[0] as $option ) {
	   switch ( $option[0] ) {
	 case 'h': case 'help':
	   usage();
	 break;
	 case 'f': case 'file':
	   $filename = $option[1];
	 break;
	   }
	 }
	
	// for demo: use a fixed filename for parsing
	// $filename = ($filename != NULL) ? $filename : 'wiki_export/internal.wakka.php.xml';
	
	if ($filename == NULL) usage();
	
	$rss =& new XML_RSS($filename);
	$rss->itemTags = array('TITLE', 'LINK', 'DESCRIPTION', 'PUBDATE','AUTHOR', 'GUID');
	$rss->parse();
	if (PEAR::isError($args] {
	   fwrite(STDERR,$args->getMessage()."\n");
	   exit(NO_FILE);
	}
	
	// get each item
	
	$convert = array (
	"'" => "'''", //bold
	'[' => '[',   //link
	']' => ']',
	'[' => '[',   //link
	']' => ']',
	'' => '',  //strike through
	'  * ' => '  * ', //list item
	'' => '',
	'' => ''''', //convert  to bold
	''=> '', //table
	);
	
	// TODO: redirect... but i guess this is better to do manually...
	// TODO: '' etc... to bold?
	$convert_regex = array (
	'#^''#' => "''", ''italic, but not in http://foobar etc...
	'#(?<!:)''#' => "\1''",
	'#[[TOC]]#' => '[TOC]', ''TOC
	'#~([A-Z]\w*[A-Z]\w*)#' => '!\1', '' !WikiName
	'#=(={1,5})([^=].*?)={2,6}#' => '\1 \2 \1', ''headings...
	
	);
	
	// generate arrays for str_replace;
	foreach ($convert as $key=>$value) {
	$wacko[] = $key;
	$trac[]  = $value;
	}
	
	foreach ($convert_regex as $key=>$value) {
	$wacko_regex[] = $key;
	$trac_regex[]  = $value;
	}
	
	// pseudo-filename when $title is empty...
	$cluster = $rss->getChannelInfo();
	$clusterName = $cluster['title'];
	
	// create directory
	
	if (!file_exists($clusterName] mkdir($clusterName);
	
	$filename_nr = 1;
	
	foreach ($rss->getItems() as $item) {
	
	// echo "Author: ".$item['author']."\n";
	echo "Title:  ".$item['guid']."\n";
	// echo "Text:   ".$item['description']."\n";
	echo "\n\n'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''*\n\n";
	// replace tokens
	$description = str_replace($wacko,$trac,$item['description']);
	$description = preg_replace($wacko_regex,$trac_regex,$description);
	
	// replace table cells
	preg_match_all('#\|\|(.*?)\|\', $description,$table);
	foreach ($table[1] as $row) {
	$trac_row = str_replace('|','||',$row);
	$description = str_replace($row,$trac_row,$description);
	}
	// echo "Trac:   ".$description."\n";
	// echo "\n\n'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''*\n\n";
	
	$new_text = "FIXME: Look over new file... \n\n".$description;
	
	// save to file:
	if ($item['guid']=='') {
	$item['guid'] = $clusterName.$filename_nr;
	$filename_nr++;
	}
	
	// check if it's a subcluster
	if (strpos($item['guid'],'/'] {
	$path = explode('/',$item['guid']);
	// rename parent file
	if (is_file($clusterName.'/'.$path[0]] {
	rename($clusterName.'/'.$path[0],$clusterName.'/'.$path[0].'-1');
	}
	mkdir($clusterName.'/'.$path[0]);
	
	}
	
	$handle = fopen($clusterName.'/'.$item['guid'],'a');
	if (!fwrite($handle, $new_text] {
	   print "Cannot write to $filename";
	   exit;
	   }
	
	// wait for pressing enter...
	// $foo=fgets(STDIN);
	
	}
	?>

[1]:	http://wackowiki.com/WackoDocDeutsch/Formatierung
[2]:	http://trac.seagullproject.org/wiki/WikiFormatting
[3]:	http://www.edgewall.com/
[4]:	/wiki:TitleIndex/
[5]:	http://trac.seagullproject.org/wiki/WikiProcessors
[6]:	http://seagull.phpkitchen.com/docs/wakka.php?wakka=Internal/export.xml

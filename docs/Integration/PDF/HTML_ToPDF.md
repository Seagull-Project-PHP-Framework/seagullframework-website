<!-- Name: Integration/PDF/HTML_ToPDF -->
<!-- Version: 2 -->
<!-- Last-Modified: 2008/07/15 13:52:32 -->
<!-- Author: demian -->

# Integrating HTML\_ToPDF
[[TOC]]

## Overview
HTML\_ToPDF is a PHP class that makes it easy to convert HTML documents to PDF files on the fly.

http://www.rustyparts.com/pdf.php 

Since HTML\_ToPDF converts HTML to PDF it is pretty easy to use a Flexy template to create the desired output. 

## Example


	<?php
	
	function _cmd_pdf(&$input, &$output)
	{
	    SGL::logMessage(null, PEAR_LOG_DEBUG);
	
	    parent::_cmd_view($input, $output);
	
	    if (!isset($language) || empty($language) ) {
	        $language = SGL_Translation::getLangID();
	    }
	
	    $output->masterTemplate = 'reviewViewPdf.html';
	
	    // Create a unique filename for the resulting PDF
	    $linkToPDFFull = $linkToPDF = tempnam(SGL_PATH . '/www/review', 'PDF-');
	
	    // Remove the temporary file it creates
	    unlink($linkToPDFFull);
	
	    // Give it an extension
	    $linkToPDFFull .= '.pdf';
	    $linkToPDF .= '.pdf';
	
	    // Make it web accessible
	    $linkToPDF = basename($linkToPDF);
	
	    $output->itemDetails = SGL_Item::getItemDetail(
	        $output->leadArticle['reviewItemId'], null, $language, true);
	
	    $view = new SGL_HtmlSimpleView($output);
	
	    $url = SGL_BASE_URL . '/review/' . $linkToPDF;
	
	    // Send the class our HTML and the defaultDomain for images, css, etc.
	    $pdf = new HTML_ToPDF($view->render(), '');
	
	    $pdf->setFooter('left', $this->conf['site']['name']);
	    $pdf->setFooter('right', '$D');
	
	    // Could turn on debugging to see what exactly is happening
	    // (commands being run, images being grabbed, etc.)
	    //$pdf->setDebug(true);
	
	    $result = $pdf->convert();
	    if (is_a($result, 'HTML_ToPDFException')) {
	        SGL::raiseMsg('HTML to PDF error: ' . $result->getMessage());
	    }
	
	    copy($result, $linkToPDFFull);
	    unlink($result);
	
	    if (file_exists($linkToPDFFull)) {
	        header('Location: ' . $url);
	    }
	}

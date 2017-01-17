<!-- Name: Howto/CSS/Palette -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/11/30 15:50:41 -->
<!-- Author: demian -->
<!-- Status: Original -->

# About the CSS Palette
> Please note: This is for versions 0.5 and higher, not for 0.4.x releases

* TOC
{:toc}


The aim of this page is documenting the work on a standard seagull palette.
How can we reach this goal? We have to work on two sides:
  * guarantee that the presentation layer (and so the colours) are defined only in css stylesheets
  * define and use only colours in php variables

What benefits will we have?
  * a standard palette can provide a standard and consistent layout over the whole framework/application.
  * customizing the standard stylesheet will be easy and clean 

In the end I'd like to have something like this: http://wiki.kde.org/tiki-index.php?page=Colors

Currently we have 21 unique colours (white excluded), MS and GNOME 30, KDE 42.

## www/themes/default/css/style.php

### seagull palette:

	$primary               = '!#99cc00'; '' lime green[[BR]]
	$primaryLight          = '#bbe713'; '' light green[[BR]]
	$primaryText           = '#e6ffa2'; '' pale white for text on lime green[[BR]]
	$primaryTextLight      = '#ffffff'; '' white[[BR]]
	$secondaryLight        = '#e5f1ff'; '' baby blue[[BR]]
	$secondary             = '!#9dcdfe'; '' blue[[BR]]
	$secondaryMedium       = '!#3399ff'; '' medium blue[[BR]]
	$secondaryDark         = '!#184a84'; '' dark blue[[BR]]
	$tertiary              = '#d9d9d9'; '' normal gray[[BR]]
	$tertiaryLight         = '#efefef'; '' light gray[[BR]]
	$tertiaryMedium        = '#bcbcbc'; '' medium gray[[BR]]
	$tertiaryDark          = '!#999999'; '' dark gray[[BR]]
	$tertiaryDarker        = '!#666666'; '' darker gray[[BR]]
	
	$blocksBorderBody      = $tertiaryMedium;[[BR]]
	$blocksBorderTitle     = $tertiaryMedium;[[BR]]
	$blocksBackgroundBody  = $tertiaryLight;[[BR]]
	$blocksBackgroundTitle = $primary;[[BR]]
	$blocksColorBody       = $secondaryDark;[[BR]]
	$blocksColorTitle      = $primaryText;[[BR]]
	
	$tableRowLight         = $tertiaryLight;[[BR]]
	$tableRowDark          = $tertiary;[[BR]]
	
	$sectionHeaderBackground      = $tertiary;[[BR]]
	$sectionHeaderColor           = $secondaryDark;[[BR]]
	$colHeaderBackground          = $tertiaryLight;[[BR]]
	$colHeaderColor               = $secondaryDark;[[BR]]
	$navigatorBackground          = $tertiaryDarker;[[BR]]
	$navigatorColor               = $tertiaryMedium;[[BR]]
	
	$forApproval = '#ff0000';[[BR]]
	$approved    = '#ff9933';[[BR]]
	$published   = '#!00cc00';[[BR]]
	$archived    = '#!909090';[[BR]]
	
	$error      = '#ffcc00';[[BR]]
	$errorLight     = '#ffff99';[[BR]]
	$errorDark      = '#ff9600';[[BR]]
	$errorText      = $secondaryDark;[[BR]]
	$errorTextLight = '#ffffcc';[[BR]]
	$errorTextMedium    = '#ff0000';[[BR]]
	
	$buttonBorderColors      = '#ffffff #!333333 #!333333 #ffffff';

### all:

body
  * color: $tertiaryDarker
  * background: $primaryTextLight

### header:

# sgl-header
  * background-color: $primary

# sgl-header-left
  * color: $primaryTextLight

# sgl-header-right
  * color: $primaryTextLight

# sgl-header-right a
  * color: $primaryTextLight

# sgl-header-right #headerLogAction
  * border-color: $buttonBorderColors
  * background-color: $primaryLight

### tables:

th
  * color: $primaryTextLight
  * background-color: $tertiaryMedium

# imRead
  * background-color: $tertiaryMedium

.sgl-row-light
  * background-color: $tableRowLight

.sgl-row-dark
  * background-color: $tableRowDark

### left & right blocks:

.sgl-blocks-left-item-title,
  * background-color: $blocksBackgroundTitle
  * color: $blocksColorTitle
  * border-color: $blocksBorderTitle

.sgl-blocks-right-item-title
  * background-color: $blocksBackgroundTitle
  * color: $blocksColorTitle
  * border-color: $blocksBorderTitle

.sgl-blocks-left-item-body
  * background-color: $blocksBackgroundBody
  * color: $blocksColorBody
  * border-color: $blocksBorderBody

.sgl-blocks-right-item-body
  * background-color: $blocksBackgroundBody
  * color: $blocksColorBody
  * border-color: $blocksBorderBody

### headings:

h1.pageTitle
  * color: $secondaryDark

.pageTitle
  * color: $secondaryDark

### anchors:

a
  * color: $secondaryMedium

a:visited
  * color: $tertiaryDark

a:hover
  * color: $secondaryDark

### miscellaneous:

hr
  * border-color: $tertiary

.error
  * color: $errorTextMedium 
  * background-color: transparent

.primary
  * background-color: $primary

.secondary
  * background-color: $secondary

.title
  * color: $tertiaryDark

.detail
  * color: $tertiaryDark

### publisher:

.sectionHeader
  * color: $sectionHeaderColor
  * background-color: $sectionHeaderBackground

.colHeader
  * color: $colHeaderColor
  * background-color: $colHeaderBackground

.navigator
  * color: $navigatorColor
  * background-color: $navigatorBackground

### Article Manager:

.forApproval
  * color: $forApproval

.approved
  * color: $approved

.published
  * color: $published

.archived
  * color: $archived

### Legacy:

.fieldName
  * color: $secondaryDark
  * background-color: $tertiaryLight

.fieldNameWrap
  * color: $secondaryDark
  * background-color: $tertiaryLight

.fieldValue
  * background-color: $primaryTextLight

.newsItem
  * background-color: $errorTextLight
  * border-color: $tertiaryDark

fieldset
  * color: $secondaryDark

legend
  * color: $secondaryDark

### Links:

.linkCrumbsAlt1
  * color: $secondaryDark

.linkCrumbsAlt1:hover
  * color: $secondaryDark

### Various:

.pinstripe table
  * background-color: $tertiaryLight

.pinstripe td
  * background-color: $primaryTextLight

.pager
  * background-color: $errorTextLight
  * border-color: $errorDark

.errorHeader
  * background-color: $error
  * color: $errorTextLight

.errorContent
  * background-color: $errorLight
  * border-color: $errorDark
  * border-top-color: $error

.errorMessage
  * background-color: $errorLight
  * border-color: $errorDark

.messageHeader
  * color: $primaryText
  * background-color: $primary

.messageContent
  * color: $secondaryDark
  * background-color: $primaryTextLight
  * border-color: $primary

.bgnd
  * background-color: $secondaryLight
  * border-color: $tertiaryDark

### Tooltips:

span.tipOwner span.tipText
  * border-color: $buttonBorderColors
  * background-color: $tertiaryLight
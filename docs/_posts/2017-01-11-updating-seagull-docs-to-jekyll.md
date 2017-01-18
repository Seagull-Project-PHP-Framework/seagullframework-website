---
layout: post
title:  "Markdown Conversions Required for Jekyll"
date:   2017-01-11 15:34:53 +0800
---
# Markdown Conversions Required for Jekyll
* TOC
{:toc}

## Install github-pages Gem
You need to install the `github-pages` gem and run it instead of the `jekyll` gem.

	If you want to use GitHub Pages, remove the "gem "jekyll"" above and
	uncomment the line gem "github-pages", group: :jekyll_plugins.

After this you’ll need to run `bundle update`.

## Addressing Github Jekyll Bug
Every md file in this repo has a 4 line comment at the top, this was added at the time of the last export (years ago).  You must add a blank line after the last comment line or the markdown that follows will not be parsed correctly.

## Page Layouts
For a clean output and file structure please add the following header format at the top of each file.  In Jekyll parlance this is the `Front Matter`.  The page will still be addressable if you don’t you have a page layout (and a value for `permalink`), however both MD and html output files will be generated so it’s messy.  Please use `Front Matter` headers of the following format:

	---
	layout: page
	title: How to setup a Virtual Host in Apache
	permalink: /Installation/SettingUpApacheVirtualHosts.html
	---

## Fixing TOCs
	TOC
		{:toc}

## Indicate Page Status
The best way I can come up with to indicate a page’s status is adding relevant info to the comments section, all wiki pages have this at the top of the file.  Please add one of the following status comments as a new line underneath existing comments when you update a page:

	<!-- Status: Updated -->
	<!-- Status: In Progress -->
	<!-- Status: Original -->

## Fixing Image Links

- they used to use Image() macro
- they are technically `attachments` from Trac
- they’ve been migrates to `/images` folder
- a folder was implicitly generated for them by Trac, so the hierarchy must be respected
- i.e., this is the Trac markup `[[Image(plesk_config.jpg, nolink)]]`
- and this is the updated markup `/images/Installation/plesk_config.jpg`, using img markdown


## Fixing Links To Repo Source Code

- old: `browser:/branches/0.6-bugfix/etc/customInstallDefaults.ini.dist`
- intermediate: `gitlink:/branches/0.6-bugfix/etc/customInstallDefaults.ini.dist`
- new: `valid github link` // real link once I get source uploaded

## Tables
Start with

	|---

## Lists
Local Jekyll will parse a list fine however Jekyll running on Github will not create list items unless you have a blank line before the start of your list.

## Gotchas
- The characters `three open curly braces` or `three closed curly braces` will cause a Jekyll parse error and kill the whole site, please make sure you remove these. They are used for escaping code so preferably use two single apostrophes instead.
- you cannot use a colon (:) in the Front Matter `title` attribute

## Correctly Linking Files

### seagull_files
	http://seagullfiles.phpkitchen.com

replace with 

	/files/path/to/file.ext

### Trac wiki attachments
	[attachment:filename.ext

replace with 

	/images/Path/To/Markdown/File/filename

#### Example

	[attachment:tutorial-1.tar.gz]

was changed to

	/images/Tutorials/HelloWorld/tutorial-1.tar.gz

## References
- Kramdown syntax https://kramdown.gettalong.org/syntax.html
- https://jekyllrb.com/docs/configuration/
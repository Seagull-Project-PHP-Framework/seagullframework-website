<!-- Name: TipsAndTricks/SearchingTrac -->
<!-- Version: 5 -->
<!-- Last-Modified: 2006/04/01 18:42:54 -->
<!-- Author: werner -->

# Searching Trac directly from your browser
[[TOC]]
Most browsers give you the possiblity to extend for better functionality.

## Firefox
  * download the search plugin from here http://mycroft.mozdev.org/download.html?name=seagull&submitform=Search
  * close and reopen the browser
  * select from the search bar the seagull trac search plugin
  * type #193 to browse ticket 193, [1580] to browse changeset 1580, Community/VacantJobs for the [VacantJobs][1] wiki page
  * type anything for searching whole trac environment (changesets, tickets and wiki)

## Opera
  * close opera
  * to search by ticket number add to your OPERA\_DIR/search.ini (adapt "Search Engine" number to your list)

	[Search Engine 33]
	Name=&SeagullTicket
	URL=http://trac.seagullproject.org/ticket/%s
	Query=
	Key=t
	Is post=0
	Has endseparator=0
	Encoding=utf-8
	Search Type=0
	Verbtext=17063
	Position=-1
	Nameid=0
  * to search in tracker

	[Search Engine 34]
	Name=&SeagullTracker
	URL=http://trac.seagullproject.org/search?q=%s&wiki=on&changeset=on&ticket=on
	Query=
	Key=ts
	Is post=0
	Has endseparator=0
	Encoding=utf-8
	Search Type=0
	Verbtext=17063
	Position=-1
	Nameid=0
  * start opera
  * type in address bar *'t <ticket>*' or *'ts <query>*' respectively

[1]:	/Community/VacantJobs.html
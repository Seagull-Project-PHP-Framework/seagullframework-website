<!-- Name: RFC/LookAndFeel -->
<!-- Version: 8 -->
<!-- Last-Modified: 2005/12/20 19:18:07 -->
<!-- Author: werner -->

# Look And Feel
[[SubWiki(RFC/LookAndFeel)]]
## General

My purpose is to improve Seagull's look and feel and I'd like to have your comments about what should be done and how to implement it.

When talking about Look and Feel I mean :

 * Separate backend from frontend design
 * Modify backend graphical design so it looks more like a WebApp than a WebSite
 * Modify HTML code from HTML/table design to xHTML/CSS (it eases maintenance and reduces size of pages)


## Separate Backend from Frontend design

Currently Seagull's design is the same in Frontend and Backend. Only the navigation changes depending on the user's rights.

In my point of view, a different design for the Backend interface improves users' experience.


## Seagull's Graphical User Interface

If you agree we need a GUI (for all administration tasks' screens e.g. in Publisher/ArticleMgr but not in Publisher/ArticleViewMgr) then we could give it a more professional look.

We could also optimise the way useful information is loaded and displayed in the page.

## HTML templates cleanup

A think there is too much use of <table> for structure purposes that needs to replaced by some xHTML/CSS code.
This way html code is cleaner and easier to modify for graphical folks when you want to change your theme.
This would also brings two more assets : a standard compliant xHTML code improves search engines indexing and optimisation, and reduces size of files.

As per W3C recommendation, <table> structure should only be used for tabular information that needs to be represented like that i.e. table tags should only be used to build ... a table.
This implies to better test html rendering on various browsers as some are not so standard compliant, you know from what I'm talking about ;-)

But when using tables to represent information, we could benefit from a Datagrid improvements.

As an example is always more explicit I'll put some screenshots to show you and have your comments.
<!-- Name: RFC/Translation2/TemplateArticles -->
<!-- Version: 3 -->
<!-- Last-Modified: 2005/11/13 19:41:39 -->
<!-- Author: werner -->
# Template Articles

Maybe this could be a new article type or made for all types of articles:

  * The article itself is a Flexy template
  * all strings outside tags (or alt title attributes) are automatically wrapped by a translate function
  * strings to translate are automatically added to files / db etc.
  * versioning of the template?

## Pro
  * easier management of same layout for all languages of an article

## Needs
  * Flexy needs to accept a string as template, not only a file
  * automatic cashing on articleId / lang

## Comments

    [aJ] wmk: yeah it'd be a html article that gets parsed by flexy before output
    [aJ] wmk: I'll probably wait until I start on the SGL_Item stuff to do that
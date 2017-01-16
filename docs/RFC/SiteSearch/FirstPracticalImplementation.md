<!-- Name: RFC/SiteSearch/FirstPracticalImplementation -->
<!-- Version: 10 -->
<!-- Last-Modified: 2006/07/07 16:10:42 -->
<!-- Author: rbenea -->
# First Practical Implementation of Global Site Search
[[TOC]]

I am now implementing a global search module for one of my projects. This is what I did based on already made suggestions:

== Solutuin description == 

I want to search for articles (in title and body), documents (in title and description) and photo gallery (in photo description). As described before I created a separate module 'search' with a 'searchMgr' that will call the 'globalSearch' method from articleViewMgr, documetViewMgr and GalleryMgr. The global search method accepts a 'searchParameters' array and returns a 'results' array.

In 'searchParameters' there can be used the following  keys:  keywords, category_id,  role_id, user_id, from, limit. There are no mandatory keys that need to be passed and also can be used other keys too. Every module's 'globalSearch' method takes the data it needs and is responsible to manage if  some of the keys are missing.

The returned 'results' array is like this (adapted for articles):


            $results = array(
                        'properties' =>  array(
                            'module' => 'publisher',
                            'manager' => 'articleView',
                            'numResults' => 1 (number of results)
                          ),
                          'resultsList' => array(
                             'link' => 'http://....', (mandatory)
                             'title' => 'This is the title, click me to go to the link',
                             'tyle' => 'Static html article',
                             'description' => 'Here will be the first 250 chars from from bodyHtml',
                             'path' => 'PublisherRoot > example'
                          ),
                );

If there are errors during the search the module will return an empty array. Each module is responsible for the method of search.

The global search list template will display:
    - the link,
    - the title with the link on it if title is supplied
    - the other fields if specified.

By providing the 'type' field we can set separate templates for showing a result item. In this case the result item will show like this:


    This is the title, click me to go to the link [link to http://....]
    Here will be the first 250 chars from from bodyHtml ...
    Path: PublisherRoot > example

For example for gallery will list only the photo name and the path, etc..

Also the search can be limited only to the desired modules. If you search in more then one module the result will show only the first 5 (for ex) results from every module. From there you can see the other results only from one module at a time and here will use the pager. For example:


    34 Articles found, showing articles 1 to 5
    {list of article}
    Click here for the next articles
    
    7 Documents found, showing documents 1 to 5
    {list of documents}
    Click here for the next documents

This is working for the moment and I hope that I'll provide an on-line demo soon.

## Patch files

In siteSearch_patch.txt you find the first version of the code. It contains only new files so I should work on any SGL 0.6.x framework without damaging the existing install. It has some things hard coded but it works. For the moment it searches in articles and documents only. This code is provided as a starting point for development. Any contribution is welcomed. 

## Future ideas (need help on this):
 * provide a score for every result and order the results by the score
 * combine the results from more modules and show them in one list ordered by score
 * index pdf, excel, word etc.. documents also
 * implement an indexed global search database and make the search faster

## Comments:
 * Mattt: please make the results return the portions of the target that triggered the search hit - eg.  'description' => 'Here will be the 250 _most relevant_ chars from from bodyHtml'

 * omniton: i was thinked about global search. How about module specific search addons? On search event the search manager:[[BR]]
   - looks up search addon for every registered maodule;[[BR]]
   - includes addon file;[[BR]]
   - initiates search class;[[BR]]
   - calls init method with search params;[[BR]]

 * demian: i think we just need the equivalent of _implements searchable_ so each manager that can search data would have a suitably-named method that could be invoked by a search process. workflow would involve grepping filesystem for all selected modules, getting dependent managers, determining which implment searchable, and aggregating a query accordingly.
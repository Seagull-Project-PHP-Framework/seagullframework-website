<!-- Name: Design/AdminGUI/ImprovingLayout -->
<!-- Version: 5 -->
<!-- Last-Modified: 2006/11/30 16:39:13 -->
<!-- Author: lakiboy -->
## STATUS: ONGOING

# Improving Admin Backend Layout, i.e. default_admin theme

## Overview

There are still some IE specific bugs unfixed. Moreover IE7 has been released. In order to minimise the use of css hacks, we'll first clean up current layout. Then we'll use a recently created cssHelper (isBrowserFamily) which allow to identify the browser family and its version for IE.

Also main stylesheet (core.php) need to be improved in its structure to better separate css rules that are related to the main structure (display, position, widths ...) from those which are more related to styling our elements (color, font, ...).

Last but not least, I'd like to enforce naming conventions in our stylesheets.

### Note about naming conventions
Two models are often opposed in naming our css elements (classes and ids) :
 * structural naming
 * presentational naming

See [http://www.stuffandnonsense.co.uk/archives/whats_in_a_name.html]
and [http://meyerweb.com/eric/thoughts/2004/06/26/structural-naming/]

Basically structural naming is giving a name as per the meaning of your element within your web page (e.g. branding, navigation, additionalInformation, ...) whereas presentational naming establishes a relationship between the name and the style you want to give it (e.g. topNavigation, header, leftBar, ...) or even worse (leftGreenBar, topRedTitle). The latter is completely useless as you would lose the benefit of reusable css rules.

Imagine some useful information you want to provide in your website. You're used to put it in a right sided bar. A widespread habit is to name this element (say you put that information in a <div>) "rightSideBar".
Six monts later you want to put this information on top of your main content, so what happens? You have to change your css rules, of course, but you will also need to update the name of your css element, say "topBar", which is weird! So a better naming would have been "additionalInformation" which is semantically meaningful.

On the other hand CSS have to be helpful for you who design interfaces. And you don't want to create different rules for elements that share the same layout properties. A good example could be some rules that help you display your form fields in a column like style. In the structural naming model, you would use "contactForm", "newUserForm", ...
That is where the presentational naming model will be useful : you can create a "twoCol" or "threeCol" or "sideBySide" CSS element that you will apply to every HTML element that need to be displayed like this.

Of course there have been some passionated discussions about this topic and some will always claim one model is better than the other. I would say half/half. Try to take the benefits from both models. First try to think what is the meaning of the element you add, and thus give it a "structural name". If you cannot find one, or obviously your element purpose is to help you define a generic styling rule, then give it a "presentational name". That's it! Well that's my opinion. And that's what we're going to enforce in Seagull default themes.

# How to achieve this

First we're going to make the big picture of our page structure, i.e. positioning blocks in the page.

Then we'll look at basic tag style.

Finally we'll identify some generic CSS elements usefull to display some information such as form, fields and table.


## Main Structure
As a starter, from [http://www.stuffandnonsense.co.uk/archives/whats_in_a_name.html]

Also see [http://www.stuffandnonsense.co.uk/archives/whats_in_a_name_pt2.html]


    So what should we name our elements?
    
    Here are my suggestions for main structural divs
    
        * Container div: container
        * Header or banner div: header
        * Main or global navigation div: main-nav
        * Left or right side columns: sidebar-a, sidebar-b
        * Content div: content
        * Footer div: footer
    
    And for other page elements
    
        * Sub-navigation list: sub-nav
        * Search: search
        * Search results: search-results
        * Copyright information: copyright
    


## Default Rules For Tags
_Block elements_

 body :

 h1, ..., h6 :

 p

 blockquote

 ul, ol, dl (li, dt, dd)

_Special_

 form, fieldset, legend (input, select, textarea ...)

 table, tr, th, td

_Inline elements_

 a

 img

 q


## Generic Presentational Elements
They may have a nice semantical meaning such as:

.tooltip, .button, ...

or a pure presentational one such as:

.sideBySide, .twoCols, .box1, .box2

## Comments

#### Comment by lakiboy on Thu Nov 30 10:39:13 2006
Hey Julien!
What about wrappers? Very often we have to wrap page elements to give it unique look and feel i.e. bg image, color, padding etc. How should we name them? Currently there are names like:
 * inner-wrapper,
 * outer-wrapper,
 * wrapLeft,
 * wrapRight,
 * etc.

Regarding suggested names for main structural divs. _header_ is pure presentational naming as well as _footer_. There is a possibility to replace them with _brandinding_ and _copyright_ accordingly. But to be honest I personally like first variant more.

[[AddComment]]

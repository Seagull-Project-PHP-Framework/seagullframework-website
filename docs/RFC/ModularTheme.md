<!-- Name: RFC/ModularTheme -->
<!-- Version: 3 -->
<!-- Last-Modified: 2005/11/13 19:26:59 -->
<!-- Author: werner -->
# Modlular Theme Support
[[TOC]]

This is a RFC for a modular theme support. The RFC is what i think should
be better for sanely handling themes, but is possible that i'm missing a
lot of points, so please review.

## What is a css theme?

A css theme is:
  * _style.php_
  * _vars.php_ - where PHP variables are defined
  * _core.php_ - core CSS code of the app / site
  * _$nav.php_ - the navbar

A barebone _style.php_ looks like

    #!php
    <?php
    /* PHP functions for client cache, whatever */
    
    theme_include_once(vars.php)
    
    theme_include_once(core.php)
    
    theme_include_once($nav.php)
    
    /* PHP function for locale, ... */
    ?>
theme_include_once() is a wrapper around include_once() that check if a file is available, if not it picks the one in default/css. Like flexy does for the rest of theme.

Isn't it very simple? yep this is simple because it is generic. Further modularization could be done linking _layout.php_ and _common.php_ instead of _core.php_.

As you can see i've removed _style.local.php_. Why? Because it was an easy
workaround that could help someone but doesn't solve anything for everyone
(or the most at least). Let's have an example:
  * SGL put style.local.php include in the first <?php ?> block, just before the css code.
  * A wants to have the css code of his apps separated from the sgl one so it put it in _style.local.php_. And he is happy.
  * B wants the same that A but his code override some sgl code. But it don't works for him. "WTF???" is B screaming loud, "the FSCKED sgl code is overriding mine!". B resolves it moving the call to _style.local.php_ on the second <?php ?> block.
  * C wants to override some things but at the same time want to add some php variables so that they can be used by sgl css code too. C practices yoga so he doesn't scream but it is dissapointed of adding another include_once() for _style2.local.php_

2 of 3 people are not happy with out of the box _style.local.php_ because it is not *generic*. So if you need something *special* just add theme_include_once() / include_once() in your *own* application and since every file now is a php file you can put it everywhere you want.

<!-- Name: Standards/DevelopmentProcess -->
<!-- Version: 5 -->
<!-- Last-Modified: 2007/10/15 17:46:28 -->
<!-- Author: malber -->
# Suggestions For a Development Process
## Introduction
The development process covers everything that happens from when you get your first ideas of what kind of app you want to build, to when the app goes live on the web.

The reason for these few lines is a surprising amount of people appear to set about creating web projects in the most difficult manner possible.

## Essentials
The following is a list of recommendations, if any of them sound foreign to you, please give them a try, you'd be surprised how much time you could save yourself:
 * Work with requirements signed off by the customer (don't laugh please ;-)) 
 * Use a decent editor that at least shows you class views, a summarised list of methods/properties, clicking on a class name takes you to the class definition, a good search functionality, etc.  Zend Studio Professional is recommended and is well worth the investment.
 * Don't use pre-packaged versions of PHP/Apache/MySQL (XAMP, etc), they tend to be heavily customised, offering non-standard functionality and very difficult to upgrade
 * Don't use the Windows installer version of PHP from the downloads section on the PHP site because that gives you a CGI version of PHP which is quite crippled
 * Always develop on your local machine and deploy the results to your webserver, in other words, don't even think about developing on a remote server, this is a huge waste of time
 * Developing locally on Windows and deploying to a Linux machine is fine, Seagull runs identically on both
 * *Use a debugger*: you will never regret this, it will initially cut your development time in half, or down to a third, then your development time will be further reduced when you learn to fully take advantage of debugging. Zend Studio has an excellent debugger integrated, use the Zend Debug Server and not the local one.
 * Use [Unit Tests](/wiki:Standards/UnitTesting/), have tests for all your code, if you really want to be efficient write the test before you write the code
 * Manage all your own work (ideally you will only have to develop modules for Seagull without needing to alter the core) with a source code management tool, like CVS but ideally SVN, very easy to setup, will save you hours of time when you need to track through previous revisions
 * If it hasn't happened yet, become comfortable with the [PEAR package manager](http://pear.php.net/manual/en/installation.getting.php), a commandline tool for managing your PEAR libs and Seagull packages.

## Desirable
The following are a good idea, but you can survive without them:
 * Comment all your classes and methods with PHP-doc style comments (almost identical to javadoc)
 * Comment non-obvious passages in your code indicating what's going on, will be a life-saver in 6 months time when you need to refactor something
 * Use a [decent browser](http://www.mozilla.com/firefox/) and the [Web Developer Toolbar](https://addons.mozilla.org/extensions/moreinfo.php?id=60) plugin, this allows you to tweak divs, CSS, produce xhtml-compliant code and handle js probs effortlessly, amongst other things.
 * don't use PHP for building templates unless
   * you're a mom & pop shop and do everything in the app yourself
   * you know your front end devs intimately and trust them with your reputation
   * it's just you and your mate developing, and it's only a personal homepage so doesn't need to be scalable, nor will outside frontend devs ever be used

## Rationale
If saving huge amounts of time and becoming a more efficient developer aren't compelling enough reasons for you, consider that if you indicate having experience with any/all of the above on your CV, you can probably add $10k on the salary you demand for your next job.

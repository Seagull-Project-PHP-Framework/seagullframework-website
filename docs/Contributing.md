<!-- Name: Contributing -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/09/20 19:32:59 -->
<!-- Author: demian -->
# Contributing to Seagull project
[[TOC]]

You want to submit some of your work with the project?

You noticed a bug and want to submit a fix for it?

Well you came to the right place. Please take a minute to read following guidelines to help you create efficient tickets, and submit correct patches.

## Creating a ticket
I hear you say: "Hum I know how to create a ticket in Trac, a 10 year old kid knows how".

Well it's true that it is easy to create a patch. But chances are that an efficient patch will be given more attention than a cheesy one.

First you have to [be registered](http://seagullproject.org/user/register/) to be able to create a ticket.

Then when you create the ticket, we ask you include some basic information to help us search through Trac efficiently:

 * Short summary: be explicit, also try to be concise
 * Description: when creating a ticket (not for editing a previously created one) please give details about the bug you  noticed, the feature you want to add.
 * Priority: default to normal. Use high only when the bug might be critical. For any security issue you might have noticed, better try to contact Demian or AJ on #seagull, or leave a ticket with no explicit details.
 * Component: which part of Seagull does it apply to? There are 3 main sections : SGL (for core libraries), modules and those related to the themes.
 * *Severity (new)*: we use this field to give a status to tickets: open (at creation), analysing (when thinking how to fix, implement), in progress (when working on it), need feedback (when you want others to give their opinion, advice, etc)
 * Assign to: well assign it to the person who should be notified for this patch. This means you at first step. Then assign it to someone else for feedback, and finally to Demian, as he's the one who commits patches.
 * Milestone: current milestone is 0.6 (this will be updated as per current milestone)
 * Version: N/A
 * Keywords: entering 2/3 keywords here may help identifying tickets more easily
 * Cc: a email if you want to be notified

That's it, preview it, correct if needed, then submit it.

Thanks for sharing with us ;-)

## Creating a patch
First you need to have a fresh working copy of the latest code.

We use Subversion as our versionning tool. If you're new to it or want to check what's the latest code available, check [our SVN page](http://trac.seagullproject.org/wiki/Installation/FromSVN)

We want to ensure that code is homogeneous as if it had been written by the same person. So please check our [Coding Standards (CS)](http://trac.seagullproject.org/wiki/Standards/CodingStandards). You may also find usefull to look at other [standards](http://trac.seagullproject.org/wiki/Standards) we follow.

So, assuming you're in your local copy of Seagull, do a

    $ svn up
Notice the revision number.

Make your changes and see [how to create a patch](http://trac.seagullproject.org/wiki/Code/SubmittingPatches)


    #!html
    <div style="float:right;width:25%;background:#ffffdd;padding:5px;">
    <p style="font-weight:bold">Note for Windows users</p>
    <p>Using Tortoise is fine for updating your working copy, reverting files, visually see which files are modified ...</p>
    <p>But we recommend you create patches using the subversion command line utility.
    </div>

## What name should I give to my patch?

Well, try to give it a short explicit name. Appending "_revisionNumber" to your file name may help when you want to go back to a previously created patch. But this is not mandatory.

## Are Coding Standards Important?
Coding standards are very important and patches that fail to observe [browser:/trunk/CODING_STANDARDS.txt Seagull CS] will not be accepted.  There is an excellent package available in PEAR called [PHP_CodeSniffer](http://pear.php.net/package/PHP_CodeSniffer/) that can help you learn the standards.

To install simply run:


    $ pear install PHP_CodeSniffer

Then run the provided script against your code:


    $ phpcs /path/to/my/file.php

## Uploading Patches to Trac

Now you can go back to the ticket you created (or any ticket you want to submit a patch to) and click on "attach file"

You're done. Thanks again for contributing.
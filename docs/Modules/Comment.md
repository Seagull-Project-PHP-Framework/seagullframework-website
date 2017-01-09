<!-- Name: Modules/Comment -->
<!-- Version: 3 -->
<!-- Last-Modified: 2006/09/07 02:50:53 -->
<!-- Author: demian -->
[[TOC]]

## Comment Module
Using the comment module you can add comments to any other module in Seagull.  Examples have been implemented for the FAQ module and the Publisher module.

## Getting comments to work for Publisher
By default comments are implemented for articles, to enable them edit the module's conf.ini ([HOWTO](/wiki:Howto/UsingConfigFiles#ModuleConfig/)) file and set [commentsEnabled] = true.

The hook that calls the comments is found in the _cmd_list action method, it looks like this:


    //  display comments?
    if (SGL::moduleIsEnabled('comment') && !empty($this->conf['ArticleViewMgr']['commentsEnabled'])) {
        $output->aComments = $this->da->getCommentsByEntityId('articleview',
            $input->articleID);
    }

The data access object ($this->da) is setup in ArticleViewMgr's constructor:


    //  enable comments if configured
    if (SGL::moduleIsEnabled('comment')) {
        require_once SGL_MOD_DIR  . '/comment/classes/CommentDAO.php';
        require_once SGL_CORE_DIR . '/Delegator.php';
        $dao = &CommentDAO::singleton();
        $this->da = new SGL_Delegator();
        $this->da->add($dao);
    }

This approach can be used for any of your own modules.

## How to call getCommentsByEntityId() from the Data Access layer
Notice the first argument to the data access call:


    $this->da->getCommentsByEntityId($entityName, $entityId)

The entity name is the manager's name without the 'mgr' part, and all in lower case, same as what's used in the URI.  The second argument is optional, and is the ID of your current entity.

This means any amount of comments can be associated with your manager which manages a particular entity, or in the case of articles, comments can be associated with an instance of the entity.

## Using comments with FAQs
Imagine you want to allow users to add comments to your FAQ section, say to get ideas how to improve the existing FAQs.  Simply enable comments in the modules config file, and you're done.


## Still to come
The current implementation is very simple, except in the sense that it can be used to associate comments with any entity in the system.  To be a full-fledged comment manager it still needs
 * comment administration (I'd like to see something similar to what the latest s9y has)
 * make comments approvable - db field exists for this
 * integrate CAPTCHA - should also be quite easy with our existing CAPTCHA implementation
 * integrate http://www.gravatar.com/ icons for authors
 * ability to hook in with anti-spam web services like http://akismet.com/
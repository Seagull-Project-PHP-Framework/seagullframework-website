<!-- Name: RFC/LookAndFeel/BackendVsFrontend -->
<!-- Version: 2 -->
<!-- Last-Modified: 2005/12/20 17:53:39 -->
<!-- Author: jcasanova -->
# Seaparate Backend from Frontend design

## Deciding when to load admin interface

We can check if both user's role and specific manager allow to load the admin GUI.

We currently only have the user's role. Then we have to know if the manager allows adminGUI interface.

The best way I've come up so far to achieve this is by adding an 'adminGUI' parameter set to true in conf.ini file for every Manager that is concerned.


    Example in module/publisher/conf.ini
    
    [ArticleViewMgr]
    requiresAuth    = false
    
    [ArticleMgr]
    requiresAuth    = true
    fleschScore     = false
    adminGUI        = true
    
    [CategoryMgr]
    requiresAuth    = true
    adminGUI        = true
    ...

This allows to load the 'admin interface' depending on the user's role and areas of the application currently defined by the !Manager name.

## How to implement that ?

This could be added in the postProcess method of the output renderer.


    function postProcess(/*SGL_View*/ &$view)
        {
            $process =  new SGL_Process_BuildOutputData(
                        new SGL_Process_SetupWysiwyg(
                        new SGL_Process_GetPerformanceInfo(
                        ''' new SGL_Process_SetupInterface( '''
                        ~~ new SGL_Process_SetupNavigation( ~~
                        new SGL_Process_SetupBlocks()
                       ))));
    
            $process->process($view);
        }

Thanks to Blocks improvements we should replace the specific navigation block and needed method to load it by a classical block.
Thus if we are in the 'admin interface' we can easily have a really different menu. I've also added an AdminMenuMgr to configure your admin menu.

Note : What I found most confusing for an enduser is the fact that when modifying the navigation of the site you had frontend and backend items mixed together. In my point of view, only the super admin of the site that is responsible for the configuration of Seagull needs to view/create admin items of navigation.

## What to do

Currently I've added an 'admin' theme. And my 'setAdminInterface()' method only changes $output->theme value.

In my point of view changing from one backend theme to a frontend one is better than having too many code inside templates to decide what to display.

Thanks to LiveUser improvements it should be easier for the system to know if the user has right on any operation.
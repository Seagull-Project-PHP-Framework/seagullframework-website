<!-- Name: Howto/DB/ModifyingPagerAppearance -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/03/29 02:09:40 -->
<!-- Author: demian -->
# Modifying the Appearance of the Pager

For a straightforward example of how to get Seagull to take care of your pagination for you, see the [CodeExamples#AutoPagination code example].

If you would like to modify the way the pager widget looks there are many ways to do so, here is an example of how to use graphics for the next/previous buttons.


    // setting URL for link images in pager Bar
    $themePrefix = SGL_BASE_URL . '/themes/' . $_SESSION['aPrefs']['theme'] . '/images/';
    $pager_options['firstPagePre'] = '<img src="' . $themePrefix . 'arrow_left.gif" /><img src="' . $themePrefix . 'arrow_left.gif" />';
    $pager_options['firstPagePost'] = '';
    $pager_options['prevImg'] = '<img src="' . $themePrefix . 'arrow_left.gif" />';
    $pager_options['altPrev'] = 'previous page';
    $pager_options['nextImg'] = '<img src="' . $themePrefix . 'arrow_right.gif" />';
    $pager_options['altNext'] = 'next page';
    $pager_options['lastPagePre'] = '';
    $pager_options['lastPagePost'] = '<img src="' . $themePrefix . 'arrow_right.gif" /><img src="' . $themePrefix . 'arrow_right.gif" />';

This would be added in seagull/lib/SGL/DB:getPagedData();
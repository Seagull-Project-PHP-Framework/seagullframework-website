<!-- Name: Howto/InputValidation -->
<!-- Version: 2 -->
<!-- Last-Modified: 2006/11/30 15:44:42 -->
<!-- Author: demian -->
# Input validation
[[TOC]]
## Getting and cleaning data from request


        $input->from            = $req->get('frmFrom') ? $req->get('frmFrom') : 0;
        $input->catID           = $req->get('frmCatID');
        $input->deleteArray     = $req->get('frmDelete');
        $input->queryRange      = $req->get('frmQueryRange');
        $input->bodyValue       = $req->get('frmBodyName', $allowTags = true);
    
        //  request values for upload
        $input->assetFileArray        = $req->get('assetFile');
        $input->assetFileName         = $input->assetFileArray['name'];
        $input->assetFileType         = $input->assetFileArray['type'];
        $input->assetFileTmpName      = $input->assetFileArray['tmp_name'];
        $input->assetFileSize         = $input->assetFileArray['size'];
    
        //  determine user type
        $input->isAdmin = (Session::getAuthLevel() == SGL_ADMIN);
    
        //  request values for save upload
        $input->document = (object)$req->get('document');

The following variable cleanup has occured:

  * any whitespace stripped
  * default values assigned
  * casting to more complex datatypes, ie. objects, arrays
  * auto-stripping of HTML except where $allowTags = true specified
  * when HTML allowed, all JavaScript stripped to minimise XSS attacks
  * magic quotes dealt with regardless of installation default

## Setting locale-specific error messages
When validating your input you can also set translated error messages.  Use the master language in your module, english, and call the translate() helper function in the templates:


        require_once 'Validate.php';
        $aErrors = array();
        if ($input->submit) {
            $v = & new Validate();
            if (empty($input->contact->first_name)) {
                $aErrors['first_name'] = 'You must enter your first name';
            }
            if (empty($input->contact->last_name)) {
                $aErrors['last_name'] = 'You must enter your last name';
            }
            if (empty($input->contact->email)) {
                $aErrors['email'] = 'You must enter your email';
            } else {
                if (!$v->email($input->contact->email)) {
                    $aErrors['email'] = 'Your email is not correctly formatted';
                }
            }
        //  if errors have occured
        if (is_array($aErrors) && count($aErrors)) {
            Output::msgSet('Please fill in the indicated fields');
            $input->error = $aErrors;
            $this->validated = false;
        }
        return $input;       

then in the template:


    <span class="error" flexy:if="error[username]">{translate(error[username])}</span>
    <input type="text" name="user[username]" id="user[username]" value="{user.username}" />


## Determine which button was pressed
You have a list where you can check several items. At the bottom of the form are two buttons:


    {translate(#With selected bookmark(s)#)}: 
    <input type="submit" name="delete" value="{translate(#delete#)}" 
            onclick="return confirmSubmit('bookmark', 'bookmarks')" /> 
    <input type="submit" name="resetCounter" value="{translate(#reset Counter#)}" 
           onclick="return confirmCustom('{translate(#You must select a bookmark to reset#)}', '{translate(#Are you sure you want to reset this bookmark(s)?#)}', 'bookmarks')"/>

In the $myManager->validate($req, &$input) is this code:

    
        $input->action = $req->get('action') ? $req->get('action') : 'list';
    
        // determine action based on which button was pressed
        if ($req->get('delete')) { $input->action = 'delete'; }
        if ($req->get('resetCounter')) { $input->action = 'resetCounter'; }
    ?>

Note: if you use image buttons, ie. <input type="image"...>, don't forget to put a value even if you don't use it:
 * if the image is broken, the navigator will display a plain button with the value as face name;
 * if you don't put a value, the trick above just doesn't work...

[[AddComment]]
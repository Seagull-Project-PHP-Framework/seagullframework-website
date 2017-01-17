<!-- Name: RFC/Modules/Gallery -->
<!-- Version: 12 -->
<!-- Last-Modified: 2006/10/18 12:23:05 -->
<!-- Author: werner -->
# RFC for Rares' Photo Gallery module

### Download

Download from here: http://trac.seagullproject.org/ticket/939

### What the zip contains

/modules/gallery[[BR]]
/www/themes/default/css/gallery.php[[BR]]
/www/themes/default\_admin/css/gallery.php[[BR]]
/www/themes/default\_admin/images/16/module\_gallery.gif[[BR]]

### Features
 * List thumbnails
 * Display big image
 * Add images
 * Edit images
 * Delete images
 * Reindex images - this is the thing that will make you life more easier. It performs a synchronization between the files store in the image directory, thumb directory and DB. The action performs this tasks:
   * if you add a new file to image directory it will add it to DB and generate the thumb for it
   * if you delete a file from the image directory it will delete it from DB and it's thumb file
   * if a file exists then it checks if it is the right size (image+thumb) and performs a resize if necessary
   * it deletes orphan DB records
   * it deletes orphan thumb files

### Installation
Just unzip over a fresh SVN checkout of 60bugfix. The zip will not overwrite existing files so it's a safe install. Perform a fresh install. Add the gallery/galleryAdmin to the Admin menu.

If you already have SGL installed, check if you have the PEAR::Image\_Transform installed in your /lib/pear folder. You have to create the tables, add the tables to the conf table list, auto detect the module etc... by hand.

### Description
This is a small and basic Photo Gallery module. It is design for small sites that don't need a big and more complex photo galley. If you are looking for something like that search for Gallery2 module.

As a basic idea, you store, by default, the images in /www/images/Images/gallery directory, the thumbnails in /www/images/Images/gallery/thumb directory and also in DB. The stored files are renamed like this: $gallery\_id . $fileName . '.jpg'

The gallery table structure is like this:
 * gallery\_id
 * category\_id
 * image - the image file name
 * description
 * item\_order - so you can order the images for listing (will be implemented soon)

For gallery list it uses CSS only.

### Suggestions
  * decide which category to add a new image to when scanning for new images. This would need a screen where you choose the category before scanning
  * scan maybe only the current category (and it's children). Would be good to reduce load on big galleries
  * define a standard description for new images (or use filename...) Somehow on a first test all images got description "im" (filenames were like img\_7714.jpg )
  * mass rename / edit of images: just show thumbnails and a text input, maybe a category chooser
  * preview pic of galleries. at the moment you don't see in galleryview that the current gallery has subgaleries / categories. Like mig does. see:  http://www.hallstatt.net/gallerie/
  * save a timestamp and maybe other meta data when adding images. this way you can make a "most recent images" block.
  * Refactor code and make helper methods for adding / deleting / resizing to avoid redundant code in e.g. _cmd\_insert() and _cmd\_reindex()
  * save pics per category in different directories, which makes uploads more easy and "search for new" just goes through all dirs... So a customer can upload stuff more easily
  * Introduce DA-Layer, so one can easily switch to filebased gallery etc...
  * Implement Item Order Logic as it's currently in database ;)
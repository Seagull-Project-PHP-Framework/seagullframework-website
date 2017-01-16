<!-- Name: Howto/WorkingWithImages -->
<!-- Version: 10 -->
<!-- Last-Modified: 2007/01/22 11:58:15 -->
<!-- Author: lakiboy -->
<!-- Status: Original -->

# Working with images
* TOC
{:toc}

## Introduction
There are some new tools available in Seagull 0.6.2 for working with images. Currently Media module uses it's abilities to process images after downloading files on server. The everyday tasks for image related processing after one is uploaded are:
 * fit image to certain size (not wider than XxY),
 * create thumbnail(s),
 * add watermark to authorise image source,
 * make all images same size like XxY filling the rest of canvas with some background color and positioning original image in the center (very useful when developing well-formed galleries opened in pop-up window),
 * applying borders (this can be also done with CSS, but we can reduce the mark-up),
 * and everything mentioned above at once.

Everything mentioned above is now available in Seagull.

## Package structure
### Files
 * main library - _Image.php_ (located in _SGL\_CORE\_DIR_)
 * transformations - _ImageTransform_ directory, which contains transformation files (located in _SGL_CORE_DIR_),
 * tests (located in _SGL\_CORE\_DIR/tests_):
	* \''!ImageTest.ndb.php'',
	* \''!ImageConfigTest.ndb.php'',
	* \''!ImageTransformStrategyTest.ndb.php'',
	* \''image.ini'',
	* \''chicago.jpg'',
 * _GD\_SGL.php_ - driver (located in _SGL\_LIB\_PEAR\_DIR/Image/Transform/Driver_).

### Classes
 * _SGL\_Image_ - main class, executes strategies, creates thumbnails etc,
 * _SGL\_ImageConfig_ - class for dealing with _SGL\_Image_ configuration i.e. parsing ini files, extracting strategy parameters from a string,
 * _SGL\_ImageTransformStrategy_ - abstract strategy, which is inherited by
	* \''SGL\_ImageTranform\_ResizeStrategy'' - resize strategy,
	* \''SGL\_ImageTranform\_AddImageStrategy'' - strategy for adding images across the original one,
	* \''SGL\_ImageTranform\_ColorOverlayStrategy'' - overlays image with color,
	* \''SGL\_ImageTranform\_AddBorderStrategy'' - strategy for adding border,
	* \''SGL\_ImageTranform\_CanvasResizeStrategy'' - strategy for resizing canvas,
 * _Image\_Transform\_Driver\_GD\_SGL_ - driver, which extends some functionality of original _GD_ driver supplied with _PEAR::Image\_Transform_ package.

### Api

[wiki:Howto/WorkingWithImages/Api].

### Code examples
[wiki:Howto/WorkingWithImages/CodeExamples]

## Transformations
The main feature of image tools is an ability to transform specified image and it's thumbnails using the _driver_ to perform this. The driver, which use five default strategies, is _GD\_SGL_, but can be any other _PEAR::Image\_Transform_ can offer. In theory one image can be transformed by two different strategies using two different drivers e.g. with _GD_ for resizing and with _NetPBM_ for rotating. A developer can easily write his/her own extending _SGL\_ImageTransformStrategy_ abstract class.

Below is an example of the image transformed by all five currently available transformations:

[[Image(transformation\_examples.jpg, nolink)]]

 * original image was resized to certain size,
 * canvas was enlarged filled with specified background color,
 * performed color overlay (from right bottom corner),
 * watermark image was added,
 * applied border.

## Configuration
[wiki:Howto/WorkingWithImages/Transformations].
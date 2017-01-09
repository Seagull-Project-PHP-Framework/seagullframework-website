<!-- Name: Howto/WorkingWithImages/Transformations -->
<!-- Version: 9 -->
<!-- Last-Modified: 2006/12/22 16:24:00 -->
<!-- Author: lakiboy -->
## Transformations

### Resize
Resizes image by given _width_ and/or _height_. In other words, resizes image to fit specified criteria. At least one dimension must be specified, otherwise _PEAR_Error_ object will be returned.

Parameteres:
 * width (in pixels)
 * height (in pixels)

Ini-file example:

    [default_small]
    resize = width:120,height:90

Image example:

[[Image(transform_resize.jpg,nolink)]]
[[br]][[br]]

### Canvas resize
Resizes image to exact _width_ and/or _height_ filling with background _color_ and placing original image at _position_. At least one of specified dimensions must be larger than image size you are applying the transformation to, otherwise _false_ will be returned and transformation is skipped.

Parameters:
 * width (in pixels)
 * height (in pixels)
 * color
    * ''hex'' notation is allowed e.g. ''!#000000'',
    * or can be specified as name e.g. ''black'',
    * default value: ''white'',
 * position:
    * only one position is suppored at the time of writing - ''center'',
    * default value: ''center''

Ini-file example:

    [default_large]
    canvasResize = color:#dddddd,width:250,height:250

Image example:

[[Image(transform_canvas-resize.jpg,nolink)]]
[[br]][[br]]

### Adding border
Creates border. At the time of writing only 1 pixel border is applied per one color specified in configuration, i.e.

    border = black
will add 1 pixel width black border around the image. To apply 2 pixel black border you have to specify:

    border = black,black
This behaviour may change in future. Please note, that image size is enlarged after border is applied, i.e. creating 1 pixel border will enlarge image's width and height by 2 pixels. Probably, an optional argument will be added in future, which will allow to draw border over existing image.

Parameters:
 * comma-separated list of colors,
 * color can be specified via _hex_ notation e.g. _#ffffff_,
 * or as name e.g. _white_

Ini-file example:

    [default_medium]
    border = #999999,white,#999999

Image example:

[[Image(transform_border.jpg,nolink)]]
[[br]][[br]]

### Adding image
Adds another image across the orginal one. In other words, this transformation creates a watermark using file specified in configuration. In case it is omitted _PEAR_Error_ object is returned. You can tweak watermark position using supported parameters:
 * file - path to image (specified from _SGL_APP_ROOT_),
 * alingX - horizontal alignment, available values:
    * ''left'',
    * ''right'',
    * default value: ''right'',
 * alignY - vertical alignment, available values:
    * ''top'',
    * ''bottom'',
    * default value: ''bottom'',
 * paddingX (in pixels),
    * if ''alignX'' value is ''left'' then ''paddingX'' is applied from left,
    * if ''alignX'' value is ''right'' then ''paddingX'' is applied from right,
    * default value: ''0'',
 * paddingY (in pixels)
    * if ''alignY'' value is ''top'' then ''paddingY'' is applied from top,
    * if ''alignY'' value is ''bottom'' then ''paddingY'' is applied from bottom,
    * default value: ''0''
 
Ini-file example:

    [default_large]
    addImage = file:www/images/seagull.png,alignX:right,alignY:bottom,paddingX:15,paddingY:15

Image example:

[[Image(transform_add-image.jpg,nolink)]]
[[br]][[br]]

### Color overlay
Overlays image part with color.

Parameters:
 * align - alignment, available values:
    * ''top'',
    * ''bottom'',
    * ''left'',
    * ''right'',
    * default value: ''bottom'',
 * size (in pixels), represents _width_ or _height_ depending on alignment,
    * default value: ''10'',
 * color,
    * default value: ''white'',
 * trans - overlay transparency, i.e. _0_ means no transparency, _100_ - absolute transparent,
    * values range from ''0'' till ''100'',
    * default value: ''0'',
 * paddingTop (in pixels),
 * paddingBottom (in pixels),
 * paddingLeft (in pixels),
 * paddingRight (in pixels)

Ini-file example:

    [default_large]
    colorOverlay = color:blue,align:bottom,paddingLeft:10,paddingBottom:10,size:30,trans:70

Image example:

[[Image(transform_color-overlay.jpg,nolink)]]

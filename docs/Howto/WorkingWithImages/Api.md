<!-- Name: Howto/WorkingWithImages/Api -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/12/13 02:40:03 -->
<!-- Author: lakiboy -->
## Image tools API

### SGL_Image


    // constructor
    SGL_Image($fileName, $moduleName); // args can be omitted
    
    // init image instance
    SGL_Image#init($fileName); // can accept ini-file name for parsing or already parsed array
    
    // operate
    SGL_Image#create($srcImage, $callback); // default callback function is 'move_uploaded_file',
    SGL_Image#replace($srcImage, $callback); // can be also set as 'copy' or 'move'
    SGL_Image#delete();
    
    // transform
    SGL_Image#transform('small'); // only thumbnail
    SGL_Image#transformAll(); // affects main image and all it's thumbs
    
    // other
    SGL_Image#getFileName();
    SGL_Image#getThumbnailNames();
    SGL_Image#generateUniqueFileName();
    SGL_Image#getPath();
    SGL_Image#getUrl();
    
    // or can be called statically
    SGL_Image::getPath();
    SGL_image::getUrl();

### SGL_ImageConfig


    // extracting parameters
    SGL_ImageConfig::getParamsFromFile($fileName); // ini file name
    SGL_ImageConfig::getParamsFromString($string); // config string for strategy
    
    // working with parameters
    SGL_ImageConfig::getAvailableParams();
    SGL_ImageConfig::getProperty($propName);
    
    // utils
    SGL_ImageConfig::cleanup($array);
    SGL_ImageConfig::paramsCheck($array);
    SGL_ImageConfig::getUniqueSectionNames($array);

### SGL_ImageTransformStrategy


    // constructor
    SGL_ImageTransformStrategy(&$driver); // instance of Image_Transfrom_Driver
    
    // init strategy i.e. load file to driver, init parameters
    SGL_ImageTransformStrategy#init($fileName, $paramString);
    
    // set params manually
    SGL_ImageTransformStrategy#setParams($array);
    
    SGL_ImageTransformStrategy#load($fileName); // load file to driver
    SGL_ImageTransformStrategy#save($saveQuality, $saveFormat); // currently save formats are not supported
    
    // performs transformation
    SGL_ImageTransformStrategy#transform();// defined by sub-classes
    

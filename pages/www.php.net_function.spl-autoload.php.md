# spl_autoload



Note, that the default autoload implementation is written in C land and is always slightly faster then your native PHP one.<br><br>Here is a trick to use default implementation with any configuration:<br><br>

```
<?php

    // Your custom class dir
    define('CLASS_DIR', 'class/')

    // Add your class dir to include path
    set_include_path(get_include_path().PATH_SEPARATOR.CLASS_DIR);

    // You can use this trick to make autoloader look for commonly used "My.class.php" type filenames
    spl_autoload_extensions('.class.php');

    // Use default autoload implementation
    spl_autoload_register();
?>
```
<br><br>This also works with namespaces out of the box. So you can write code like "use My\Name\Object" and it will map to "class/My/Name/Object.class.php" file path!  

---

Note this function will LOWERCASE the class names its looking for, dont be confused when it cant find Foo_Bar.php<br><br>also, unlike most other autoloader code snippets, this function DOES NOT translate underscores to slashes.<br><br>class Foo_Bar {}<br>will load foo_bar.php and will not try to load foo/bar.php<br><br>You can get around this with<br>spl_autoload_register(function($class) { return spl_autoload(str_replace(&apos;_&apos;, &apos;/&apos;, $class));});  

---

One small example that shows how you can use spl_autoload function in your MVC, Framewrk&apos;s applications. For example, will use the Loader class.<br> <br><br>

```
<?php

 class Loader
 {
        
    /**
     * Controller Directory Path
     *
     * @var Array
     * @access protected
     */
    protected $_controllerDirectoryPath = array();
    
    /**
     * Model Directory Path
     *
     * @var Array
     * @access protected
     */
    protected $_modelDirectoryPath = array();
    
    /**
     * Library Directory Path
     *
     * @var Array
     * @access protected
     */
    protected $_libraryDirectoryPath = array();
    
    
    /** 
     * Constructor
     * Constant contain my full path to Model, View, Controllers and Lobrary-
     * Direcories.
     *
     * @Constant MPATH,VPATH,CPATH,LPATH
     */
     
    public function __construct()
    {
        $this->modelDirectoryPath      = MPATH;
        $this->viewDirectoryPath        = VPATH;
        $this->controllerDirectoryPath = CPATH;
        $this->libraryDirectoryPath     = LPATH;
        
        spl_autoload_register(array($this,'load_controller'));
        spl_autoload_register(array($this,'load_model'));
        spl_autoload_register(array($this,'load_library'));
   
        log_message('debug',"Loader Class Initialized");
    }

    /** 
     *-----------------------------------------------------
     * Load Library
     *-----------------------------------------------------
     * Method for load library.
     * This method return class object.
     *
     * @library String
     * @param String
     * @access public
     */    
    public function load_library($library, $param = null)
    {
        if (is_string($library)) {
            return $this->initialize_class($library);
        }
        if (is_array($library)) {
            foreach ($library as $key) {
                return $this->initialize_class($library);
            }
        }                
    }

    /** 
     *-----------------------------------------------------
     * Initialize Class
     *-----------------------------------------------------
     * Method for initialise class
     * This method return new object. 
     * This method can initialize more class using (array)
     *
     * @library String|Array
     * @param String
     * @access public
     */    
    public function initialize_class($library)
    {
        try {
            if (is_array($library)) {
                foreach($library as $class) {
                    $arrayObject =  new $class;
                }            
                return $this;
            }
            if (is_string($library)) {
                $stringObject = new $library;
            }else {
                throw new ISException('Class name must be string.');
            }
            if (null == $library) {
                throw new ISException('You must enter the name of the class.');
            }
        } catch(Exception $exception) {
            echo $exception;
        }
    }    
    
    /**
     * Autoload Controller class
     *
     * @param  string $class
     * @return object
     */
     
    public function load_controller($controller)
    {
        if ($controller) {
            set_include_path($this->controllerDirectoryPath);
            spl_autoload_extensions('.php');
            spl_autoload($class);
        }
    }    
    

      /**
     * Autoload Model class
     *
     * @param  string $class
     * @return object
     */
     
    public function load_models($model)
    {
        if ($model) {
            set_include_path($this->modelDirectoryPath);
            spl_autoload_extensions('.php');
            spl_autoload($class);
        }
    }    
    
      /**
     * Autoload Library class
     *
     * @param  string $class
     * @return object
     */
     
    public function load_library($library)
    {
        if ($library) {
            set_include_path($this->libraryDirectoryPath);
            spl_autoload_extensions('.php');
            spl_autoload($class);
        }
    }
    

    
 }
 
 ?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.spl-autoload.php)

**[To root](/README.md)**
# Autoloading Classes



You should not have to use require_once inside the autoloader, as if the class is not found it wouldn&apos;t be trying to look for it by using the autoloader. <br><br>Just use require(), which will be better on performance as well as it does not have to check if it is unique.  

#

You don&apos;t need exceptions to figure out if a class can be autoloaded. This is much simpler.<br><br>

```
<?php
//Define autoloader
function __autoload($className) {
      if (file_exists($className . &apos;.php&apos;)) {
          require_once $className . &apos;.php&apos;;
          return true;
      }
      return false;
}

function canClassBeAutloaded($className) {
      return class_exists($className);
}
?>
```
  

#

This is my autoloader for my PSR-4 clases. I prefer to use composer&apos;s autoloader, but this works for legacy projects that can&apos;t use composer.<br><br>

```
<?php<br>/**<br> * Simple autoloader, so we don&apos;t need Composer just for this.<br> */<br>class Autoloader<br>{<br>    public static function register()<br>    {<br>        spl_autoload_register(function ($class) {<br>            $file = str_replace(&apos;\\&apos;, DIRECTORY_SEPARATOR, $class).&apos;.php&apos;;<br>            if (file_exists($file)) {<br>                require $file;<br>                return true;<br>            }<br>            return false;<br>        });<br>    }<br>}<br>Autoloader::register();  

#

Andrew: 03-Nov-2006 12:26<br><br>That seems a bit messy to me, this is a bit neater:<br>

```
<?php<br>    function __autoload($class_name) <br>    {<br>        //class directories<br>        $directorys = array(<br>            &apos;classes/&apos;,<br>            &apos;classes/otherclasses/&apos;,<br>            &apos;classes2/&apos;,<br>            &apos;module1/classes/&apos;<br>        );<br>        <br>        //for each directory<br>        foreach($directorys as $directory)<br>        {<br>            //see if the file exsists<br>            if(file_exists($directory.$class_name . &apos;.php&apos;))<br>            {<br>                require_once($directory.$class_name . &apos;.php&apos;);<br>                //only require the class once, so quit after to save effort (if you got more, then name them something else <br>                return;<br>            }            <br>        }<br>    }  

#

Before you start using __autload, remember that it holds no scope/namespace. This means that if you are depending on third party applications and they have an autoload function defined and so do you, your application will error.<br><br>To remedy this, everyone should look at the spl_autoload functions, eg: spl_autoload_register. This function allows more than one custom functions to be called through the default spl_autoload (default __autoload) handler.  

#

I&apos;m sure this is needed by more than me.<br><br>My objective was to allow __autoload() to be easily extended in complex systems/frameworks where specific libraries etc may need loading differently but you don&apos;t want to hard-code little adjustments into your working __autoload() to allow this to happen.<br><br>Using a ServiceLocator object with some static methods and properties to allow loosely coupled locators to be attached to it you can swap/change and add to the functionality of your __autoload() at runtime.<br><br>The core stuff:<br>

```
<?php

/**
 * Defines the methods any actual locators must implement
 * @package ServiceLocator
 * @author Chris Corbyn
 */
interface Locator
{
    /**
     * Inform of whether or not the given class can be found
     * @param string class
     * @return bool
     */
    public function canLocate($class);
    /**
     * Get the path to the class
     * @param string class
     * @return string
     */
    public function getPath($class);
}

/**
 * The main service locator.
 * Uses loosely coupled locators in order to operate
 * @package ServiceLocator
 * @author Chris Corbyn
 */
class ServiceLocator
{
    /**
     * Contains any attached service locators
     * @var array Locator
     */
    protected static $locators = array();
    
    /**
     * Attach a new type of locator
     * @param object Locator
     * @param string key
     */
    public static function attachLocator(Locator $locator, $key)
    {
        self::$locators[$key] = $locator;
    }
    /**
     * Remove a locator that&apos;s been added
     * @param string key
     * @return bool
     */
    public static function dropLocator($key)
    {
        if (self::isActiveLocator($key))
        {
            unset(self::$locators[$key]);
            return true;
        }
        else return false;
    }
    /**
     * Check if a locator is currently loaded
     * @param string key
     * @return bool
     */
    public static function isActiveLocator($key)
    {
        return array_key_exists($key, self::$locators);
    }
    /**
     * Load in the required service by asking all service locators
     * @param string class
     */
    public function load($class)
    {
        foreach (self::$locators as $key =&gt; $obj)
        {
            if ($obj-&gt;canLocate($class))
            {
                require_once $obj-&gt;getPath($class);
                if (class_exists($class)) return;
            }
        }
    }
}

/**
 * PHPs default __autload
 * Grabs an instance of ServiceLocator then runs it
 * @package ServiceLocator
 * @author Chris Corbyn
 * @param string class
 */
function __autoload($class)
{
    $locator = new ServiceLocator();
    $locator-&gt;load($class);
}

?>
```


An example Use Case:


```
<?php

require &apos;ServiceLocator.php&apos;;

//Define some sort of service locator to attach...
class PearLocator implements Locator
{
    protected $base = &apos;.&apos;;
    
    public function __construct($directory=&apos;.&apos;)
    {
        $this-&gt;base = (string) $directory;
    }
    
    public function canLocate($class)
    {
        $path = $this-&gt;getPath($class);
        if (file_exists($path)) return true;
        else return false;
    }
    
    public function getPath($class)
    {
        return $this-&gt;base . &apos;/&apos; . str_replace(&apos;_&apos;, &apos;/&apos;, $class) . &apos;.php&apos;;
    }
}

// ... attach it ...
ServiceLocator::attachLocator(new PearLocator(), &apos;PEAR&apos;);

// ... and code away....
$foo = new Foo_Test();

?>
```
  

#

Or you can use this, without using any "require/include":<br><br>

```
<?php
class autoloader {

    public static $loader;

    public static function init()
    {
        if (self::$loader == NULL)
            self::$loader = new self();

        return self::$loader;
    }

    public function __construct()
    {
        spl_autoload_register(array($this,&apos;model&apos;));
        spl_autoload_register(array($this,&apos;helper&apos;));
        spl_autoload_register(array($this,&apos;controller&apos;));
        spl_autoload_register(array($this,&apos;library&apos;));
    }

    public function library($class)
    {
        set_include_path(get_include_path().PATH_SEPARATOR.&apos;/lib/&apos;);
        spl_autoload_extensions(&apos;.library.php&apos;);
        spl_autoload($class);
    }

    public function controller($class)
    {
        $class = preg_replace(&apos;/_controller$/ui&apos;,&apos;&apos;,$class);
        
        set_include_path(get_include_path().PATH_SEPARATOR.&apos;/controller/&apos;);
        spl_autoload_extensions(&apos;.controller.php&apos;);
        spl_autoload($class);
    }

    public function model($class)
    {
        $class = preg_replace(&apos;/_model$/ui&apos;,&apos;&apos;,$class);
        
        set_include_path(get_include_path().PATH_SEPARATOR.&apos;/model/&apos;);
        spl_autoload_extensions(&apos;.model.php&apos;);
        spl_autoload($class);
    }

    public function helper($class)
    {
        $class = preg_replace(&apos;/_helper$/ui&apos;,&apos;&apos;,$class);

        set_include_path(get_include_path().PATH_SEPARATOR.&apos;/helper/&apos;);
        spl_autoload_extensions(&apos;.helper.php&apos;);
        spl_autoload($class);
    }

}

//call
autoloader::init();
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.autoload.php)

**[To root](/README.md)**
# Autoloading Classes





You should not have to use require_once inside the autoloader, as if the class is not found it wouldn&apos;t be trying to look for it by using the autoloader. 

Just use require(), which will be better on performance as well as it does not have to check if it is unique.

  

#



You don&apos;t need exceptions to figure out if a class can be autoloaded. This is much simpler.





```
<?php

//Define autoloader

function __autoload($className) {

&#xA0; &#xA0; &#xA0; if (file_exists($className . &apos;.php&apos;)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; require_once $className . &apos;.php&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return true;

&#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; return false;

}



function canClassBeAutloaded($className) {

&#xA0; &#xA0; &#xA0; return class_exists($className);

}

?>
```



  

#



This is my autoloader for my PSR-4 clases. I prefer to use composer&apos;s autoloader, but this works for legacy projects that can&apos;t use composer.



```
<?php
/**
 * Simple autoloader, so we don&apos;t need Composer just for this.
 */
class Autoloader
{
&#xA0; &#xA0; public static function register()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_register(function ($class) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $file = str_replace(&apos;\\&apos;, DIRECTORY_SEPARATOR, $class).&apos;.php&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (file_exists($file)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; require $file;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; });
&#xA0; &#xA0; }
}
Autoloader::register();


  

#



Andrew: 03-Nov-2006 12:26

That seems a bit messy to me, this is a bit neater:


```
<?php
&#xA0; &#xA0; function __autoload($class_name) 
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; //class directories
&#xA0; &#xA0; &#xA0; &#xA0; $directorys = array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;classes/&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;classes/otherclasses/&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;classes2/&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;module1/classes/&apos;
&#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; //for each directory
&#xA0; &#xA0; &#xA0; &#xA0; foreach($directorys as $directory)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //see if the file exsists
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(file_exists($directory.$class_name . &apos;.php&apos;))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; require_once($directory.$class_name . &apos;.php&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //only require the class once, so quit after to save effort (if you got more, then name them something else 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }


  

#



Before you start using __autload, remember that it holds no scope/namespace. This means that if you are depending on third party applications and they have an autoload function defined and so do you, your application will error.

To remedy this, everyone should look at the spl_autoload functions, eg: spl_autoload_register. This function allows more than one custom functions to be called through the default spl_autoload (default __autoload) handler.

  

#



I&apos;m sure this is needed by more than me.

My objective was to allow __autoload() to be easily extended in complex systems/frameworks where specific libraries etc may need loading differently but you don&apos;t want to hard-code little adjustments into your working __autoload() to allow this to happen.

Using a ServiceLocator object with some static methods and properties to allow loosely coupled locators to be attached to it you can swap/change and add to the functionality of your __autoload() at runtime.

The core stuff:


```
<?php

/**
 * Defines the methods any actual locators must implement
 * @package ServiceLocator
 * @author Chris Corbyn
 */
interface Locator
{
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Inform of whether or not the given class can be found
&#xA0; &#xA0;&#xA0; * @param string class
&#xA0; &#xA0;&#xA0; * @return bool
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function canLocate($class);
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Get the path to the class
&#xA0; &#xA0;&#xA0; * @param string class
&#xA0; &#xA0;&#xA0; * @return string
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function getPath($class);
}

/**
 * The main service locator.
 * Uses loosely coupled locators in order to operate
 * @package ServiceLocator
 * @author Chris Corbyn
 */
class ServiceLocator
{
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Contains any attached service locators
&#xA0; &#xA0;&#xA0; * @var array Locator
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; protected static $locators = array();
&#xA0; &#xA0; 
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Attach a new type of locator
&#xA0; &#xA0;&#xA0; * @param object Locator
&#xA0; &#xA0;&#xA0; * @param string key
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public static function attachLocator(Locator $locator, $key)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; self::$locators[$key] = $locator;
&#xA0; &#xA0; }
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Remove a locator that&apos;s been added
&#xA0; &#xA0;&#xA0; * @param string key
&#xA0; &#xA0;&#xA0; * @return bool
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public static function dropLocator($key)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (self::isActiveLocator($key))
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset(self::$locators[$key]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; else return false;
&#xA0; &#xA0; }
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Check if a locator is currently loaded
&#xA0; &#xA0;&#xA0; * @param string key
&#xA0; &#xA0;&#xA0; * @return bool
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public static function isActiveLocator($key)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return array_key_exists($key, self::$locators);
&#xA0; &#xA0; }
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Load in the required service by asking all service locators
&#xA0; &#xA0;&#xA0; * @param string class
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function load($class)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; foreach (self::$locators as $key =&gt; $obj)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($obj-&gt;canLocate($class))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; require_once $obj-&gt;getPath($class);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (class_exists($class)) return;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
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
&#xA0; &#xA0; $locator = new ServiceLocator();
&#xA0; &#xA0; $locator-&gt;load($class);
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
&#xA0; &#xA0; protected $base = &apos;.&apos;;
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __construct($directory=&apos;.&apos;)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;base = (string) $directory;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function canLocate($class)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $path = $this-&gt;getPath($class);
&#xA0; &#xA0; &#xA0; &#xA0; if (file_exists($path)) return true;
&#xA0; &#xA0; &#xA0; &#xA0; else return false;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function getPath($class)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;base . &apos;/&apos; . str_replace(&apos;_&apos;, &apos;/&apos;, $class) . &apos;.php&apos;;
&#xA0; &#xA0; }
}

// ... attach it ...
ServiceLocator::attachLocator(new PearLocator(), &apos;PEAR&apos;);

// ... and code away....
$foo = new Foo_Test();

?>
```



  

#



Or you can use this, without using any &quot;require/include&quot;:



```
<?php
class autoloader {

&#xA0; &#xA0; public static $loader;

&#xA0; &#xA0; public static function init()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (self::$loader == NULL)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$loader = new self();

&#xA0; &#xA0; &#xA0; &#xA0; return self::$loader;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function __construct()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_register(array($this,&apos;model&apos;));
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_register(array($this,&apos;helper&apos;));
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_register(array($this,&apos;controller&apos;));
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_register(array($this,&apos;library&apos;));
&#xA0; &#xA0; }

&#xA0; &#xA0; public function library($class)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; set_include_path(get_include_path().PATH_SEPARATOR.&apos;/lib/&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_extensions(&apos;.library.php&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload($class);
&#xA0; &#xA0; }

&#xA0; &#xA0; public function controller($class)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $class = preg_replace(&apos;/_controller$/ui&apos;,&apos;&apos;,$class);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; set_include_path(get_include_path().PATH_SEPARATOR.&apos;/controller/&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_extensions(&apos;.controller.php&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload($class);
&#xA0; &#xA0; }

&#xA0; &#xA0; public function model($class)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $class = preg_replace(&apos;/_model$/ui&apos;,&apos;&apos;,$class);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; set_include_path(get_include_path().PATH_SEPARATOR.&apos;/model/&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_extensions(&apos;.model.php&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload($class);
&#xA0; &#xA0; }

&#xA0; &#xA0; public function helper($class)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $class = preg_replace(&apos;/_helper$/ui&apos;,&apos;&apos;,$class);

&#xA0; &#xA0; &#xA0; &#xA0; set_include_path(get_include_path().PATH_SEPARATOR.&apos;/helper/&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_extensions(&apos;.helper.php&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload($class);
&#xA0; &#xA0; }

}

//call
autoloader::init();
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.autoload.php)

**[To root](/README.md)**
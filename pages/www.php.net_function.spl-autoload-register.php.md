# spl_autoload_register



Good news for PHP 5.3 users with namespaced classes:<br><br>When you create a subfolder structure matching the namespaces of the containing classes, you will never even have to define an autoloader.<br><br>

```
<?php
    spl_autoload_extensions(".php"); // comma-separated list
    spl_autoload_register();
?>
```


It is recommended to use only one extension for all classes. PHP (more exactly spl_autoload) does the rest for you and is even quicker than a semantically equal self-defined autoload function like this one:



```
<?php
    function my_autoload ($pClassName) {
        include(__DIR__ . "/" . $pClassName . ".php");
    }
    spl_autoload_register("my_autoload");
?>
```
<br><br>I compared them with the following setting: There are 10 folders, each having 10 subfolders, each having 10 subfolders, each containing 10 classes.<br><br>To load and instantiate these 1000 classes (parameterless no-action constructor), the user-definded autoload function approach took 50ms longer in average than the spl_autoload function in a series of 10 command-line calls for each approach.<br><br>I made this benchmark to ensure that I don&apos;t recommend something that could be called "nice, but slow" later.<br><br>Best regards,  

#

When switching from using __autoload() to using spl_autoload_register keep in mind that deserialization of the session can trigger class lookups.<br><br>This works as expected: <br>

```
<?php
session_start();
function __autoload($class) {
...
}
?>
```


This will result in "__PHP_Incomplete_Class_Name" errors when using classes deserialized from the session.


```
<?php
session_start();
function customAutoloader($class) {
...
}
spl_autoload_register("customAutoloader");
?>
```


So you need to make sure the spl_autoload_register is done BEFORE session_start() is called.

CORRECT:


```
<?php
function customAutoloader($class) {
...
}
spl_autoload_register("customAutoloader");
session_start();
?>
```
  

#

When using spl_autoload_register() with class methods, it might seem that it can use only public methods, though it can use private/protected methods as well, if registered from inside the class:<br>

```
<?php

    class ClassAutoloader {
        public function __construct() {
            spl_autoload_register(array($this, 'loader'));
        }
        private function loader($className) {
            echo 'Trying to load ', $className, ' via ', __METHOD__, "()\n";
            include $className . '.php';
        }
    }

    $autoloader = new ClassAutoloader();

    $obj = new Class1();
    $obj = new Class2();

?>
```
<br><br>Output:<br>--------<br>Trying to load Class1 via ClassAutoloader::loader()<br>Class1::__construct()<br>Trying to load Class2 via ClassAutoloader::loader()<br>Class2::__construct()  

#

If your autoload function is a class method, you can call spl_autoload_register with an array specifying the class and the method to run.<br><br>* You can use a static method :<br>

```
<?php

class MyClass {
  public static function autoload($className) {
    // ...
  }
}

spl_autoload_register(array('MyClass', 'autoload'));
?>
```


* Or you can use an instance :


```
<?php
class MyClass {
  public function autoload($className) {
    // ...
  }
}

$instance = new MyClass();
spl_autoload_register(array($instance, 'autoload'));
?>
```
  

#

Think twice about throwing an exception from a registered autoloader.<br><br>If you have multiple autoloaders registered, and one (or more) throws an exception before a later autoloader loads the class, stacked exceptions are thrown (and must be caught) even though the class was loaded successfully.  

#

What I said here previously is only true on Windows. The built-in default autoloader that is registered when you call spl_autoload_register() without any arguments simply adds the qualified class name plus the registered file extension (.php) to each of the include paths and tries to include that file.<br><br>Example (on Windows):<br><br>include paths:<br>- "."<br>- "d:/projects/phplib"<br><br>qualified class name to load:<br>network\http\rest\Resource<br><br>Here&apos;s what happens:<br><br>PHP tries to load<br>&apos;.\\network\\http\\rest\\Resource.php&apos;<br>-&gt; file not found<br><br>PHP tries to load<br>&apos;d:/projects/phplib\\network\\http\\rest\\Resource.php&apos;<br>-&gt; file found and included<br><br>Note the slashes and backslashes in the file path. On Windows this works perfectly, but on a Linux machine, the backslashes won&apos;t work and additionally the file names are case-sensitive.<br><br>That&apos;s why on Linux the quick-and-easy way would be to convert these qualified class names to slashes and to lowercase and pass them to the built-in autoloader like so:<br><br>

```
<?php
spl_autoload_register(
  function ($pClassName) {
    spl_autoload(strtolower(str_replace("\\", "/", $pClassName)));
  }
);
?>
```
<br><br>But this means, you have to save all your classes with lowercase file names. Otherwise, if you omit the strtolower call, you have to use the class names exactly as specified by the file name, which can be annoying for class names that are defined with non-straightforward case like e. g. XMLHttpRequest.<br><br>I prefer the lowercase approach, because it is easier to use and the file name conversion can be done automatically on deploying.  

#

Be careful using this function on case sensitive file systems.<br><br>

```
<?php
spl_autoload_extensions('.php');
spl_autoload_register();
?>
```
<br><br>I develop on OS X and everything was working fine. But when releasing to my linux server, none of my class files were loading. I had to lowercase all my filenames, because calling a class "DatabaseObject" would try including "databaseobject.php", instead of "DatabaseObject.php"<br><br>I think i&apos;ll go back to using the slower __autoload() function, just so i can keep my class files readable  

#

[Official documentation page](https://www.php.net/manual/en/function.spl-autoload-register.php)

**[To root](/README.md)**
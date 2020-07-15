# __autoload



It is highly recommended not to use the __autoload() function any more. Now the spl_autoload_register() function is what you should consider.Sorry for the mistake in line 6 of my previous note. And below is the corrected PHP code.<br>

```
<?php
    if(!function_exists('classAutoLoader')){
        function classAutoLoader($class){
            $class=strtolower($class);
            $classFile=$_SERVER['DOCUMENT_ROOT'].'/include/class/'.$class.'.class.php';
            if(is_file($classFile)&amp;&amp;!class_exists($class)) include $classFile;
        }
    }
    spl_autoload_register('classAutoLoader');
?>
```
  

#

Even I have never been using this function, just a simple example in order to explain it;<br><br>./myClass.php<br>

```
<?php
class myClass {
    public function __construct() {
        echo "myClass init'ed successfuly!!!";
    }
}
?>
```


./index.php


```
<?php
// we've writen this code where we need
function __autoload($classname) {
    $filename = "./". $classname .".php";
    include_once($filename);
}

// we've called a class ***
$obj = new myClass();
?>
```


*** At this line, our "./myClass.php" will be included! This is the magic that we're wondering... And you get this result "myClass init'ed successfuly!!!".

So, if you call a class that named as myClass then a file will be included myClass.php if it exists (if not you get an include error normally). If you call Foo, Foo.php will be included, and so on...

And you don't need some code like this anymore;



```
<?php
include_once("./myClass.php");
include_once("./myFoo.php");
include_once("./myBar.php");

$obj = new myClass();
$foo = new Foo();
$bar = new Bar();
?>
```
<br><br>Your class files will be included "automatically" when you call (init) them without these functions: "include, include_once, require, require_once".  

#

[Official documentation page](https://www.php.net/manual/en/function.autoload.php)

**[To root](/README.md)**
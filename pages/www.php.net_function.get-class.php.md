# get_class



&gt;= 5.5<br><br>::class<br>fully qualified class name, instead of get_class<br><br>

```
<?php
namespace my\library\mvc;

class Dispatcher {}

print Dispatcher::class; // FQN == my\library\mvc\Dispatcher

$disp = new Dispatcher;

print $disp::class; // parse error?>
```
  

---

A lot of people in other comments wanting to get the classname without the namespace. Some weird suggestions of code to do that - not what I would&apos;ve written! So wanted to add my own way.<br><br>

```
<?php
function get_class_name($classname)
{
    if ($pos = strrpos($classname, '\\')) return substr($classname, $pos + 1);
    return $pos;
}
?>
```


Also did some quick benchmarking, and strrpos() was the fastest too. Micro-optimisations = macro optimisations!

39.0954 ms - preg_match()
28.6305 ms - explode() + end()
20.3314 ms - strrpos()

(For reference, here's the debug code used. c() is a benchmarking function that runs each closure run 10,000 times.)



```
<?php
c(
    function($class = 'a\b\C') {
        if (preg_match('/\\\\([\w]+)$/', $class, $matches)) return $matches[1];
        return $class;
    },
    function($class = 'a\b\C') {
        $bits = explode('\\', $class);
        return end($bits);
    },
    function($class = 'a\b\C') {
        if ($pos = strrpos($class, '\\')) return substr($class, $pos + 1);
        return $pos;
    }
);
?>
```
  

---

People seem to mix up what __METHOD__, get_class($obj) and get_class() do, related to class inheritance.<br><br>Here&apos;s a good example that should fix that for ever:<br><br>

```
<?php

class Foo {
 function doMethod(){
  echo __METHOD__ . "\n";
 }
 function doGetClassThis(){
  echo get_class($this).'::doThat' . "\n";
 }
 function doGetClass(){
  echo get_class().'::doThat' . "\n";
 }
}

class Bar extends Foo {

}

class Quux extends Bar {
 function doMethod(){
  echo __METHOD__ . "\n";
 }
 function doGetClassThis(){
  echo get_class($this).'::doThat' . "\n";
 }
 function doGetClass(){
  echo get_class().'::doThat' . "\n";
 }
}

$foo = new Foo();
$bar = new Bar();
$quux = new Quux();

echo "\n--doMethod--\n";

$foo->doMethod();
$bar->doMethod();
$quux->doMethod();

echo "\n--doGetClassThis--\n";

$foo->doGetClassThis();
$bar->doGetClassThis();
$quux->doGetClassThis();

echo "\n--doGetClass--\n";

$foo->doGetClass();
$bar->doGetClass();
$quux->doGetClass();

?>
```
<br><br>OUTPUT:<br><br>--doMethod--<br>Foo::doMethod<br>Foo::doMethod<br>Quux::doMethod<br><br>--doGetClassThis--<br>Foo::doThat<br>Bar::doThat<br>Quux::doThat<br><br>--doGetClass--<br>Foo::doThat<br>Foo::doThat<br>Quux::doThat  

---

[Official documentation page](https://www.php.net/manual/en/function.get-class.php)

**[To root](/README.md)**
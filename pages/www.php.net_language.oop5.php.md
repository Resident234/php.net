# Classes and Objects



PHP 5 is very very flexible in accessing member variables and member functions. These access methods maybe look unusual and unnecessary at first glance; but they are very useful sometimes; specially when you work with SimpleXML classes and objects. I have posted a similar comment in SimpleXML function reference section, but this one is more comprehensive.<br><br>I use the following class as reference for all examples:<br><br>

```
<?php
class Foo {
    public $aMemberVar = &apos;aMemberVar Member Variable&apos;;
    public $aFuncName = &apos;aMemberFunc&apos;;
    
    
    function aMemberFunc() {
        print &apos;Inside `aMemberFunc()`&apos;;
    }
}

$foo = new Foo;
?>
```


You can access member variables in an object using another variable as name:



```
<?php
$element = &apos;aMemberVar&apos;;
print $foo-&gt;$element; // prints "aMemberVar Member Variable"
?>
```


or use functions:



```
<?php
function getVarName()
{ return &apos;aMemberVar&apos;; }

print $foo-&gt;{getVarName()}; // prints "aMemberVar Member Variable"
?>
```


Important Note: You must surround function name with { and } or PHP would think you are calling a member function of object "foo".

you can use a constant or literal as well:



```
<?php
define(MY_CONSTANT, &apos;aMemberVar&apos;);
print $foo-&gt;{MY_CONSTANT}; // Prints "aMemberVar Member Variable"
print $foo-&gt;{&apos;aMemberVar&apos;}; // Prints "aMemberVar Member Variable"
?>
```


You can use members of other objects as well:



```
<?php
print $foo-&gt;{$otherObj-&gt;var};
print $foo-&gt;{$otherObj-&gt;func()};
?>
```


You can use mathods above to access member functions as well:



```
<?php
print $foo-&gt;{&apos;aMemberFunc&apos;}(); // Prints "Inside `aMemberFunc()`"
print $foo-&gt;{$foo-&gt;aFuncName}(); // Prints "Inside `aMemberFunc()`"
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.php)

**[To root](/README.md)**
# method_exists



As noted [elsewhere] method_exists() does not care about the existence of __call(), whereas is_callable() does:<br><br>

```
<?php
class Test {
  public function explicit(  ) {
      // ...
  }
   
  public function __call( $meth, $args ) {
      // ...
  }
}

$Tester = new Test();

var_export(method_exists($Tester, 'anything')); // false
var_export(is_callable(array($Tester, 'anything'))); // true
?>
```
  

#

A couple of the older comments (specifically "jp at function dot fi" and "spam at majiclab dot com") state that is_callable() does not respect a methods visibility when checked outside of that class. ie. That private/protected methods are seen as callable when tested publicly. However, this was a bug (#29210) in early versions of PHP 5 and was fixed (according to the changelog) in PHP 5.0.5 (and/or PHP 5.1.0).<br><br>Bug #29210 - Function: is_callable - no support for private and protected classes<br>http://bugs.php.net/29210<br><br>Changelog - Fixed bug #29210 (Function: is_callable - no support for private and protected classes). (Dmitry)<br>http://php.net/ChangeLog-5.php#5.1.0  

#

This function is case-insensitive (as is PHP) and here is the proof:<br>

```
<?php
class A {
    public function FUNC() { echo '*****'; }
}

$a = new A();
$a->func(); // *****
var_dump(method_exists($a, 'func')); // bool(true)
?>
```
  

#

Just to mention it: both method_exists() and is_callable() return true for inherited methods:<br><br>

```
<?php
class ParentClass {

   function doParent() { }
}

class ChildClass extends ParentClass { }

$p = new ParentClass();
$c = new ChildClass();

// all return true
var_dump(method_exists($p, 'doParent'));
var_dump(method_exists($c, 'doParent'));

var_dump(is_callable(array($p, 'doParent')));
var_dump(is_callable(array($c, 'doParent')));
?>
```
  

#

if you want to check for a method "inside" of a class use:<br><br>method_exists($this, &apos;function_name&apos;)<br><br>i was a bit confused &apos;cause i thought i&apos;m only able to check for a method when i got an object like $object_name = new class_name() with: <br><br>method_exists($object_name, &apos;method_name&apos;)<br><br>small example for those who didn&apos;t understood what i mean ( maybe caused by bad english :) ):<br><br>

```
<?php

class a {

    function a() {
        
        if(method_exists($this, 'test'))
            echo 'a::test() exists!';
        else
            echo 'a::test() doesn\'t exists';

    }

    
    function test() {
        
        return true;
    
    }

}

$b = new a();

?>
```
<br><br>the output will be: a::test() exists!<br><br>maybe this will help someone  

#

[Official documentation page](https://www.php.net/manual/en/function.method-exists.php)

**[To root](/README.md)**
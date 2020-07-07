# Scope Resolution Operator (::)





A class constant, class property (static), and class function (static) can all share the same name and be accessed using the double-colon.



```
<?php

class A {

&#xA0; &#xA0; public static $B = &apos;1&apos;; # Static class variable.

&#xA0; &#xA0; const B = &apos;2&apos;; # Class constant.
&#xA0; &#xA0; 
&#xA0; &#xA0; public static function B() { # Static class function.
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;3&apos;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
}

echo A::$B . A::B . A::B(); # Outputs: 123
?>
```



  

#



In PHP, you use the self keyword to access static properties and methods.

The problem is that you can replace $this-&gt;method() with self::method() anywhere, regardless if method() is declared static or not. So which one should you use?

Consider this code:

class ParentClass {
&#xA0; &#xA0; function test() {
&#xA0; &#xA0; &#xA0; &#xA0; self::who();&#xA0; &#xA0; // will output &apos;parent&apos;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;who();&#xA0; &#xA0; // will output &apos;child&apos;
&#xA0; &#xA0; }

&#xA0; &#xA0; function who() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;parent&apos;;
&#xA0; &#xA0; }
}

class ChildClass extends ParentClass {
&#xA0; &#xA0; function who() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;child&apos;;
&#xA0; &#xA0; }
}

$obj = new ChildClass();
$obj-&gt;test();
In this example, self::who() will always output &#x2018;parent&#x2019;, while $this-&gt;who() will depend on what class the object has.

Now we can see that self refers to the class in which it is called, while $this refers to the class of the current object.

So, you should use self only when $this is not available, or when you don&#x2019;t want to allow descendant classes to overwrite the current method.

  

#



It seems as though you can use more than the class name to reference the static variables, constants, and static functions of a class definition from outside that class using the :: . The language appears to allow you to use the object itself. 

For example:
class horse 
{
&#xA0;&#xA0; static $props = {&apos;order&apos;=&gt;&apos;mammal&apos;};
}
$animal = new horse();
echo $animal::$props[&apos;order&apos;];

// yields &apos;mammal&apos;

This does not appear to be documented but I see it as an important convenience in the language. I would like to see it documented and supported as valid. 

If it weren&apos;t supported officially, the alternative would seem to be messy, something like this:

$animalClass = get_class($animal);
echo $animalClass::$props[&apos;order&apos;];

  

#



Just found out that using the class name may also work to call similar function of anchestor class.



```
<?php

class Anchestor {
&#xA0;&#xA0; 
&#xA0;&#xA0; public $Prefix = &apos;&apos;;

&#xA0;&#xA0; private $_string =&#xA0; &apos;Bar&apos;;
&#xA0; &#xA0; public function Foo() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;Prefix.$this-&gt;_string;
&#xA0; &#xA0; }
}

class MyParent extends Anchestor {
&#xA0; &#xA0; public function Foo() {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $this-&gt;Prefix = null;
&#xA0; &#xA0; &#xA0; &#xA0; return parent::Foo().&apos;Baz&apos;;
&#xA0; &#xA0; }
}

class Child extends MyParent {
&#xA0; &#xA0; public function Foo() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;Prefix = &apos;Foo&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; return Anchestor::Foo();
&#xA0; &#xA0; }
}

$c = new Child();
echo $c-&gt;Foo(); //return FooBar, because Prefix, as in Anchestor::Foo()

?>
```


The Child class calls at Anchestor::Foo(), and therefore MyParent::Foo() is never run.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.paamayim-nekudotayim.php)

**[To root](/README.md)**
# Visibility





INSIDE CODE and OUTSIDE CODE



```
<?php

class Item
{
&#xA0; /**
&#xA0;&#xA0; * This is INSIDE CODE because it is written INSIDE the class.
&#xA0;&#xA0; */
&#xA0; public $label;
&#xA0; public $price;
}

/**
 * This is OUTSIDE CODE because it is written OUTSIDE the class.
 */
$item = new Item();
$item-&gt;label = &apos;Ink-Jet Tatoo Gun&apos;;
$item-&gt;price = 49.99;

?>
```


Ok, that&apos;s simple enough... I got it inside and out. The big problem with this is that the Item class is COMPLETELY IGNORANT in the following ways:
* It REQUIRES OUTSIDE CODE to do all the work AND to know what and how to do it -- huge mistake.
* OUTSIDE CODE can cast Item properties to any other PHP types (booleans, integers, floats, strings, arrays, and objects etc.) -- another huge mistake.

Note: we did it correctly above, but what if someone made an array for $price? FYI: PHP has no clue what we mean by an Item, especially by the terms of our class definition above. To PHP, our Item is something with two properties (mutable in every way) and that&apos;s it. As far as PHP is concerned, we can pack the entire set of Britannica Encyclopedias into the price slot. When that happens, we no longer have what we expect an Item to be.

INSIDE CODE should keep the integrity of the object. For example, our class definition should keep $label a string and $price a float -- which means only strings can come IN and OUT of the class for label, and only floats can come IN and OUT of the class for price.



```
<?php

class Item
{
&#xA0; /**
&#xA0;&#xA0; * Here&apos;s the new INSIDE CODE and the Rules to follow:
&#xA0;&#xA0; *
&#xA0;&#xA0; * 1. STOP ACCESS to properties via $item-&gt;label and $item-&gt;price,
&#xA0;&#xA0; *&#xA0; &#xA0; by using the protected keyword.
&#xA0;&#xA0; * 2. FORCE the use of public functions.
&#xA0;&#xA0; * 3. ONLY strings are allowed IN &amp; OUT of this class for $label
&#xA0;&#xA0; *&#xA0; &#xA0; via the getLabel and setLabel functions.
&#xA0;&#xA0; * 4. ONLY floats are allowed IN &amp; OUT of this class for $price
&#xA0;&#xA0; *&#xA0; &#xA0; via the getPrice and setPrice functions.
&#xA0;&#xA0; */

&#xA0; protected $label = &apos;Unknown Item&apos;; // Rule 1 - protected.
&#xA0; protected $price = 0.0;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Rule 1 - protected.

&#xA0; public function getLabel() {&#xA0; &#xA0; &#xA0;&#xA0; // Rule 2 - public function.
&#xA0; &#xA0; return $this-&gt;label;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // Rule 3 - string OUT for $label.
&#xA0; }

&#xA0; public function getPrice() {&#xA0; &#xA0; &#xA0;&#xA0; // Rule 2 - public function.&#xA0; &#xA0; 
&#xA0; &#xA0; return $this-&gt;price;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // Rule 4 - float OUT for $price.
&#xA0; }

&#xA0; public function setLabel($label)&#xA0;&#xA0; // Rule 2 - public function.
&#xA0; {
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Make sure $label is a PHP string that can be used in a SORTING
&#xA0; &#xA0;&#xA0; * alogorithm, NOT a boolean, number, array, or object that can&apos;t
&#xA0; &#xA0;&#xA0; * properly sort -- AND to make sure that the getLabel() function
&#xA0; &#xA0;&#xA0; * ALWAYS returns a genuine PHP string.
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * Using a RegExp would improve this function, however, the main
&#xA0; &#xA0;&#xA0; * point is the one made above.
&#xA0; &#xA0;&#xA0; */

&#xA0; &#xA0; if(is_string($label))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $this-&gt;label = (string)$label; // Rule 3 - string IN for $label.
&#xA0; &#xA0; }
&#xA0; }

&#xA0; public function setPrice($price)&#xA0;&#xA0; // Rule 2 - public function.
&#xA0; {
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Make sure $price is a PHP float so that it can be used in a
&#xA0; &#xA0;&#xA0; * NUMERICAL CALCULATION. Do not accept boolean, string, array or
&#xA0; &#xA0;&#xA0; * some other object that can&apos;t be included in a simple calculation.
&#xA0; &#xA0;&#xA0; * This will ensure that the getPrice() function ALWAYS returns an
&#xA0; &#xA0;&#xA0; * authentic, genuine, full-flavored PHP number and nothing but.
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * Checking for positive values may improve this function,
&#xA0; &#xA0;&#xA0; * however, the main point is the one made above.
&#xA0; &#xA0;&#xA0; */

&#xA0; &#xA0; if(is_numeric($price))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $this-&gt;price = (float)$price; // Rule 4 - float IN for $price.
&#xA0; &#xA0; }
&#xA0; }
}

?>
```


Now there is nothing OUTSIDE CODE can do to obscure the INSIDES of an Item. In other words, every instance of Item will always look and behave like any other Item complete with a label and a price, AND you can group them together and they will interact without disruption. Even though there is room for improvement, the basics are there, and PHP will not hassle you... which means you can keep your hair!

  

#



If you have problems with overriding private methods in extended classes, read this:)



The manual says that &quot;Private limits visibility only to the class that defines the item&quot;. That means extended children classes do not see the private methods of parent class and vice versa also. 



As a result, parents and children can have different implementations of the &quot;same&quot; private methods, depending on where you call them (e.g. parent or child class instance). Why? Because private methods are visible only for the class that defines them and the child class does not see the parent&apos;s private methods. If the child doesn&apos;t see the parent&apos;s private methods, the child can&apos;t override them. Scopes are different. In other words -- each class has a private set of private variables that no-one else has access to. 



A sample demonstrating the percularities of private methods when extending classes:





```
<?php

abstract class base {

&#xA0; &#xA0; public function inherited() {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;overridden();

&#xA0; &#xA0; }

&#xA0; &#xA0; private function overridden() {

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;base&apos;;

&#xA0; &#xA0; }

}



class child extends base {

&#xA0; &#xA0; private function overridden() {

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;child&apos;;

&#xA0; &#xA0; }

}



$test = new child();

$test-&gt;inherited();

?>
```




Output will be &quot;base&quot;.



If you want the inherited methods to use overridden functionality in extended classes but public sounds too loose, use protected. That&apos;s what it is for:)



A sample that works as intended:





```
<?php

abstract class base {

&#xA0; &#xA0; public function inherited() {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;overridden();

&#xA0; &#xA0; }

&#xA0; &#xA0; protected function overridden() {

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;base&apos;;

&#xA0; &#xA0; }

}



class child extends base {

&#xA0; &#xA0; protected function overridden() {

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;child&apos;;

&#xA0; &#xA0; }

}



$test = new child();

$test-&gt;inherited();

?>
```


Output will be &quot;child&quot;.

  

#



Just a quick note that it&apos;s possible to declare visibility for multiple properties at the same time, by separating them by commas.

eg:



```
<?php
class a
{
&#xA0; &#xA0; protected $a, $b;

&#xA0; &#xA0; public $c, $d;

&#xA0; &#xA0; private $e, $f;
}
?>
```



  

#



A class A static public function can access to class A private function :



```
<?php
class A {
&#xA0; &#xA0; private function foo()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; print(&quot;bar&quot;);
&#xA0; &#xA0; }

&#xA0; &#xA0; static public function bar($a)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $a-&gt;foo();
&#xA0; &#xA0; }
}

$a = new A();

A::bar($a);
?>
```


It&apos;s working.

  

#



Beware: Visibility works on a per-class-base and does not prevent instances of the same class accessing each others properties!



```
<?php
class Foo
{
&#xA0; &#xA0; private $bar;

&#xA0; &#xA0; public function debugBar(Foo $object)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // this does NOT violate visibility although $bar is private
&#xA0; &#xA0; &#xA0; &#xA0; echo $object-&gt;bar, &quot;\n&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function setBar($value)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // Neccessary method, for $bar is invisible outside the class
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;bar = $value;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function setForeignBar(Foo $object, $value)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // this does NOT violate visibility!
&#xA0; &#xA0; &#xA0; &#xA0; $object-&gt;bar = $value;
&#xA0; &#xA0; }
}

$a = new Foo();
$b = new Foo();
$a-&gt;setBar(1);
$b-&gt;setBar(2);
$a-&gt;debugBar($b);&#xA0; &#xA0; &#xA0; &#xA0; // 2
$b-&gt;debugBar($a);&#xA0; &#xA0; &#xA0; &#xA0; // 1
$a-&gt;setForeignBar($b, 3);
$b-&gt;setForeignBar($a, 4);
$a-&gt;debugBar($b);&#xA0; &#xA0; &#xA0; &#xA0; // 3
$b-&gt;debugBar($a);&#xA0; &#xA0; &#xA0; &#xA0; // 4
?>
```



  

#



Please note that protected methods are also available from sibling classes as long as the method is declared in the common parent. This may also be an abstract method.
 
In the below example Bar knows about the existence of _test() in Foo because they inherited this method from the same parent. It does not matter that it was abstract in the parent.



```
<?php
abstract class Base {
&#xA0; &#xA0; abstract protected function _test();
}
 
class Bar extends Base {
&#xA0; &#xA0; 
&#xA0; &#xA0; protected function _test() { }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function TestFoo() {
&#xA0; &#xA0; &#xA0; &#xA0; $c = new Foo();
&#xA0; &#xA0; &#xA0; &#xA0; $c-&gt;_test();
&#xA0; &#xA0; }
}
 
class Foo extends Base {
&#xA0; &#xA0; protected function _test() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Foo&apos;;
&#xA0; &#xA0; }
}
 
$bar = new Bar();
$bar-&gt;TestFoo(); // result: Foo
?>
```



  

#



This has already been noted here, but there was no clear example. Methods defined in a parent class can NOT access private methods defined in a class which inherits from them. They can access protected, though.

Example:



```
<?php

class ParentClass {

&#xA0; &#xA0; public function execute($method) {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;$method();
&#xA0; &#xA0; }
&#xA0; &#xA0; 
}

class ChildClass extends ParentClass {

&#xA0; &#xA0; private function privateMethod() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;hi, i&apos;m private&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; protected function protectedMethod() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;hi, i&apos;m protected&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
}

$object = new ChildClass();

$object-&gt;execute(&apos;protectedMethod&apos;);

$object-&gt;execute(&apos;privateMethod&apos;);

?>
```


Output:

hi, i&apos;m protected
Fatal error: Call to private method ChildClass::privateMethod() from context &apos;ParentClass&apos; in index.php on line 6

In an early approach this may seem unwanted behaviour but it actually makes sense. Private can only be accessed by the class which defines, neither parent nor children classes.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.visibility.php)

**[To root](/README.md)**
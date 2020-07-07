# Object Inheritance





Here is some clarification about PHP inheritance &#x2013; there is a lot of bad information on the net.&#xA0; PHP does support Multi-level inheritance.&#xA0; (I tested it using version 5.2.9).&#xA0; It does not support multiple inheritance.
 
This means that you cannot have one class extend 2 other classes (see the extends keyword).&#xA0; However, you can have one class extend another, which extends another, and so on. 
 
Example:
 


```
<?php
class A {
&#xA0; &#xA0; &#xA0; &#xA0; // more code here
}
 
class B extends A {
&#xA0; &#xA0; &#xA0; &#xA0; // more code here
}
 
class C extends B {
&#xA0; &#xA0; &#xA0; &#xA0; // more code here
}
 
 
$someObj = new A();&#xA0; // no problems
$someOtherObj = new B(); // no problems
$lastObj = new C(); // still no problems
 
?>
```



  

#



I think the best way for beginners to understand inheritance is through a real example so here is a simple example I can gave to you 



```
<?php

class Person
{
&#xA0; &#xA0; public $name;
&#xA0; &#xA0; protected $age;
&#xA0; &#xA0; private $phone;

&#xA0; &#xA0; public function talk(){
&#xA0; &#xA0; &#xA0; &#xA0; //Do stuff here
&#xA0; &#xA0; }

&#xA0; &#xA0; protected function walk(){
&#xA0; &#xA0; &#xA0; &#xA0; //Do stuff here
&#xA0; &#xA0; }

&#xA0; &#xA0; private function swim(){
&#xA0; &#xA0; &#xA0; &#xA0; //Do stuff here
&#xA0; &#xA0; }
}

class Tom extends Person
{
&#xA0; &#xA0; /*Since Tom class extends Person class this means 
&#xA0; &#xA0; &#xA0; &#xA0; that class Tom is a child class and class person is 
&#xA0; &#xA0; &#xA0; &#xA0; the parent class and child class will inherit all public 
&#xA0; &#xA0; &#xA0; &#xA0; and protected members(properties and methods) from
&#xA0; &#xA0; &#xA0; &#xA0; the parent class*/

&#xA0; &#xA0;&#xA0; /*So class Tom will have these properties and methods*/

&#xA0; &#xA0;&#xA0; //public $name;
&#xA0; &#xA0;&#xA0; //protected $age;
&#xA0; &#xA0;&#xA0; //public function talk(){}
&#xA0; &#xA0;&#xA0; //protected function walk(){}

&#xA0; &#xA0;&#xA0; //but it will not inherit the private members 
&#xA0; &#xA0;&#xA0; //this is all what Object inheritance means
}


  

#



The Idea that multiple inheritence is not supported is correct but with tratits this can be reviewed.

for e.g.
 


```
<?php
trait&#xA0; custom
{
&#xA0; &#xA0;&#xA0; public function hello()
&#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;hello&quot;;
&#xA0; &#xA0;&#xA0; }
}

trait custom2
{
&#xA0; &#xA0; &#xA0;&#xA0; public function hello()
&#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;hello2&quot;;
&#xA0; &#xA0; &#xA0;&#xA0; }
}

class inheritsCustom
{
&#xA0; &#xA0; &#xA0; &#xA0; use custom, custom2
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; custom2::hello insteadof custom;
&#xA0; &#xA0; &#xA0; &#xA0; }
}

$obj = new inheritsCustom();
$obj-&gt;hello();
?>
```



  

#



I was recently extending a PEAR class when I encountered a situation where I wanted to call a constructor two levels up the class hierarchy, ignoring the immediate parent.&#xA0; In such a case, you need to explicitly reference the class name using the :: operator.

Fortunately, just like using the &apos;parent&apos; keyword PHP correctly recognizes that you are calling the function from a protected context inside the object&apos;s class hierarchy.

E.g:



```
<?php
class foo
{
&#xA0; public function something()
&#xA0; {
&#xA0; &#xA0; echo __CLASS__; // foo
&#xA0; &#xA0; var_dump($this);
&#xA0; }
}

class foo_bar extends foo
{
&#xA0; public function something()
&#xA0; {
&#xA0; &#xA0; echo __CLASS__; // foo_bar
&#xA0; &#xA0; var_dump($this);
&#xA0; }
}

class foo_bar_baz extends foo_bar
{
&#xA0; public function something()
&#xA0; {
&#xA0; &#xA0; echo __CLASS__; // foo_bar_baz
&#xA0; &#xA0; var_dump($this);
&#xA0; }

&#xA0; public function call()
&#xA0; {
&#xA0; &#xA0; echo self::something(); // self
&#xA0; &#xA0; echo parent::something(); // parent
&#xA0; &#xA0; echo foo::something(); // grandparent
&#xA0; }
}

error_reporting(-1);

$obj = new foo_bar_baz();
$obj-&gt;call();

// Output similar to:
// foo_bar_baz
// object(foo_bar_baz)[1]
// foo_bar
// object(foo_bar_baz)[1]
// foo
// object(foo_bar_baz)[1]

?>
```



  

#



You can force a class to be strictly an inheritable class by using the &quot;abstract&quot; keyword. When you define a class with abstract, any attempt to instantiate a separate instance of it will result in a fatal error. This is useful for situations like a base class where it would be inherited by multiple child classes yet you want to restrict the ability to instantiate it by itself.

Example........



```
<?php

abstract class Cheese
{
&#xA0; &#xA0; &#xA0; //can ONLY be inherited by another class
}

class Cheddar extends Cheese
{
}

$dinner = new Cheese; //fatal error
$lunch = new Cheddar; //works!

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.inheritance.php)

**[To root](/README.md)**
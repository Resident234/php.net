# Traits





Unlike inheritance; if a trait has static properties, each class using that trait has independent instances of those properties.

Example using parent class:


```
<?php
class TestClass {
&#xA0; &#xA0; public static $_bar;
}
class Foo1 extends TestClass { }
class Foo2 extends TestClass { }
Foo1::$_bar = &apos;Hello&apos;;
Foo2::$_bar = &apos;World&apos;;
echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar; // Prints: World World
?>
```


Example using trait:


```
<?php
trait TestTrait {
&#xA0; &#xA0; public static $_bar;
}
class Foo1 {
&#xA0; &#xA0; use TestTrait;
}
class Foo2 {
&#xA0; &#xA0; use TestTrait;
}
Foo1::$_bar = &apos;Hello&apos;;
Foo2::$_bar = &apos;World&apos;;
echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar; // Prints: Hello World
?>
```



  

#



The best way to understand what traits are and how to use them is to look at them for what they essentially are:&#xA0; language assisted copy and paste.

If you can copy and paste the code from one class to another (and we&apos;ve all done this, even though we try not to because its code duplication) then you have a candidate for a trait.

  

#



Note that the &quot;use&quot; operator for traits (inside a class) and the &quot;use&quot; operator for namespaces (outside the class) resolve names differently. &quot;use&quot; for namespaces always sees its arguments as absolute (starting at the global namespace):



```
<?php
namespace Foo\Bar;
use Foo\Test;&#xA0; // means \Foo\Test - the initial \ is optional
?>
```


On the other hand, &quot;use&quot; for traits respects the current namespace:



```
<?php
namespace Foo\Bar;
class SomeClass {
&#xA0; &#xA0; use Foo\Test;&#xA0;&#xA0; // means \Foo\Bar\Foo\Test
}
?>
```


Together with &quot;use&quot; for closures, there are now three different &quot;use&quot; operators. They all mean different things and behave differently.

  

#



It may be worth noting here that the magic constant __CLASS__ becomes even more magical - __CLASS__ will return the name of the class in which the trait is being used.

for example



```
<?php
trait sayWhere {
&#xA0; &#xA0; public function whereAmI() {
&#xA0; &#xA0; &#xA0; &#xA0; echo __CLASS__;
&#xA0; &#xA0; }
}

class Hello {
&#xA0; &#xA0; use sayWHere;
}

class World {
&#xA0; &#xA0; use sayWHere;
}

$a = new Hello;
$a-&gt;whereAmI(); //Hello

$b = new World;
$b-&gt;whereAmI(); //World
?>
```


The magic constant __TRAIT__ will giev you the name of the trait

  

#



add to &quot;chris dot rutledge at gmail dot com&quot;:
__CLASS__ will return the name of the class in which the trait is being used (!) not the class in which trait method is being called:



```
<?php
trait TestTrait {
&#xA0; &#xA0; public function testMethod() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Class: &quot; . __CLASS__ . PHP_EOL;
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Trait: &quot; . __TRAIT__ . PHP_EOL;
&#xA0; &#xA0; }
}

class BaseClass {
&#xA0; &#xA0; use TestTrait;
}

class TestClass extends BaseClass {

}

$t = new TestClass();
$t-&gt;testMethod();

//Class: BaseClass
//Trait: TestTrait


  

#



Keep in mind; &quot;final&quot; keyword is useless in traits when directly using them, unlike extending classes / abstract classes.



```
<?php
trait Foo {
&#xA0; &#xA0; final public function hello($s) { print &quot;$s, hello!&quot;; }
}
class Bar {
&#xA0; &#xA0; use Foo;
&#xA0; &#xA0; // Overwrite, no error
&#xA0; &#xA0; final public function hello($s) { print &quot;hello, $s!&quot;; }
}

abstract class Foo {
&#xA0; &#xA0; final public function hello($s) { print &quot;$s, hello!&quot;; }
}
class Bar extends Foo {
&#xA0; &#xA0; // Fatal error: Cannot override final method Foo::hello() in ..
&#xA0; &#xA0; final public function hello($s) { print &quot;hello, $s!&quot;; }
}
?>
```


But this way will finalize trait methods as expected;



```
<?php
trait FooTrait {
&#xA0; &#xA0; final public function hello($s) { print &quot;$s, hello!&quot;; }
}
abstract class Foo {
&#xA0; &#xA0; use FooTrait;
}
class Bar extends Foo {
&#xA0; &#xA0; // Fatal error: Cannot override final method Foo::hello() in ..
&#xA0; &#xA0; final public function hello($s) { print &quot;hello, $s!&quot;; }
}
?>
```



  

#



Another difference with traits vs inheritance is that methods defined in traits can access methods and properties of the class they&apos;re used in, including private ones.

For example:


```
<?php
trait MyTrait
{
&#xA0; protected function accessVar()
&#xA0; {
&#xA0; &#xA0; return $this-&gt;var;
&#xA0; }

}

class TraitUser
{
&#xA0; use MyTrait;

&#xA0; private $var = &apos;var&apos;;

&#xA0; public function getVar()
&#xA0; {
&#xA0; &#xA0; return $this-&gt;accessVar();
&#xA0; }
}

$t = new TraitUser();
echo $t-&gt;getVar(); // -&gt; &apos;var&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

?>
```



  

#



As already noted, static properties and methods in trait could be accessed directly using trait. Since trait is language assisted c/p, you should be aware that static property from trait will be initialized to the value trait property had in the time of class declaration. 

Example:



```
<?php

trait Beer {
&#xA0; &#xA0; protected static $type = &apos;Light&apos;;
&#xA0; &#xA0; public static function printed(){
&#xA0; &#xA0; &#xA0; &#xA0; echo static::$type.PHP_EOL;
&#xA0; &#xA0; }
&#xA0; &#xA0; public static function setType($type){
&#xA0; &#xA0; &#xA0; &#xA0; static::$type = $type;
&#xA0; &#xA0; }
}

class Ale {
&#xA0; &#xA0; use Beer;
}

Beer::setType(&quot;Dark&quot;);

class Lager {
&#xA0; &#xA0; use Beer;
}

Beer::setType(&quot;Amber&quot;);

header(&quot;Content-type: text/plain&quot;);

Beer::printed();&#xA0; // Prints: Amber
Ale::printed();&#xA0;&#xA0; // Prints: Light
Lager::printed(); // Prints: Dark

?>
```



  

#



A number of the notes make incorrect assertions about trait behaviour because they do not extend the class.

So, while &quot;Unlike inheritance; if a trait has static properties, each class using that trait has independent instances of those properties.

Example using parent class:


```
<?php
class TestClass {
&#xA0; &#xA0; public static $_bar;
}
class Foo1 extends TestClass { }
class Foo2 extends TestClass { }
Foo1::$_bar = &apos;Hello&apos;;
Foo2::$_bar = &apos;World&apos;;
echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar; // Prints: World World
?>
```


Example using trait:


```
<?php
trait TestTrait {
&#xA0; &#xA0; public static $_bar;
}
class Foo1 {
&#xA0; &#xA0; use TestTrait;
}
class Foo2 {
&#xA0; &#xA0; use TestTrait;
}
Foo1::$_bar = &apos;Hello&apos;;
Foo2::$_bar = &apos;World&apos;;
echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar; // Prints: Hello World
?>
```
&quot;

shows a correct example, simply adding


```
<?php
require_once(&apos;above&apos;);
class Foo3 extends Foo2 {
}
Foo3::$_bar = &apos;news&apos;;
echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar . &apos; &apos; . Foo3::$_bar; 

// Prints: Hello news news

I think the best conceptual model of an incorporated trait is an advanced insertion of text, or as someone put it &quot;language assisted copy and paste.&quot; If Foo1 and Foo2 were defined with $_bar, you would not expect them to share the instance. Similarly, you would expect Foo3 to share with Foo2, and it does.

Viewing this way explains away a lot of&#xA0; the &apos;quirks&apos; that are observed above with final, or subsequently declared private vars,


  

#



Not very obvious but trait methods can be called as if they were defined as static methods in a regular class





```
<?php

trait Foo {

&#xA0; &#xA0; function bar() {

&#xA0; &#xA0; &#xA0; &#xA0; return &apos;baz&apos;;

&#xA0; &#xA0; }

}



echo Foo::bar(),&quot;\\n&quot;;

?>
```



  

#



Traits can not implement interfaces.
(should be obvious, but tested is tested)

  

#





```
<?php
trait A
{
&#xA0; &#xA0; public function bar()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;A::bar&apos;;
&#xA0; &#xA0; }
}

trait B
{
&#xA0; &#xA0; public function bar()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;B::bar&apos;;
&#xA0; &#xA0; }
}

trait C
{
&#xA0; &#xA0; public function bar()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;C::bar&apos;;
&#xA0; &#xA0; }
}

class Foo
{
&#xA0; &#xA0; use A, B, C {
&#xA0; &#xA0; &#xA0; &#xA0; C::bar insteadof A, B;
&#xA0; &#xA0; }
}

$foo = new Foo();
$foo-&gt;bar(); //C::bar


  

#



The magic method __call works as expected using traits.



```
<?php 
trait Call_Helper{
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __call($name, $args){
&#xA0; &#xA0; &#xA0; &#xA0; return count($args);
&#xA0; &#xA0; }
}

class Foo{
&#xA0; &#xA0; use Call_Helper;
}

$foo = new Foo();
echo $foo-&gt;go(1,2,3,4); // echoes 4


  

#



Simple singleton trait.



```
<?php

trait singleton {&#xA0; &#xA0; 
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * private construct, generally defined by using class
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; //private function __construct() {}
&#xA0; &#xA0; 
&#xA0; &#xA0; public static function getInstance() {
&#xA0; &#xA0; &#xA0; &#xA0; static $_instance = NULL;
&#xA0; &#xA0; &#xA0; &#xA0; $class = __CLASS__;
&#xA0; &#xA0; &#xA0; &#xA0; return $_instance ?: $_instance = new $class;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __clone() {
&#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&apos;Cloning &apos;.__CLASS__.&apos; is not allowed.&apos;,E_USER_ERROR);
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __wakeup() {
&#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&apos;Unserializing &apos;.__CLASS__.&apos; is not allowed.&apos;,E_USER_ERROR);
&#xA0; &#xA0; }
}

/**
 * Example Usage
 */

class foo {
&#xA0; &#xA0; use singleton;
&#xA0; &#xA0; 
&#xA0; &#xA0; private function __construct() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;name = &apos;foo&apos;;
&#xA0; &#xA0; }
}

class bar {
&#xA0; &#xA0; use singleton;
&#xA0; &#xA0; 
&#xA0; &#xA0; private function __construct() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;name = &apos;bar&apos;;
&#xA0; &#xA0; }
}

$foo = foo::getInstance();
echo $foo-&gt;name;

$bar = bar::getInstance();
echo $bar-&gt;name;


  

#



The difference between Traits and multiple inheritance is in the inheritance part.&#xA0;&#xA0; A trait is not inherited from, but rather included or mixed-in, thus becoming part of &quot;this class&quot;.&#xA0;&#xA0; Traits also provide a more controlled means of resolving conflicts that inevitably arise when using multiple inheritance in the few languages that support them (C++).&#xA0; Most modern languages are going the approach of a &quot;traits&quot; or &quot;mixin&quot; style system as opposed to multiple-inheritance, largely due to the ability to control ambiguities if a method is declared in multiple &quot;mixed-in&quot; classes.

Also, one can not &quot;inherit&quot; static member functions in multiple-inheritance.

  

#



Note that you can omit a method&apos;s inclusion by excluding it from one trait in favor of the other and doing the exact same thing in the reverse way.



```
<?php

trait A {
&#xA0; &#xA0; public function sayHello()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Hello from A&apos;;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function sayWorld()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;World from A&apos;;
&#xA0; &#xA0; }
}

trait B {
&#xA0; &#xA0; public function sayHello()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Hello from B&apos;;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function sayWorld()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;World from B&apos;;
&#xA0; &#xA0; }
}

class Talker {
&#xA0; &#xA0; use A, B {
&#xA0; &#xA0; &#xA0; &#xA0; A::sayHello insteadof B;
&#xA0; &#xA0; &#xA0; &#xA0; A::sayWorld insteadof B;
&#xA0; &#xA0; &#xA0; &#xA0; B::sayWorld insteadof A;
&#xA0; &#xA0; }
}

$talker = new Talker();
$talker-&gt;sayHello();
$talker-&gt;sayWorld();

?>
```


The method sayHello is imported, but the method sayWorld is simply excluded.

  

#



don&apos;t forget you can create complex (embedded) traits as well



```
<?php
trait Name {
&#xA0; // ...
}
trait Address {
&#xA0; // ...
}
trait Telephone {
&#xA0; // ...
}
trait Contact {
&#xA0; use Name, Address, Telephone;
}
class Customer {
&#xA0; use Contact;
}
class Invoce {
&#xA0; use Contact;
}
?>
```



  

#



A (somewhat) practical example of trait usage.

Without traits:



```
<?php

class Controller {
&#xA0; /* Controller-specific methods defined here. */
}

class AdminController extends Controller {
&#xA0; /* Controller-specific methods inherited from Controller. */
&#xA0; /* Admin-specific methods defined here. */
}

class CrudController extends Controller {
&#xA0; /* Controller-specific methods inherited from Controller. */
&#xA0; /* CRUD-specific methods defined here. */
}

class AdminCrudController extends CrudController {
&#xA0; /* Controller-specific methods inherited from Controller. */
&#xA0; /* CRUD-specific methods inherited from CrudController. */
&#xA0; /* (!!!) Admin-specific methods copied and pasted from AdminController. */
}

?>
```


With traits:



```
<?php

class Controller {
&#xA0; /* Controller-specific methods defined here. */
}

class AdminController extends Controller {
&#xA0; /* Controller-specific methods inherited from Controller. */
&#xA0; /* Admin-specific methods defined here. */
}

trait CrudControllerTrait {
&#xA0; /* CRUD-specific methods defined here. */
}

class AdminCrudController extends AdminController {
&#xA0; use CrudControllerTrait;
&#xA0; /* Controller-specific methods inherited from Controller. */
&#xA0; /* Admin-specific methods inherited from AdminController. */
&#xA0; /* CRUD-specific methods defined by CrudControllerTrait. */
}

?>
```



  

#



Traits are useful for strategies, when you want the same data to be handled (filtered, sorted, etc) differently.

For example, you have a list of products that you want to filter out based on some criteria (brands, specs, whatever), or sorted by different means (price, label, whatever). You can create a sorting trait that contains different functions for different sorting types (numeric, string, date, etc). You can then use this trait not only in your product class (as given in the example), but also in other classes that need similar strategies (to apply a numeric sort to some data, etc).



```
<?php
trait SortStrategy {
&#xA0; &#xA0; private $sort_field = null;
&#xA0; &#xA0; private function string_asc($item1, $item2) {
&#xA0; &#xA0; &#xA0; &#xA0; return strnatcmp($item1[$this-&gt;sort_field], $item2[$this-&gt;sort_field]);
&#xA0; &#xA0; }
&#xA0; &#xA0; private function string_desc($item1, $item2) {
&#xA0; &#xA0; &#xA0; &#xA0; return strnatcmp($item2[$this-&gt;sort_field], $item1[$this-&gt;sort_field]);
&#xA0; &#xA0; }
&#xA0; &#xA0; private function num_asc($item1, $item2) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($item1[$this-&gt;sort_field] == $item2[$this-&gt;sort_field]) return 0;
&#xA0; &#xA0; &#xA0; &#xA0; return ($item1[$this-&gt;sort_field] &lt; $item2[$this-&gt;sort_field] ? -1 : 1 );
&#xA0; &#xA0; }
&#xA0; &#xA0; private function num_desc($item1, $item2) {
&#xA0; &#xA0; &#xA0; &#xA0; if ($item1[$this-&gt;sort_field] == $item2[$this-&gt;sort_field]) return 0;
&#xA0; &#xA0; &#xA0; &#xA0; return ($item1[$this-&gt;sort_field] &gt; $item2[$this-&gt;sort_field] ? -1 : 1 );
&#xA0; &#xA0; }
&#xA0; &#xA0; private function date_asc($item1, $item2) {
&#xA0; &#xA0; &#xA0; &#xA0; $date1 = intval(str_replace(&apos;-&apos;, &apos;&apos;, $item1[$this-&gt;sort_field]));
&#xA0; &#xA0; &#xA0; &#xA0; $date2 = intval(str_replace(&apos;-&apos;, &apos;&apos;, $item2[$this-&gt;sort_field]));
&#xA0; &#xA0; &#xA0; &#xA0; if ($date1 == $date2) return 0;
&#xA0; &#xA0; &#xA0; &#xA0; return ($date1 &lt; $date2 ? -1 : 1 );
&#xA0; &#xA0; }
&#xA0; &#xA0; private function date_desc($item1, $item2) {
&#xA0; &#xA0; &#xA0; &#xA0; $date1 = intval(str_replace(&apos;-&apos;, &apos;&apos;, $item1[$this-&gt;sort_field]));
&#xA0; &#xA0; &#xA0; &#xA0; $date2 = intval(str_replace(&apos;-&apos;, &apos;&apos;, $item2[$this-&gt;sort_field]));
&#xA0; &#xA0; &#xA0; &#xA0; if ($date1 == $date2) return 0;
&#xA0; &#xA0; &#xA0; &#xA0; return ($date1 &gt; $date2 ? -1 : 1 );
&#xA0; &#xA0; }
}

class Product {
&#xA0; &#xA0; public $data = array();
&#xA0; &#xA0; 
&#xA0; &#xA0; use SortStrategy;
&#xA0; &#xA0; 
&#xA0; &#xA0; public function get() {
&#xA0; &#xA0; &#xA0; &#xA0; // do something to get the data, for this ex. I just included an array
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;data = array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 101222 =&gt; array(&apos;label&apos; =&gt; &apos;Awesome product&apos;, &apos;price&apos; =&gt; 10.50, &apos;date_added&apos; =&gt; &apos;2012-02-01&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 101232 =&gt; array(&apos;label&apos; =&gt; &apos;Not so awesome product&apos;, &apos;price&apos; =&gt; 5.20, &apos;date_added&apos; =&gt; &apos;2012-03-20&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 101241 =&gt; array(&apos;label&apos; =&gt; &apos;Pretty neat product&apos;, &apos;price&apos; =&gt; 9.65, &apos;date_added&apos; =&gt; &apos;2012-04-15&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 101256 =&gt; array(&apos;label&apos; =&gt; &apos;Freakishly cool product&apos;, &apos;price&apos; =&gt; 12.55, &apos;date_added&apos; =&gt; &apos;2012-01-11&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 101219 =&gt; array(&apos;label&apos; =&gt; &apos;Meh product&apos;, &apos;price&apos; =&gt; 3.69, &apos;date_added&apos; =&gt; &apos;2012-06-11&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function sort_by($by = &apos;price&apos;, $type = &apos;asc&apos;) {
&#xA0; &#xA0; &#xA0; &#xA0; if (!preg_match(&apos;/^(asc|desc)$/&apos;, $type)) $type = &apos;asc&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; switch ($by) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case &apos;name&apos;:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;sort_field = &apos;label&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; uasort($this-&gt;data, array(&apos;Product&apos;, &apos;string_&apos;.$type));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case &apos;date&apos;:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;sort_field = &apos;date_added&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; uasort($this-&gt;data, array(&apos;Product&apos;, &apos;date_&apos;.$type));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; default:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;sort_field = &apos;price&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; uasort($this-&gt;data, array(&apos;Product&apos;, &apos;num_&apos;.$type));
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}

$product = new Product();
$product-&gt;get();
$product-&gt;sort_by(&apos;name&apos;);
echo &apos;&lt;pre&gt;&apos;.print_r($product-&gt;data, true).&apos;&lt;/pre&gt;&apos;;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.traits.php)

**[To root](/README.md)**
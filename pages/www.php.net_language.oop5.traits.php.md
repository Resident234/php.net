# Traits



Unlike inheritance; if a trait has static properties, each class using that trait has independent instances of those properties.<br><br>Example using parent class:<br>

```
<?php
class TestClass {
    public static $_bar;
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
    public static $_bar;
}
class Foo1 {
    use TestTrait;
}
class Foo2 {
    use TestTrait;
}
Foo1::$_bar = &apos;Hello&apos;;
Foo2::$_bar = &apos;World&apos;;
echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar; // Prints: Hello World
?>
```
  

#

The best way to understand what traits are and how to use them is to look at them for what they essentially are:  language assisted copy and paste.<br><br>If you can copy and paste the code from one class to another (and we&apos;ve all done this, even though we try not to because its code duplication) then you have a candidate for a trait.  

#

Note that the "use" operator for traits (inside a class) and the "use" operator for namespaces (outside the class) resolve names differently. "use" for namespaces always sees its arguments as absolute (starting at the global namespace):<br><br>

```
<?php
namespace Foo\Bar;
use Foo\Test;  // means \Foo\Test - the initial \ is optional
?>
```


On the other hand, "use" for traits respects the current namespace:



```
<?php
namespace Foo\Bar;
class SomeClass {
    use Foo\Test;   // means \Foo\Bar\Foo\Test
}
?>
```
<br><br>Together with "use" for closures, there are now three different "use" operators. They all mean different things and behave differently.  

#

It may be worth noting here that the magic constant __CLASS__ becomes even more magical - __CLASS__ will return the name of the class in which the trait is being used.<br><br>for example<br><br>

```
<?php
trait sayWhere {
    public function whereAmI() {
        echo __CLASS__;
    }
}

class Hello {
    use sayWHere;
}

class World {
    use sayWHere;
}

$a = new Hello;
$a-&gt;whereAmI(); //Hello

$b = new World;
$b-&gt;whereAmI(); //World
?>
```
<br><br>The magic constant __TRAIT__ will giev you the name of the trait  

#

add to "chris dot rutledge at gmail dot com":<br>__CLASS__ will return the name of the class in which the trait is being used (!) not the class in which trait method is being called:<br><br>

```
<?php<br>trait TestTrait {<br>    public function testMethod() {<br>        echo "Class: " . __CLASS__ . PHP_EOL;<br>        echo "Trait: " . __TRAIT__ . PHP_EOL;<br>    }<br>}<br><br>class BaseClass {<br>    use TestTrait;<br>}<br><br>class TestClass extends BaseClass {<br><br>}<br><br>$t = new TestClass();<br>$t-&gt;testMethod();<br><br>//Class: BaseClass<br>//Trait: TestTrait  

#

Keep in mind; "final" keyword is useless in traits when directly using them, unlike extending classes / abstract classes.<br><br>

```
<?php
trait Foo {
    final public function hello($s) { print "$s, hello!"; }
}
class Bar {
    use Foo;
    // Overwrite, no error
    final public function hello($s) { print "hello, $s!"; }
}

abstract class Foo {
    final public function hello($s) { print "$s, hello!"; }
}
class Bar extends Foo {
    // Fatal error: Cannot override final method Foo::hello() in ..
    final public function hello($s) { print "hello, $s!"; }
}
?>
```


But this way will finalize trait methods as expected;



```
<?php
trait FooTrait {
    final public function hello($s) { print "$s, hello!"; }
}
abstract class Foo {
    use FooTrait;
}
class Bar extends Foo {
    // Fatal error: Cannot override final method Foo::hello() in ..
    final public function hello($s) { print "hello, $s!"; }
}
?>
```
  

#

Another difference with traits vs inheritance is that methods defined in traits can access methods and properties of the class they&apos;re used in, including private ones.<br><br>For example:<br>

```
<?php
trait MyTrait
{
  protected function accessVar()
  {
    return $this-&gt;var;
  }

}

class TraitUser
{
  use MyTrait;

  private $var = &apos;var&apos;;

  public function getVar()
  {
    return $this-&gt;accessVar();
  }
}

$t = new TraitUser();
echo $t-&gt;getVar(); // -&gt; &apos;var&apos;                                                                                                                                                                                                                          

?>
```
  

#

As already noted, static properties and methods in trait could be accessed directly using trait. Since trait is language assisted c/p, you should be aware that static property from trait will be initialized to the value trait property had in the time of class declaration. <br><br>Example:<br><br>

```
<?php

trait Beer {
    protected static $type = &apos;Light&apos;;
    public static function printed(){
        echo static::$type.PHP_EOL;
    }
    public static function setType($type){
        static::$type = $type;
    }
}

class Ale {
    use Beer;
}

Beer::setType("Dark");

class Lager {
    use Beer;
}

Beer::setType("Amber");

header("Content-type: text/plain");

Beer::printed();  // Prints: Amber
Ale::printed();   // Prints: Light
Lager::printed(); // Prints: Dark

?>
```
  

#

A number of the notes make incorrect assertions about trait behaviour because they do not extend the class.<br><br>So, while "Unlike inheritance; if a trait has static properties, each class using that trait has independent instances of those properties.<br><br>Example using parent class:<br>

```
<?php
class TestClass {
    public static $_bar;
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
    public static $_bar;
}
class Foo1 {
    use TestTrait;
}
class Foo2 {
    use TestTrait;
}
Foo1::$_bar = &apos;Hello&apos;;
Foo2::$_bar = &apos;World&apos;;
echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar; // Prints: Hello World
?>
```
"

shows a correct example, simply adding


```
<?php<br>require_once(&apos;above&apos;);<br>class Foo3 extends Foo2 {<br>}<br>Foo3::$_bar = &apos;news&apos;;<br>echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar . &apos; &apos; . Foo3::$_bar; <br><br>// Prints: Hello news news<br><br>I think the best conceptual model of an incorporated trait is an advanced insertion of text, or as someone put it "language assisted copy and paste." If Foo1 and Foo2 were defined with $_bar, you would not expect them to share the instance. Similarly, you would expect Foo3 to share with Foo2, and it does.<br><br>Viewing this way explains away a lot of  the &apos;quirks&apos; that are observed above with final, or subsequently declared private vars,  

#

Not very obvious but trait methods can be called as if they were defined as static methods in a regular class<br><br>

```
<?php
trait Foo {
    function bar() {
        return &apos;baz&apos;;
    }
}

echo Foo::bar(),"\\n";
?>
```
  

#

Traits can not implement interfaces.<br>(should be obvious, but tested is tested)  

#



```
<?php<br>trait A<br>{<br>    public function bar()<br>    {<br>        echo &apos;A::bar&apos;;<br>    }<br>}<br><br>trait B<br>{<br>    public function bar()<br>    {<br>        echo &apos;B::bar&apos;;<br>    }<br>}<br><br>trait C<br>{<br>    public function bar()<br>    {<br>        echo &apos;C::bar&apos;;<br>    }<br>}<br><br>class Foo<br>{<br>    use A, B, C {<br>        C::bar insteadof A, B;<br>    }<br>}<br><br>$foo = new Foo();<br>$foo-&gt;bar(); //C::bar  

#

The magic method __call works as expected using traits.<br><br>

```
<?php 
trait Call_Helper{
    
    public function __call($name, $args){
        return count($args);
    }
}

class Foo{
    use Call_Helper;
}

$foo = new Foo();
echo $foo-&gt;go(1,2,3,4); // echoes 4?>
```
  

#

Simple singleton trait.<br><br>

```
<?php<br><br>trait singleton {    <br>    /**<br>     * private construct, generally defined by using class<br>     */<br>    //private function __construct() {}<br>    <br>    public static function getInstance() {<br>        static $_instance = NULL;<br>        $class = __CLASS__;<br>        return $_instance ?: $_instance = new $class;<br>    }<br>    <br>    public function __clone() {<br>        trigger_error(&apos;Cloning &apos;.__CLASS__.&apos; is not allowed.&apos;,E_USER_ERROR);<br>    }<br>    <br>    public function __wakeup() {<br>        trigger_error(&apos;Unserializing &apos;.__CLASS__.&apos; is not allowed.&apos;,E_USER_ERROR);<br>    }<br>}<br><br>/**<br> * Example Usage<br> */<br><br>class foo {<br>    use singleton;<br>    <br>    private function __construct() {<br>        $this-&gt;name = &apos;foo&apos;;<br>    }<br>}<br><br>class bar {<br>    use singleton;<br>    <br>    private function __construct() {<br>        $this-&gt;name = &apos;bar&apos;;<br>    }<br>}<br><br>$foo = foo::getInstance();<br>echo $foo-&gt;name;<br><br>$bar = bar::getInstance();<br>echo $bar-&gt;name;  

#

The difference between Traits and multiple inheritance is in the inheritance part.   A trait is not inherited from, but rather included or mixed-in, thus becoming part of "this class".   Traits also provide a more controlled means of resolving conflicts that inevitably arise when using multiple inheritance in the few languages that support them (C++).  Most modern languages are going the approach of a "traits" or "mixin" style system as opposed to multiple-inheritance, largely due to the ability to control ambiguities if a method is declared in multiple "mixed-in" classes.<br><br>Also, one can not "inherit" static member functions in multiple-inheritance.  

#

Note that you can omit a method&apos;s inclusion by excluding it from one trait in favor of the other and doing the exact same thing in the reverse way.<br><br>

```
<?php

trait A {
    public function sayHello()
    {
        echo &apos;Hello from A&apos;;
    }

    public function sayWorld()
    {
        echo &apos;World from A&apos;;
    }
}

trait B {
    public function sayHello()
    {
        echo &apos;Hello from B&apos;;
    }

    public function sayWorld()
    {
        echo &apos;World from B&apos;;
    }
}

class Talker {
    use A, B {
        A::sayHello insteadof B;
        A::sayWorld insteadof B;
        B::sayWorld insteadof A;
    }
}

$talker = new Talker();
$talker-&gt;sayHello();
$talker-&gt;sayWorld();

?>
```
<br><br>The method sayHello is imported, but the method sayWorld is simply excluded.  

#

don&apos;t forget you can create complex (embedded) traits as well<br><br>

```
<?php
trait Name {
  // ...
}
trait Address {
  // ...
}
trait Telephone {
  // ...
}
trait Contact {
  use Name, Address, Telephone;
}
class Customer {
  use Contact;
}
class Invoce {
  use Contact;
}
?>
```
  

#

A (somewhat) practical example of trait usage.<br><br>Without traits:<br><br>

```
<?php

class Controller {
  /* Controller-specific methods defined here. */
}

class AdminController extends Controller {
  /* Controller-specific methods inherited from Controller. */
  /* Admin-specific methods defined here. */
}

class CrudController extends Controller {
  /* Controller-specific methods inherited from Controller. */
  /* CRUD-specific methods defined here. */
}

class AdminCrudController extends CrudController {
  /* Controller-specific methods inherited from Controller. */
  /* CRUD-specific methods inherited from CrudController. */
  /* (!!!) Admin-specific methods copied and pasted from AdminController. */
}

?>
```


With traits:



```
<?php

class Controller {
  /* Controller-specific methods defined here. */
}

class AdminController extends Controller {
  /* Controller-specific methods inherited from Controller. */
  /* Admin-specific methods defined here. */
}

trait CrudControllerTrait {
  /* CRUD-specific methods defined here. */
}

class AdminCrudController extends AdminController {
  use CrudControllerTrait;
  /* Controller-specific methods inherited from Controller. */
  /* Admin-specific methods inherited from AdminController. */
  /* CRUD-specific methods defined by CrudControllerTrait. */
}

?>
```
  

#

Traits are useful for strategies, when you want the same data to be handled (filtered, sorted, etc) differently.<br><br>For example, you have a list of products that you want to filter out based on some criteria (brands, specs, whatever), or sorted by different means (price, label, whatever). You can create a sorting trait that contains different functions for different sorting types (numeric, string, date, etc). You can then use this trait not only in your product class (as given in the example), but also in other classes that need similar strategies (to apply a numeric sort to some data, etc).<br><br>

```
<?php
trait SortStrategy {
    private $sort_field = null;
    private function string_asc($item1, $item2) {
        return strnatcmp($item1[$this-&gt;sort_field], $item2[$this-&gt;sort_field]);
    }
    private function string_desc($item1, $item2) {
        return strnatcmp($item2[$this-&gt;sort_field], $item1[$this-&gt;sort_field]);
    }
    private function num_asc($item1, $item2) {
        if ($item1[$this-&gt;sort_field] == $item2[$this-&gt;sort_field]) return 0;
        return ($item1[$this-&gt;sort_field] &lt; $item2[$this-&gt;sort_field] ? -1 : 1 );
    }
    private function num_desc($item1, $item2) {
        if ($item1[$this-&gt;sort_field] == $item2[$this-&gt;sort_field]) return 0;
        return ($item1[$this-&gt;sort_field] &gt; $item2[$this-&gt;sort_field] ? -1 : 1 );
    }
    private function date_asc($item1, $item2) {
        $date1 = intval(str_replace(&apos;-&apos;, &apos;&apos;, $item1[$this-&gt;sort_field]));
        $date2 = intval(str_replace(&apos;-&apos;, &apos;&apos;, $item2[$this-&gt;sort_field]));
        if ($date1 == $date2) return 0;
        return ($date1 &lt; $date2 ? -1 : 1 );
    }
    private function date_desc($item1, $item2) {
        $date1 = intval(str_replace(&apos;-&apos;, &apos;&apos;, $item1[$this-&gt;sort_field]));
        $date2 = intval(str_replace(&apos;-&apos;, &apos;&apos;, $item2[$this-&gt;sort_field]));
        if ($date1 == $date2) return 0;
        return ($date1 &gt; $date2 ? -1 : 1 );
    }
}

class Product {
    public $data = array();
    
    use SortStrategy;
    
    public function get() {
        // do something to get the data, for this ex. I just included an array
        $this-&gt;data = array(
            101222 =&gt; array(&apos;label&apos; =&gt; &apos;Awesome product&apos;, &apos;price&apos; =&gt; 10.50, &apos;date_added&apos; =&gt; &apos;2012-02-01&apos;),
            101232 =&gt; array(&apos;label&apos; =&gt; &apos;Not so awesome product&apos;, &apos;price&apos; =&gt; 5.20, &apos;date_added&apos; =&gt; &apos;2012-03-20&apos;),
            101241 =&gt; array(&apos;label&apos; =&gt; &apos;Pretty neat product&apos;, &apos;price&apos; =&gt; 9.65, &apos;date_added&apos; =&gt; &apos;2012-04-15&apos;),
            101256 =&gt; array(&apos;label&apos; =&gt; &apos;Freakishly cool product&apos;, &apos;price&apos; =&gt; 12.55, &apos;date_added&apos; =&gt; &apos;2012-01-11&apos;),
            101219 =&gt; array(&apos;label&apos; =&gt; &apos;Meh product&apos;, &apos;price&apos; =&gt; 3.69, &apos;date_added&apos; =&gt; &apos;2012-06-11&apos;),
        );
    }
    
    public function sort_by($by = &apos;price&apos;, $type = &apos;asc&apos;) {
        if (!preg_match(&apos;/^(asc|desc)$/&apos;, $type)) $type = &apos;asc&apos;;
        switch ($by) {
            case &apos;name&apos;:
                $this-&gt;sort_field = &apos;label&apos;;
                uasort($this-&gt;data, array(&apos;Product&apos;, &apos;string_&apos;.$type));
            break;
            case &apos;date&apos;:
                $this-&gt;sort_field = &apos;date_added&apos;;
                uasort($this-&gt;data, array(&apos;Product&apos;, &apos;date_&apos;.$type));
            break;
            default:
                $this-&gt;sort_field = &apos;price&apos;;
                uasort($this-&gt;data, array(&apos;Product&apos;, &apos;num_&apos;.$type));
        }
    }
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
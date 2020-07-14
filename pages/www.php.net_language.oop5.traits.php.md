# Traits



Unlike inheritance; if a trait has static properties, each class using that trait has independent instances of those properties.<br><br>Example using parent class:<br>

```
<?php
class TestClass {
    public static $_bar;
}
class Foo1 extends TestClass { }
class Foo2 extends TestClass { }
Foo1::$_bar = 'Hello';
Foo2::$_bar = 'World';
echo Foo1::$_bar . ' ' . Foo2::$_bar; // Prints: World World
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
Foo1::$_bar = 'Hello';
Foo2::$_bar = 'World';
echo Foo1::$_bar . ' ' . Foo2::$_bar; // Prints: Hello World
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
$a->whereAmI(); //Hello

$b = new World;
$b->whereAmI(); //World
?>
```
<br><br>The magic constant __TRAIT__ will giev you the name of the trait  

#

add to "chris dot rutledge at gmail dot com":<br>__CLASS__ will return the name of the class in which the trait is being used (!) not the class in which trait method is being called:<br><br>

```
<?php
trait TestTrait {
    public function testMethod() {
        echo "Class: " . __CLASS__ . PHP_EOL;
        echo "Trait: " . __TRAIT__ . PHP_EOL;
    }
}

class BaseClass {
    use TestTrait;
}

class TestClass extends BaseClass {

}

$t = new TestClass();
$t->testMethod();

//Class: BaseClass
//Trait: TestTrait?>
```
  

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
    return $this->var;
  }

}

class TraitUser
{
  use MyTrait;

  private $var = 'var';

  public function getVar()
  {
    return $this->accessVar();
  }
}

$t = new TraitUser();
echo $t->getVar(); // -> 'var'                                                                                                                                                                                                                          

?>
```
  

#

As already noted, static properties and methods in trait could be accessed directly using trait. Since trait is language assisted c/p, you should be aware that static property from trait will be initialized to the value trait property had in the time of class declaration. <br><br>Example:<br><br>

```
<?php

trait Beer {
    protected static $type = 'Light';
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
Foo1::$_bar = 'Hello';
Foo2::$_bar = 'World';
echo Foo1::$_bar . ' ' . Foo2::$_bar; // Prints: World World
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
Foo1::$_bar = 'Hello';
Foo2::$_bar = 'World';
echo Foo1::$_bar . ' ' . Foo2::$_bar; // Prints: Hello World
?>
```
"<br><br>shows a correct example, simply adding<br>

```
<?php<br>require_once(&apos;above&apos;);<br>class Foo3 extends Foo2 {<br>}<br>Foo3::$_bar = &apos;news&apos;;<br>echo Foo1::$_bar . &apos; &apos; . Foo2::$_bar . &apos; &apos; . Foo3::$_bar; <br><br>// Prints: Hello news news<br><br>I think the best conceptual model of an incorporated trait is an advanced insertion of text, or as someone put it "language assisted copy and paste." If Foo1 and Foo2 were defined with $_bar, you would not expect them to share the instance. Similarly, you would expect Foo3 to share with Foo2, and it does.<br><br>Viewing this way explains away a lot of  the &apos;quirks&apos; that are observed above with final, or subsequently declared private vars,  

#

Not very obvious but trait methods can be called as if they were defined as static methods in a regular class<br><br>

```
<?php
trait Foo {
    function bar() {
        return 'baz';
    }
}

echo Foo::bar(),"\\n";
?>
```
  

#

Traits can not implement interfaces.<br>(should be obvious, but tested is tested)  

#



```
<?php
trait A
{
    public function bar()
    {
        echo 'A::bar';
    }
}

trait B
{
    public function bar()
    {
        echo 'B::bar';
    }
}

trait C
{
    public function bar()
    {
        echo 'C::bar';
    }
}

class Foo
{
    use A, B, C {
        C::bar insteadof A, B;
    }
}

$foo = new Foo();
$foo->bar(); //C::bar?>
```
  

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
echo $foo->go(1,2,3,4); // echoes 4?>
```
  

#

Simple singleton trait.<br><br>

```
<?php

trait singleton {    
    /**
     * private construct, generally defined by using class
     */
    //private function __construct() {}
    
    public static function getInstance() {
        static $_instance = NULL;
        $class = __CLASS__;
        return $_instance ?: $_instance = new $class;
    }
    
    public function __clone() {
        trigger_error('Cloning '.__CLASS__.' is not allowed.',E_USER_ERROR);
    }
    
    public function __wakeup() {
        trigger_error('Unserializing '.__CLASS__.' is not allowed.',E_USER_ERROR);
    }
}

/**
 * Example Usage
 */

class foo {
    use singleton;
    
    private function __construct() {
        $this->name = 'foo';
    }
}

class bar {
    use singleton;
    
    private function __construct() {
        $this->name = 'bar';
    }
}

$foo = foo::getInstance();
echo $foo->name;

$bar = bar::getInstance();
echo $bar->name;?>
```
  

#

The difference between Traits and multiple inheritance is in the inheritance part.   A trait is not inherited from, but rather included or mixed-in, thus becoming part of "this class".   Traits also provide a more controlled means of resolving conflicts that inevitably arise when using multiple inheritance in the few languages that support them (C++).  Most modern languages are going the approach of a "traits" or "mixin" style system as opposed to multiple-inheritance, largely due to the ability to control ambiguities if a method is declared in multiple "mixed-in" classes.<br><br>Also, one can not "inherit" static member functions in multiple-inheritance.  

#

Note that you can omit a method&apos;s inclusion by excluding it from one trait in favor of the other and doing the exact same thing in the reverse way.<br><br>

```
<?php

trait A {
    public function sayHello()
    {
        echo 'Hello from A';
    }

    public function sayWorld()
    {
        echo 'World from A';
    }
}

trait B {
    public function sayHello()
    {
        echo 'Hello from B';
    }

    public function sayWorld()
    {
        echo 'World from B';
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
$talker->sayHello();
$talker->sayWorld();

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
        return strnatcmp($item1[$this->sort_field], $item2[$this->sort_field]);
    }
    private function string_desc($item1, $item2) {
        return strnatcmp($item2[$this->sort_field], $item1[$this->sort_field]);
    }
    private function num_asc($item1, $item2) {
        if ($item1[$this->sort_field] == $item2[$this->sort_field]) return 0;
        return ($item1[$this->sort_field] &lt; $item2[$this->sort_field] ? -1 : 1 );
    }
    private function num_desc($item1, $item2) {
        if ($item1[$this->sort_field] == $item2[$this->sort_field]) return 0;
        return ($item1[$this->sort_field] &gt; $item2[$this->sort_field] ? -1 : 1 );
    }
    private function date_asc($item1, $item2) {
        $date1 = intval(str_replace('-', '', $item1[$this->sort_field]));
        $date2 = intval(str_replace('-', '', $item2[$this->sort_field]));
        if ($date1 == $date2) return 0;
        return ($date1 &lt; $date2 ? -1 : 1 );
    }
    private function date_desc($item1, $item2) {
        $date1 = intval(str_replace('-', '', $item1[$this->sort_field]));
        $date2 = intval(str_replace('-', '', $item2[$this->sort_field]));
        if ($date1 == $date2) return 0;
        return ($date1 &gt; $date2 ? -1 : 1 );
    }
}

class Product {
    public $data = array();
    
    use SortStrategy;
    
    public function get() {
        // do something to get the data, for this ex. I just included an array
        $this->data = array(
            101222 => array('label' => 'Awesome product', 'price' => 10.50, 'date_added' => '2012-02-01'),
            101232 => array('label' => 'Not so awesome product', 'price' => 5.20, 'date_added' => '2012-03-20'),
            101241 => array('label' => 'Pretty neat product', 'price' => 9.65, 'date_added' => '2012-04-15'),
            101256 => array('label' => 'Freakishly cool product', 'price' => 12.55, 'date_added' => '2012-01-11'),
            101219 => array('label' => 'Meh product', 'price' => 3.69, 'date_added' => '2012-06-11'),
        );
    }
    
    public function sort_by($by = 'price', $type = 'asc') {
        if (!preg_match('/^(asc|desc)$/', $type)) $type = 'asc';
        switch ($by) {
            case 'name':
                $this->sort_field = 'label';
                uasort($this->data, array('Product', 'string_'.$type));
            break;
            case 'date':
                $this->sort_field = 'date_added';
                uasort($this->data, array('Product', 'date_'.$type));
            break;
            default:
                $this->sort_field = 'price';
                uasort($this->data, array('Product', 'num_'.$type));
        }
    }
}

$product = new Product();
$product->get();
$product->sort_by('name');
echo '&lt;pre&gt;'.print_r($product->data, true).'&lt;/pre&gt;';
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.traits.php)

**[To root](/README.md)**
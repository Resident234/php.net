# Object Interfaces



It seems like many contributors are missing the point of using an INTERFACE. An INTERFACE is not specifically provided for abstraction. That&apos;s what a CLASS is used for. Most examples in this article of interfaces could be achieved just as easily using just classes alone. <br><br>An INTERFACE is provided so you can describe a set of functions and then hide the final implementation of those functions in an implementing class. This allows you to change the IMPLEMENTATION of those functions without changing how you use it. <br><br>For example: I have a database. I want to write a class that accesses the data in my database. I define an interface like this:<br><br>interface Database {<br>function listOrders();<br>function addOrder();<br>function removeOrder();<br>...<br>}<br><br>Then let&apos;s say we start out using a MySQL database. So we write a class to access the MySQL database:<br><br>class MySqlDatabase implements Database {<br>function listOrders() {...<br>}<br>we write these methods as needed to get to the MySQL database tables. Then you can write your controller to use the interface as such:<br><br>$database = new MySqlDatabase();<br>foreach ($database-&gt;listOrders() as $order) {<br><br>Then let&apos;s say we decide to migrate to an Oracle database. We could write another class to get to the Oracle database as such:<br><br>class OracleDatabase implements Database {<br>public function listOrders() {...<br>}<br><br>Then - to switch our application to use the Oracle database instead of the MySQL database we only have to change ONE LINE of code:<br><br>$database = new OracleDatabase();<br><br>all other lines of code, such as:<br><br>foreach ($database-&gt;listOrders() as $order) {<br><br>will remain unchanged. The point is - the INTERFACE describes the methods that we need to access our database. It does NOT describe in any way HOW we achieve that. That&apos;s what the IMPLEMENTing class does. We can IMPLEMENT this interface as many times as we need in as many different ways as we need. We can then switch between implementations of the interface without impact to our code because the interface defines how we will use it regardless of how it actually works.  

---

Just wrote some examples of duck-typing in PHP. Sharing here.<br><br>

```
<?php

/**
 * An example of duck typing in PHP
 */

interface CanFly {
  public function fly();
}

interface CanSwim {
  public function swim();
}

class Bird {
  public function info() {
    echo "I am a {$this->name}\n";
    echo "I am an bird\n";
  }
}

/**
 * some implementations of birds
 */
class Dove extends Bird implements CanFly {
  var $name = "Dove";
  public function fly() {
    echo "I fly\n";
  } 
}

class Penguin extends Bird implements CanSwim {
  var $name = "Penguin";
  public function swim() {
    echo "I swim\n";
  } 
}

class Duck extends Bird implements CanFly, CanSwim {
  var $name = "Duck";
  public function fly() {
    echo "I fly\n";
  }
  public function swim() {
    echo "I swim\n";
  }
}

/**
 * a simple function to describe a bird
 */
function describe($bird) {
  if ($bird instanceof Bird) {
    $bird->info();
    if ($bird instanceof CanFly) {
      $bird->fly();
    }
    if ($bird instanceof CanSwim) {
      $bird->swim();
    }
  } else {
    die("This is not a bird. I cannot describe it.");
  }
}

// describe these birds please
describe(new Penguin);
echo "---\n";

describe(new Dove);
echo "---\n";

describe(new Duck);?>
```
  

---

If it&apos;s not already obvious, it&apos;s worth noticing that the parameters in the interface&apos;s method declaration do not have to have the same names as those in any of its implementations.<br><br>More significantly, default argument values may be supplied for interface method parameters, and they have to be if you want to use default argument values in the implemented classes:<br><br>

```
<?php
interface isStuffable
{
    public function getStuffed($ratio=0.5);
}

class Turkey implements isStuffable
{
    public function getStuffed($stuffing=1)
    {
        // ....
    }
}
?>
```
<br><br>Note that not only do the parameters have different names ($ratio and $stuffing), but their default values are free to be different as well. There doesn&apos;t seem to be any purpose to the interface&apos;s default argument value except as a dummy placeholder to show that there is a default (a class implementing isStuffable will not be able to implement methods with the signatures getStuffed(), getStuffed($a), or getStuffed($a,$b)).  

---

The class implementing the interface must use the exact same method signatures as are defined in the interface. Not doing so will result in a fatal error. -- this documentation page.<br><br>But, if you use default values in arguments in methods, fatal error is not follow:<br><br>

```
<?php
interface myInterface {
    public function __construct();
}

class concret implements myInterface {

    public function __construct($arg=null)
    {
        print_r(func_get_args());
    }
}

$obj = new concret(123);
?>
```
<br><br>Array ( [0] =&gt; 123 )  

---

When should you use interfaces?  What are they good for? <br>Here are two examples.  <br><br>1. Interfaces are an excellent way to implement reusability.  <br>You can create a general interface for a number of situations <br>(such as a save to/load from disk interface.)  You can then <br>implement the interface in a variety of different ways (e.g. for <br>formats such as tab delimited ASCII, XML and a database.)  <br>You can write code that asks the object to "save itself to <br>disk" without having to worry what that means for the object <br>in question.  One object might save itself to the database, <br>another to an XML and you can change this behavior over <br>time without having to rewrite the calling code.  <br><br>This allows you to write reusable calling code that can work <br>for any number of different objects -- you don&apos;t need to know <br>what kind of object it is, as long as it obeys the common <br>interface.  <br><br>2. Interfaces can also promote gradual evolution.  On a <br>recent project I had some very complicated work to do and I <br>didn&apos;t know how to implement it.  I could think of a "basic" <br>implementation but I knew I would have to change it later.  <br>So I created interfaces in each of these cases, and created <br>at least one "basic" implementation of the interface that <br>was "good enough for now" even though I knew it would have <br>to change later.  <br><br>When I came back to make the changes, I was able to create <br>some new implementations of these interfaces that added the <br>extra features I needed.  Some of my classes still used <br>the "basic" implementations, but others needed the <br>specialized ones.  I was able to add the new features to the <br>objects themselves without rewriting the calling code in most <br>cases.  It was easy to evolve my code in this way because <br>the changes were mostly isolated -- they didn&apos;t spread all <br>over the place like you might expect.  

---

Implementation must be strict, subtypes are not allowed.<br><br>"The class implementing the interface must use the EXACT SAME METHOD SIGNATURES as are defined in the interface. Not doing so will result in a fatal error. "<br><br>

```
<?php

interface I
{
  function foo(stdClass $arg);
}

class Test extends stdClass
{
}

class Implementation implements I
{
  function foo(Test $arg)
    {
    }
}
?>
```
<br>Result:<br><br>Fatal error: Declaration of InterfaceImplementation::foo() must be compatible with I::foo(stdClass $arg) in test.php on line XY  

---

On an incidental note, it is not necessary for the implementation of an interface method to use the same variable names for its parameters that were used in the interface declaration.<br><br>More significantly, your interface method declarations can include default argument values. If you do, you must specify their implementations with default arguments, too. Just like the parameter names, the default argument values do not need to be the same. In fact, there doesn&apos;t seem to be any functionality to the one in the interface declaration at all beyond the fact that it is there.<br><br>

```
<?php
interface isStuffed {
    public function getStuff($something=17);
}

class oof implements isStuffed {
    public function getStuff($a=42) {
        return $a;
    }
}

$oof = new oof;

echo $oof->getStuff();
?>
```
<br><br>Implementations that try to declare the method as getStuff(), getStuff($a), or getStuff($a,$b) will all trigger a fatal error.  

---

I was wondering if implementing interfaces will take into account inheritance. That is, can inherited methods be used to follow an interface&apos;s structure?<br><br>

```
<?php

interface Auxiliary_Platform {
    public function Weapon();
    public function Health();
    public function Shields();
}

class T805 implements Auxiliary_Platform {
    public function Weapon() {
        var_dump(__CLASS__);
    }
    public function Health() {
        var_dump(__CLASS__ . "::" . __FUNCTION__);
    }
    public function Shields() {
        var_dump(__CLASS__ . "->" . __FUNCTION__);
    }
}

class T806 extends T805 implements Auxiliary_Platform {
    public function Weapon() {
        var_dump(__CLASS__);
    }
    public function Shields() {
        var_dump(__CLASS__ . "->" . __FUNCTION__);
    }
}

$T805 = new T805();
$T805->Weapon();
$T805->Health();
$T805->Shields();

echo "<hr />";

$T806 = new T806();
$T806->Weapon();
$T806->Health();
$T806->Shields();

/* Output:
string(4) "T805"
string(12) "T805::Health"
string(13) "T805->Shields"
<hr />string(4) "T806"
string(12) "T805::Health"
string(13) "T806->Shields"
*/

?>
```


Class T805 implements the interface Auxiliary_Platform. T806 does the same thing, but the method Health() is inherited from T805 (not the exact case, but you get the idea). PHP seems to be fine with this and everything still works fine. Do note that the rules for class inheritance doesn't change in this scenario.

If the code were to be the same, but instead T805 (or T806) DOES NOT implement Auxiliary_Platform, then it'll still work. Since T805 already follows the interface, everything that inherits T805 will also be valid. I would be careful about that. Personally, I don't consider this a bug.

This seems to work in PHP5.2.9-2, PHP5.3 and PHP5.3.1 (my current versions).

We could also do the opposite:



```
<?php

class T805 {
    public function Weapon() {
        var_dump(__CLASS__);
    }
}

class T806 extends T805 implements Auxiliary_Platform {
    public function Health() {
        var_dump(__CLASS__ . "::" . __FUNCTION__);
    }
    public function Shields() {
        var_dump(__CLASS__ . "->" . __FUNCTION__);
    }
}

$T805 = new T805();
$T805->Weapon();

echo "<hr />";

$T806 = new T806();
$T806->Weapon();
$T806->Health();
$T806->Shields();

/* Output:
string(4) "T805"
<hr />string(4) "T805"
string(12) "T806::Health"
string(13) "T806->Shields"
*/

?>
```
<br><br>This works as well, but the output is different. I&apos;d be careful with this.  

---

PHP prevents interface a contant to be overridden by a class/interface that DIRECTLY inherits it.  However, further inheritance allows it.  That means that interface constants are not final as mentioned in a previous comment.  Is this a bug or a feature?<br><br>

```
<?php

interface a
{
    const b = 'Interface constant';
}

// Prints: Interface constant
echo a::b;

class b implements a
{
}

// This works!!!
class c extends b
{
    const b = 'Class constant';
}

echo c::b;
?>
```
  

---

The statement, that you have to implement _all_ methods of an interface has not to be taken that seriously, at least if you declare an abstract class and want to force the inheriting subclasses to implement the interface.<br>Just leave out all methods that should be implemented by the subclasses. But never write something like this:<br><br>

```
<?php

interface Foo {

      function bar();

}

abstract class FooBar implements Foo {

       abstract function bar(); // just for making clear, that this
                                 // method has to be implemented

}

?>
```
<br><br>This will end up with the following error-message:<br><br>Fatal error: Can&apos;t inherit abstract function Foo::bar() (previously declared abstract in FooBar) in path/to/file on line anylinenumber  

---

Interfaces can define static methods, but note that this won&apos;t make sense as you&apos;ll be using the class name and not polymorphism.<br><br>...Unless you have PHP 5.3 which supports late static binding:<br><br>

```
<?php

interface IDoSomething {
    public static function doSomething();
}

class One implements IDoSomething {
    public static function doSomething() {
        echo "One is doing something\n";
    }
}

class Two extends One {
    public static function doSomething() {
        echo "Two is doing something\n";
    }
}

function example(IDoSomething $doer) {
    $doer::doSomething(); // "unexpected ::" in PHP 5.2
}

example(new One()); // One is doing something
example(new Two()); // Two is doing something

?>
```
<br><br>If you have PHP 5.2 you can still declare static methods in interfaces. While you won&apos;t be able to call them via LSB, the "implements IDoSomething" can serve as a hint/reminder to other developers by saying "this class has a ::doSomething() method".<br>Besides, you&apos;ll be upgrading to 5.3 soon, right? Right?<br><br>(Heh. I just realized: "I do something". Unintentional, I swear!)  

---

What is not mentioned in the manual is that you can use "self" to force object hinting on a method of the implementing class:<br><br>Consider the following interface:<br>

```
<?php
interface Comparable
{function compare(self $compare);}
?>
```


Which is then implemented:



```
<?php

class String implements Comparable
{
    private $string;
    function __construct($string)
    {$this->string = $string;}
    function compare(self $compare)
    {return $this->string == $compare->string;}
}

class Integer implements Comparable
{
    private $integer;
    function __construct($int)
    {$this->integer = $int;}
    function compare(self $compare)
    {return $this->integer == $compare->integer;}
}

?>
```


Comparing Integer with String will result in a fatal error, as it is not an instance of the same class:



```
<?php
$first_int = new Integer(3);
$second_int = new Integer(3);
$first_string = new String("foo");
$second_string = new String("bar");

var_dump($first_int->compare($second_int)); // bool(true)
var_dump($first_string->compare($second_string)); // bool(false)
var_dump($first_string->compare($second_int)); // Fatal Error
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/language.oop5.interfaces.php)

**[To root](/README.md)**
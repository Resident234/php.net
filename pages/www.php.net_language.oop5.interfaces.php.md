# Object Interfaces





It seems like many contributors are missing the point of using an INTERFACE. An INTERFACE is not specifically provided for abstraction. That&apos;s what a CLASS is used for. Most examples in this article of interfaces could be achieved just as easily using just classes alone. 

An INTERFACE is provided so you can describe a set of functions and then hide the final implementation of those functions in an implementing class. This allows you to change the IMPLEMENTATION of those functions without changing how you use it. 

For example: I have a database. I want to write a class that accesses the data in my database. I define an interface like this:

interface Database {
function listOrders();
function addOrder();
function removeOrder();
...
}

Then let&apos;s say we start out using a MySQL database. So we write a class to access the MySQL database:

class MySqlDatabase implements Database {
function listOrders() {...
}
we write these methods as needed to get to the MySQL database tables. Then you can write your controller to use the interface as such:

$database = new MySqlDatabase();
foreach ($database-&gt;listOrders() as $order) {

Then let&apos;s say we decide to migrate to an Oracle database. We could write another class to get to the Oracle database as such:

class OracleDatabase implements Database {
public function listOrders() {...
}

Then - to switch our application to use the Oracle database instead of the MySQL database we only have to change ONE LINE of code:

$database = new OracleDatabase();

all other lines of code, such as:

foreach ($database-&gt;listOrders() as $order) {

will remain unchanged. The point is - the INTERFACE describes the methods that we need to access our database. It does NOT describe in any way HOW we achieve that. That&apos;s what the IMPLEMENTing class does. We can IMPLEMENT this interface as many times as we need in as many different ways as we need. We can then switch between implementations of the interface without impact to our code because the interface defines how we will use it regardless of how it actually works.

  

#



Just wrote some examples of duck-typing in PHP. Sharing here.



```
<?php

/**
 * An example of duck typing in PHP
 */

interface CanFly {
&#xA0; public function fly();
}

interface CanSwim {
&#xA0; public function swim();
}

class Bird {
&#xA0; public function info() {
&#xA0; &#xA0; echo &quot;I am a {$this-&gt;name}\n&quot;;
&#xA0; &#xA0; echo &quot;I am an bird\n&quot;;
&#xA0; }
}

/**
 * some implementations of birds
 */
class Dove extends Bird implements CanFly {
&#xA0; var $name = &quot;Dove&quot;;
&#xA0; public function fly() {
&#xA0; &#xA0; echo &quot;I fly\n&quot;;
&#xA0; } 
}

class Penguin extends Bird implements CanSwim {
&#xA0; var $name = &quot;Penguin&quot;;
&#xA0; public function swim() {
&#xA0; &#xA0; echo &quot;I swim\n&quot;;
&#xA0; } 
}

class Duck extends Bird implements CanFly, CanSwim {
&#xA0; var $name = &quot;Duck&quot;;
&#xA0; public function fly() {
&#xA0; &#xA0; echo &quot;I fly\n&quot;;
&#xA0; }
&#xA0; public function swim() {
&#xA0; &#xA0; echo &quot;I swim\n&quot;;
&#xA0; }
}

/**
 * a simple function to describe a bird
 */
function describe($bird) {
&#xA0; if ($bird instanceof Bird) {
&#xA0; &#xA0; $bird-&gt;info();
&#xA0; &#xA0; if ($bird instanceof CanFly) {
&#xA0; &#xA0; &#xA0; $bird-&gt;fly();
&#xA0; &#xA0; }
&#xA0; &#xA0; if ($bird instanceof CanSwim) {
&#xA0; &#xA0; &#xA0; $bird-&gt;swim();
&#xA0; &#xA0; }
&#xA0; } else {
&#xA0; &#xA0; die(&quot;This is not a bird. I cannot describe it.&quot;);
&#xA0; }
}

// describe these birds please
describe(new Penguin);
echo &quot;---\n&quot;;

describe(new Dove);
echo &quot;---\n&quot;;

describe(new Duck);


  

#



If it&apos;s not already obvious, it&apos;s worth noticing that the parameters in the interface&apos;s method declaration do not have to have the same names as those in any of its implementations.

More significantly, default argument values may be supplied for interface method parameters, and they have to be if you want to use default argument values in the implemented classes:



```
<?php
interface isStuffable
{
&#xA0; &#xA0; public function getStuffed($ratio=0.5);
}

class Turkey implements isStuffable
{
&#xA0; &#xA0; public function getStuffed($stuffing=1)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // ....
&#xA0; &#xA0; }
}
?>
```


Note that not only do the parameters have different names ($ratio and $stuffing), but their default values are free to be different as well. There doesn&apos;t seem to be any purpose to the interface&apos;s default argument value except as a dummy placeholder to show that there is a default (a class implementing isStuffable will not be able to implement methods with the signatures getStuffed(), getStuffed($a), or getStuffed($a,$b)).

  

#



The class implementing the interface must use the exact same method signatures as are defined in the interface. Not doing so will result in a fatal error. -- this documentation page.

But, if you use default values in arguments in methods, fatal error is not follow:



```
<?php
interface myInterface {
&#xA0; &#xA0; public function __construct();
}

class concret implements myInterface {

&#xA0; &#xA0; public function __construct($arg=null)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; print_r(func_get_args());
&#xA0; &#xA0; }
}

$obj = new concret(123);
?>
```


Array ( [0] =&gt; 123 )

  

#



When should you use interfaces?&#xA0; What are they good for? 
Here are two examples.&#xA0; 

1. Interfaces are an excellent way to implement reusability.&#xA0; 
You can create a general interface for a number of situations 
(such as a save to/load from disk interface.)&#xA0; You can then 
implement the interface in a variety of different ways (e.g. for 
formats such as tab delimited ASCII, XML and a database.)&#xA0; 
You can write code that asks the object to &quot;save itself to 
disk&quot; without having to worry what that means for the object 
in question.&#xA0; One object might save itself to the database, 
another to an XML and you can change this behavior over 
time without having to rewrite the calling code.&#xA0; 

This allows you to write reusable calling code that can work 
for any number of different objects -- you don&apos;t need to know 
what kind of object it is, as long as it obeys the common 
interface.&#xA0; 

2. Interfaces can also promote gradual evolution.&#xA0; On a 
recent project I had some very complicated work to do and I 
didn&apos;t know how to implement it.&#xA0; I could think of a &quot;basic&quot; 
implementation but I knew I would have to change it later.&#xA0; 
So I created interfaces in each of these cases, and created 
at least one &quot;basic&quot; implementation of the interface that 
was &quot;good enough for now&quot; even though I knew it would have 
to change later.&#xA0; 

When I came back to make the changes, I was able to create 
some new implementations of these interfaces that added the 
extra features I needed.&#xA0; Some of my classes still used 
the &quot;basic&quot; implementations, but others needed the 
specialized ones.&#xA0; I was able to add the new features to the 
objects themselves without rewriting the calling code in most 
cases.&#xA0; It was easy to evolve my code in this way because 
the changes were mostly isolated -- they didn&apos;t spread all 
over the place like you might expect.

  

#



Implementation must be strict, subtypes are not allowed.

&quot;The class implementing the interface must use the EXACT SAME METHOD SIGNATURES as are defined in the interface. Not doing so will result in a fatal error. &quot;



```
<?php

interface I
{
&#xA0; function foo(stdClass $arg);
}

class Test extends stdClass
{
}

class Implementation implements I
{
&#xA0; function foo(Test $arg)
&#xA0; &#xA0; {
&#xA0; &#xA0; }
}
?>
```

Result:

Fatal error: Declaration of InterfaceImplementation::foo() must be compatible with I::foo(stdClass $arg) in test.php on line XY

  

#



On an incidental note, it is not necessary for the implementation of an interface method to use the same variable names for its parameters that were used in the interface declaration.

More significantly, your interface method declarations can include default argument values. If you do, you must specify their implementations with default arguments, too. Just like the parameter names, the default argument values do not need to be the same. In fact, there doesn&apos;t seem to be any functionality to the one in the interface declaration at all beyond the fact that it is there.



```
<?php
interface isStuffed {
&#xA0; &#xA0; public function getStuff($something=17);
}

class oof implements isStuffed {
&#xA0; &#xA0; public function getStuff($a=42) {
&#xA0; &#xA0; &#xA0; &#xA0; return $a;
&#xA0; &#xA0; }
}

$oof = new oof;

echo $oof-&gt;getStuff();
?>
```


Implementations that try to declare the method as getStuff(), getStuff($a), or getStuff($a,$b) will all trigger a fatal error.

  

#



I was wondering if implementing interfaces will take into account inheritance. That is, can inherited methods be used to follow an interface&apos;s structure?



```
<?php

interface Auxiliary_Platform {
&#xA0; &#xA0; public function Weapon();
&#xA0; &#xA0; public function Health();
&#xA0; &#xA0; public function Shields();
}

class T805 implements Auxiliary_Platform {
&#xA0; &#xA0; public function Weapon() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__CLASS__);
&#xA0; &#xA0; }
&#xA0; &#xA0; public function Health() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__CLASS__ . &quot;::&quot; . __FUNCTION__);
&#xA0; &#xA0; }
&#xA0; &#xA0; public function Shields() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__CLASS__ . &quot;-&gt;&quot; . __FUNCTION__);
&#xA0; &#xA0; }
}

class T806 extends T805 implements Auxiliary_Platform {
&#xA0; &#xA0; public function Weapon() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__CLASS__);
&#xA0; &#xA0; }
&#xA0; &#xA0; public function Shields() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__CLASS__ . &quot;-&gt;&quot; . __FUNCTION__);
&#xA0; &#xA0; }
}

$T805 = new T805();
$T805-&gt;Weapon();
$T805-&gt;Health();
$T805-&gt;Shields();

echo &quot;&lt;hr /&gt;&quot;;

$T806 = new T806();
$T806-&gt;Weapon();
$T806-&gt;Health();
$T806-&gt;Shields();

/* Output:
string(4) &quot;T805&quot;
string(12) &quot;T805::Health&quot;
string(13) &quot;T805-&gt;Shields&quot;
&lt;hr /&gt;string(4) &quot;T806&quot;
string(12) &quot;T805::Health&quot;
string(13) &quot;T806-&gt;Shields&quot;
*/

?>
```


Class T805 implements the interface Auxiliary_Platform. T806 does the same thing, but the method Health() is inherited from T805 (not the exact case, but you get the idea). PHP seems to be fine with this and everything still works fine. Do note that the rules for class inheritance doesn&apos;t change in this scenario.

If the code were to be the same, but instead T805 (or T806) DOES NOT implement Auxiliary_Platform, then it&apos;ll still work. Since T805 already follows the interface, everything that inherits T805 will also be valid. I would be careful about that. Personally, I don&apos;t consider this a bug.

This seems to work in PHP5.2.9-2, PHP5.3 and PHP5.3.1 (my current versions).

We could also do the opposite:



```
<?php

class T805 {
&#xA0; &#xA0; public function Weapon() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__CLASS__);
&#xA0; &#xA0; }
}

class T806 extends T805 implements Auxiliary_Platform {
&#xA0; &#xA0; public function Health() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__CLASS__ . &quot;::&quot; . __FUNCTION__);
&#xA0; &#xA0; }
&#xA0; &#xA0; public function Shields() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(__CLASS__ . &quot;-&gt;&quot; . __FUNCTION__);
&#xA0; &#xA0; }
}

$T805 = new T805();
$T805-&gt;Weapon();

echo &quot;&lt;hr /&gt;&quot;;

$T806 = new T806();
$T806-&gt;Weapon();
$T806-&gt;Health();
$T806-&gt;Shields();

/* Output:
string(4) &quot;T805&quot;
&lt;hr /&gt;string(4) &quot;T805&quot;
string(12) &quot;T806::Health&quot;
string(13) &quot;T806-&gt;Shields&quot;
*/

?>
```


This works as well, but the output is different. I&apos;d be careful with this.

  

#



PHP prevents interface a contant to be overridden by a class/interface that DIRECTLY inherits it.&#xA0; However, further inheritance allows it.&#xA0; That means that interface constants are not final as mentioned in a previous comment.&#xA0; Is this a bug or a feature?



```
<?php

interface a
{
&#xA0; &#xA0; const b = &apos;Interface constant&apos;;
}

// Prints: Interface constant
echo a::b;

class b implements a
{
}

// This works!!!
class c extends b
{
&#xA0; &#xA0; const b = &apos;Class constant&apos;;
}

echo c::b;
?>
```



  

#



The statement, that you have to implement _all_ methods of an interface has not to be taken that seriously, at least if you declare an abstract class and want to force the inheriting subclasses to implement the interface.
Just leave out all methods that should be implemented by the subclasses. But never write something like this:



```
<?php

interface Foo {

&#xA0; &#xA0; &#xA0; function bar();

}

abstract class FooBar implements Foo {

&#xA0; &#xA0; &#xA0;&#xA0; abstract function bar(); // just for making clear, that this
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // method has to be implemented

}

?>
```


This will end up with the following error-message:

Fatal error: Can&apos;t inherit abstract function Foo::bar() (previously declared abstract in FooBar) in path/to/file on line anylinenumber

  

#



Interfaces can define static methods, but note that this won&apos;t make sense as you&apos;ll be using the class name and not polymorphism.

...Unless you have PHP 5.3 which supports late static binding:



```
<?php

interface IDoSomething {
&#xA0; &#xA0; public static function doSomething();
}

class One implements IDoSomething {
&#xA0; &#xA0; public static function doSomething() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;One is doing something\n&quot;;
&#xA0; &#xA0; }
}

class Two extends One {
&#xA0; &#xA0; public static function doSomething() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Two is doing something\n&quot;;
&#xA0; &#xA0; }
}

function example(IDoSomething $doer) {
&#xA0; &#xA0; $doer::doSomething(); // &quot;unexpected ::&quot; in PHP 5.2
}

example(new One()); // One is doing something
example(new Two()); // Two is doing something

?>
```


If you have PHP 5.2 you can still declare static methods in interfaces. While you won&apos;t be able to call them via LSB, the &quot;implements IDoSomething&quot; can serve as a hint/reminder to other developers by saying &quot;this class has a ::doSomething() method&quot;.
Besides, you&apos;ll be upgrading to 5.3 soon, right? Right?

(Heh. I just realized: &quot;I do something&quot;. Unintentional, I swear!)

  

#



What is not mentioned in the manual is that you can use &quot;self&quot; to force object hinting on a method of the implementing class:

Consider the following interface:


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
&#xA0; &#xA0; private $string;
&#xA0; &#xA0; function __construct($string)
&#xA0; &#xA0; {$this-&gt;string = $string;}
&#xA0; &#xA0; function compare(self $compare)
&#xA0; &#xA0; {return $this-&gt;string == $compare-&gt;string;}
}

class Integer implements Comparable
{
&#xA0; &#xA0; private $integer;
&#xA0; &#xA0; function __construct($int)
&#xA0; &#xA0; {$this-&gt;integer = $int;}
&#xA0; &#xA0; function compare(self $compare)
&#xA0; &#xA0; {return $this-&gt;integer == $compare-&gt;integer;}
}

?>
```


Comparing Integer with String will result in a fatal error, as it is not an instance of the same class:



```
<?php
$first_int = new Integer(3);
$second_int = new Integer(3);
$first_string = new String(&quot;foo&quot;);
$second_string = new String(&quot;bar&quot;);

var_dump($first_int-&gt;compare($second_int)); // bool(true)
var_dump($first_string-&gt;compare($second_string)); // bool(false)
var_dump($first_string-&gt;compare($second_int)); // Fatal Error
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.interfaces.php)

**[To root](/README.md)**
# Constructors and Destructors





the easiest way to use and understand multiple constructors:





```
<?php

class A

{

&#xA0; &#xA0; function __construct()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; $a = func_get_args();

&#xA0; &#xA0; &#xA0; &#xA0; $i = func_num_args();

&#xA0; &#xA0; &#xA0; &#xA0; if (method_exists($this,$f=&apos;__construct&apos;.$i)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; call_user_func_array(array($this,$f),$a);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; function __construct1($a1)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; echo(&apos;__construct with 1 param called: &apos;.$a1.PHP_EOL);

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; function __construct2($a1,$a2)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; echo(&apos;__construct with 2 params called: &apos;.$a1.&apos;,&apos;.$a2.PHP_EOL);

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; function __construct3($a1,$a2,$a3)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; echo(&apos;__construct with 3 params called: &apos;.$a1.&apos;,&apos;.$a2.&apos;,&apos;.$a3.PHP_EOL);

&#xA0; &#xA0; }

}

$o = new A(&apos;sheep&apos;);

$o = new A(&apos;sheep&apos;,&apos;cat&apos;);

$o = new A(&apos;sheep&apos;,&apos;cat&apos;,&apos;dog&apos;);



// results:

// __construct with 1 param called: sheep

// __construct with 2 params called: sheep,cat

// __construct with 3 params called: sheep,cat,dog

?>
```



  

#



Be aware of potential memory leaks caused by circular references within objects.&#xA0; The PHP manual states &quot;[t]he destructor method will be called as soon as all references to a particular object are removed&quot; and this is precisely true: if two objects reference each other (or even if one object has a field that points to itself as in $this-&gt;foo = $this) then this reference will prevent the destructor being called even when there are no other references to the object at all.&#xA0; The programmer can no longer access the objects, but they still stay in memory.

Consider the following example:



```
<?php

header(&quot;Content-type: text/plain&quot;);

class Foo {
&#xA0; &#xA0; 
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * An indentifier
&#xA0; &#xA0;&#xA0; * @var string 
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; private $name;
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * A reference to another Foo object
&#xA0; &#xA0;&#xA0; * @var Foo
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; private $link;

&#xA0; &#xA0; public function __construct($name) {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;name = $name;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function setLink(Foo $link){
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;link = $link;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function __destruct() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Destroying: &apos;, $this-&gt;name, PHP_EOL;
&#xA0; &#xA0; }
}

// create two Foo objects:
$foo = new Foo(&apos;Foo 1&apos;);
$bar = new Foo(&apos;Foo 2&apos;);

// make them point to each other
$foo-&gt;setLink($bar);
$bar-&gt;setLink($foo);

// destroy the global references to them
$foo = null;
$bar = null;

// we now have no way to access Foo 1 or Foo 2, so they OUGHT to be __destruct()ed
// but they are not, so we get a memory leak as they are still in memory.
//
// Uncomment the next line to see the difference when explicitly calling the GC:
// gc_collect_cycles();
// 
// see also: http://www.php.net/manual/en/features.gc.php
// 

// create two more Foo objects, but DO NOT set their internal Foo references
// so nothing except the vars $foo and $bar point to them:
$foo = new Foo(&apos;Foo 3&apos;);
$bar = new Foo(&apos;Foo 4&apos;);

// destroy the global references to them
$foo = null;
$bar = null;

// we now have no way to access Foo 3 or Foo 4 and as there are no more references
// to them anywhere, their __destruct() methods are automatically called here,
// BEFORE the next line is executed:

echo &apos;End of script&apos;, PHP_EOL;

?>
```


This will output:

Destroying: Foo 3
Destroying: Foo 4
End of script
Destroying: Foo 1
Destroying: Foo 2

But if we uncomment the gc_collect_cycles(); function call in the middle of the script, we get:

Destroying: Foo 2
Destroying: Foo 1
Destroying: Foo 3
Destroying: Foo 4
End of script

As may be desired.

NOTE: calling gc_collect_cycles() does have a speed overhead, so only use it if you feel you need to.

  

#



The __destruct magic method must be public. 

public function __destruct()
{
&#xA0; &#xA0; ;
}

The method will automatically be called externally to the instance.&#xA0; Declaring __destruct as protected or private will result in a warning and the magic method will not be called. 

Note: In PHP 5.3.10 i saw strange side effects while some Destructors were declared as protected.

  

#



USE PARENT::CONSTRUCT() to exploit POLYMORPHISM POWERS

Since we are still in the __construct and __destruct section, alot of emphasis has been on __destruct - which I know nothing about. But I would like to show the power of parent::__construct for use with PHP&apos;s OOP polymorphic behavior (you&apos;ll see what this is very quickly).

In my example, I have created a fairly robust base class that does everything that all subclasses need to do. Here&apos;s the base class def.



```
<?php

/*
 * Animal.php
 *
 * This class holds all data, and defines all functions that all 
 * subclass extensions need to use.
 *
 */
abstract class Animal
{
&#xA0; public $type;
&#xA0; public $name;
&#xA0; public $sound;

&#xA0; /*
&#xA0;&#xA0; * called by Dog, Cat, Bird, etc.
&#xA0;&#xA0; */
&#xA0; public function __construct($aType, $aName, $aSound)
&#xA0; {
&#xA0; &#xA0; $this-&gt;type = $aType;
&#xA0; &#xA0; $this-&gt;name = $aName;
&#xA0; &#xA0; $this-&gt;sound = $aSound;
&#xA0; }

&#xA0; /*
&#xA0;&#xA0; * define the sorting rules - we will sort all Animals by name.
&#xA0;&#xA0; */ 
&#xA0; public static function compare($a, $b)
&#xA0; {
&#xA0; &#xA0; if($a-&gt;name &lt; $b-&gt;name) return -1;
&#xA0; &#xA0; else if($a-&gt;name == $b-&gt;name) return 0;
&#xA0; &#xA0; else return 1;
&#xA0; }

&#xA0; /*
&#xA0;&#xA0; * a String representation for all Animals.
&#xA0;&#xA0; */
&#xA0; public function __toString()
&#xA0; {
&#xA0; &#xA0; return &quot;$this-&gt;name the $this-&gt;type goes $this-&gt;sound&quot;;
&#xA0; }
}

?>
```


Trying to instantiate an object of type Animal will not work...

$myPet = new Animal(&quot;Parrot&quot;, &quot;Captain Jack&quot;, &quot;Kaaawww!&quot;); // throws Fatal Error: cannot instantiate abstract class Animal.

Declaring Animal as abstract is like killing two birds with one stone. 1. We stop it from being instantiated - which means we do not need a private __construct() or a static getInstance() method, and 2. We can use it for polymorphic behavior. In our case here, that means &quot;__construct&quot;, &quot;__toString&quot; and &quot;compare&quot; will be called for all subclasses of Animal that have not defined their own implementations.

The following subclasses use parent::__construct(), which sends all new data to Animal. Our Animal class stores this data and defines functions for polymorphism to work... and the best part is, it keeps our subclass defs super short and even sweeter.



```
<?php

class Dog extends Animal{
&#xA0; public function __construct($name){
&#xA0; &#xA0; parent::__construct(&quot;Dog&quot;, $name, &quot;woof!&quot;);
&#xA0; }
}

class Cat extends Animal{
&#xA0; public function __construct($name){
&#xA0; &#xA0; parent::__construct(&quot;Cat&quot;, $name, &quot;meeoow!&quot;);
&#xA0; }
}

class Bird extends Animal{
&#xA0; public function __construct($name){
&#xA0; &#xA0; parent::__construct(&quot;Bird&quot;, $name, &quot;chirp chirp!!&quot;);
&#xA0; }
}

# create a PHP Array and initialize it with Animal objects
$animals = array(
&#xA0; new Dog(&quot;Fido&quot;),
&#xA0; new Bird(&quot;Celeste&quot;),
&#xA0; new Cat(&quot;Pussy&quot;),
&#xA0; new Dog(&quot;Brad&quot;),
&#xA0; new Bird(&quot;Kiki&quot;),
&#xA0; new Cat(&quot;Abraham&quot;),
&#xA0; new Dog(&quot;Jawbone&quot;)
);

# sort $animals with PHP&apos;s usort - calls Animal::compare() many many times.
usort($animals, array(&quot;Animal&quot;, &quot;compare&quot;));

# print out the sorted results - calls Animal-&gt;__toString().
foreach($animals as $animal) echo &quot;$animal&lt;br&gt;\n&quot;;

?>
```


The results are &quot;sorted by name&quot; and &quot;printed&quot; by the Animal class:

Abraham the Cat goes meeoow!
Brad the Dog goes woof!
Celeste the Bird goes chirp chirp!!
Fido the Dog goes woof!
Jawbone the Dog goes woof!
Kiki the Bird goes chirp chirp!!
Pussy the Cat goes meeoow!

Using parent::__construct() in a subclass and a super smart base class, gives your child objects a headstart in life, by alleviating them from having to define or handle several error and exception routines that they have no control over.

Notice how subclass definitions are really short - no variables or functions at all, and there is no private __construct() method anywhere? Notice how objects of type Dog, Cat, and Bird are all sorted by our base class Animal? All the class definitions above address several issues (keeping objects from being instantiated) and enforces the desired, consistent, and reliable behavior everytime... with the least amount of code. In addition, new extenstions can easily be created. Each subclass is now super easy to redefine or even extend... now that you can see a way to do it.

  

#



Correction to the previous poster about non public constructors. If I wanted to implement Singleton design pattern where I would only want one instance of the class I would want to prevent instantiation of the class from outside of the class by making the constructor private. An example follows:

class Foo {

&#xA0; private static $instance;

&#xA0; private __construct() {
&#xA0; &#xA0; // Do stuff
&#xA0; }

&#xA0; public static getInstance() {

&#xA0; &#xA0; if (!isset(self::$instance)) {
&#xA0; &#xA0; &#xA0; $c = __CLASS__;
&#xA0; &#xA0; &#xA0; $instance = new $c;
&#xA0; &#xA0; }

&#xA0; &#xA0; return self::$instance;
&#xA0; }

&#xA0; public function sayHello() {
&#xA0; &#xA0; echo &quot;Hello World!!&quot;;
&#xA0; }

}

$bar = Foo::getInstance();

// Prints &apos;Hello World&apos; on the screen.
$bar -&gt; sayHello();

  

#



It&apos;s always the easy things that get you -

Being new to OOP, it took me quite a while to figure out that there are TWO underscores in front of the word __construct.

It is __construct
Not _construct

Extremely obvious once you figure it out, but it can be sooo frustrating until you do.

I spent quite a bit of needless time debugging working code.

I even thought about it a few times, thinking it looked a little long in the examples, but at the time that just seemed silly(always thinking &quot;oh somebody would have made that clear if it weren&apos;t just a regular underscore...&quot;)

All the manuals I looked at, all the tuturials I read, all the examples I browsed through&#xA0; - not once did anybody mention this!&#xA0; 

(please don&apos;t tell me it&apos;s explained somewhere on this page and I just missed it,&#xA0; you&apos;ll only add to my pain.)

I hope this helps somebody else!

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.decon.php)

**[To root](/README.md)**
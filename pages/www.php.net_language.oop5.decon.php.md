# Constructors and Destructors



the easiest way to use and understand multiple constructors:<br><br>

```
<?php
class A
{
    function __construct()
    {
        $a = func_get_args();
        $i = func_num_args();
        if (method_exists($this,$f=&apos;__construct&apos;.$i)) {
            call_user_func_array(array($this,$f),$a);
        }
    }
    
    function __construct1($a1)
    {
        echo(&apos;__construct with 1 param called: &apos;.$a1.PHP_EOL);
    }
    
    function __construct2($a1,$a2)
    {
        echo(&apos;__construct with 2 params called: &apos;.$a1.&apos;,&apos;.$a2.PHP_EOL);
    }
    
    function __construct3($a1,$a2,$a3)
    {
        echo(&apos;__construct with 3 params called: &apos;.$a1.&apos;,&apos;.$a2.&apos;,&apos;.$a3.PHP_EOL);
    }
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

Be aware of potential memory leaks caused by circular references within objects.  The PHP manual states "[t]he destructor method will be called as soon as all references to a particular object are removed" and this is precisely true: if two objects reference each other (or even if one object has a field that points to itself as in $this-&gt;foo = $this) then this reference will prevent the destructor being called even when there are no other references to the object at all.  The programmer can no longer access the objects, but they still stay in memory.<br><br>Consider the following example:<br><br>

```
<?php

header("Content-type: text/plain");

class Foo {
    
    /**
     * An indentifier
     * @var string 
     */
    private $name;
    /**
     * A reference to another Foo object
     * @var Foo
     */
    private $link;

    public function __construct($name) {
        $this-&gt;name = $name;
    }

    public function setLink(Foo $link){
        $this-&gt;link = $link;
    }

    public function __destruct() {
        echo &apos;Destroying: &apos;, $this-&gt;name, PHP_EOL;
    }
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
<br><br>This will output:<br><br>Destroying: Foo 3<br>Destroying: Foo 4<br>End of script<br>Destroying: Foo 1<br>Destroying: Foo 2<br><br>But if we uncomment the gc_collect_cycles(); function call in the middle of the script, we get:<br><br>Destroying: Foo 2<br>Destroying: Foo 1<br>Destroying: Foo 3<br>Destroying: Foo 4<br>End of script<br><br>As may be desired.<br><br>NOTE: calling gc_collect_cycles() does have a speed overhead, so only use it if you feel you need to.  

#

The __destruct magic method must be public. <br><br>public function __destruct()<br>{<br>    ;<br>}<br><br>The method will automatically be called externally to the instance.  Declaring __destruct as protected or private will result in a warning and the magic method will not be called. <br><br>Note: In PHP 5.3.10 i saw strange side effects while some Destructors were declared as protected.  

#

USE PARENT::CONSTRUCT() to exploit POLYMORPHISM POWERS<br><br>Since we are still in the __construct and __destruct section, alot of emphasis has been on __destruct - which I know nothing about. But I would like to show the power of parent::__construct for use with PHP&apos;s OOP polymorphic behavior (you&apos;ll see what this is very quickly).<br><br>In my example, I have created a fairly robust base class that does everything that all subclasses need to do. Here&apos;s the base class def.<br><br>

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
  public $type;
  public $name;
  public $sound;

  /*
   * called by Dog, Cat, Bird, etc.
   */
  public function __construct($aType, $aName, $aSound)
  {
    $this-&gt;type = $aType;
    $this-&gt;name = $aName;
    $this-&gt;sound = $aSound;
  }

  /*
   * define the sorting rules - we will sort all Animals by name.
   */ 
  public static function compare($a, $b)
  {
    if($a-&gt;name &lt; $b-&gt;name) return -1;
    else if($a-&gt;name == $b-&gt;name) return 0;
    else return 1;
  }

  /*
   * a String representation for all Animals.
   */
  public function __toString()
  {
    return "$this-&gt;name the $this-&gt;type goes $this-&gt;sound";
  }
}

?>
```


Trying to instantiate an object of type Animal will not work...

$myPet = new Animal("Parrot", "Captain Jack", "Kaaawww!"); // throws Fatal Error: cannot instantiate abstract class Animal.

Declaring Animal as abstract is like killing two birds with one stone. 1. We stop it from being instantiated - which means we do not need a private __construct() or a static getInstance() method, and 2. We can use it for polymorphic behavior. In our case here, that means "__construct", "__toString" and "compare" will be called for all subclasses of Animal that have not defined their own implementations.

The following subclasses use parent::__construct(), which sends all new data to Animal. Our Animal class stores this data and defines functions for polymorphism to work... and the best part is, it keeps our subclass defs super short and even sweeter.



```
<?php

class Dog extends Animal{
  public function __construct($name){
    parent::__construct("Dog", $name, "woof!");
  }
}

class Cat extends Animal{
  public function __construct($name){
    parent::__construct("Cat", $name, "meeoow!");
  }
}

class Bird extends Animal{
  public function __construct($name){
    parent::__construct("Bird", $name, "chirp chirp!!");
  }
}

# create a PHP Array and initialize it with Animal objects
$animals = array(
  new Dog("Fido"),
  new Bird("Celeste"),
  new Cat("Pussy"),
  new Dog("Brad"),
  new Bird("Kiki"),
  new Cat("Abraham"),
  new Dog("Jawbone")
);

# sort $animals with PHP&apos;s usort - calls Animal::compare() many many times.
usort($animals, array("Animal", "compare"));

# print out the sorted results - calls Animal-&gt;__toString().
foreach($animals as $animal) echo "$animal&lt;br&gt;\n";

?>
```
<br><br>The results are "sorted by name" and "printed" by the Animal class:<br><br>Abraham the Cat goes meeoow!<br>Brad the Dog goes woof!<br>Celeste the Bird goes chirp chirp!!<br>Fido the Dog goes woof!<br>Jawbone the Dog goes woof!<br>Kiki the Bird goes chirp chirp!!<br>Pussy the Cat goes meeoow!<br><br>Using parent::__construct() in a subclass and a super smart base class, gives your child objects a headstart in life, by alleviating them from having to define or handle several error and exception routines that they have no control over.<br><br>Notice how subclass definitions are really short - no variables or functions at all, and there is no private __construct() method anywhere? Notice how objects of type Dog, Cat, and Bird are all sorted by our base class Animal? All the class definitions above address several issues (keeping objects from being instantiated) and enforces the desired, consistent, and reliable behavior everytime... with the least amount of code. In addition, new extenstions can easily be created. Each subclass is now super easy to redefine or even extend... now that you can see a way to do it.  

#

Correction to the previous poster about non public constructors. If I wanted to implement Singleton design pattern where I would only want one instance of the class I would want to prevent instantiation of the class from outside of the class by making the constructor private. An example follows:<br><br>class Foo {<br><br>  private static $instance;<br><br>  private __construct() {<br>    // Do stuff<br>  }<br><br>  public static getInstance() {<br><br>    if (!isset(self::$instance)) {<br>      $c = __CLASS__;<br>      $instance = new $c;<br>    }<br><br>    return self::$instance;<br>  }<br><br>  public function sayHello() {<br>    echo "Hello World!!";<br>  }<br><br>}<br><br>$bar = Foo::getInstance();<br><br>// Prints &apos;Hello World&apos; on the screen.<br>$bar -&gt; sayHello();  

#

It&apos;s always the easy things that get you -<br><br>Being new to OOP, it took me quite a while to figure out that there are TWO underscores in front of the word __construct.<br><br>It is __construct<br>Not _construct<br><br>Extremely obvious once you figure it out, but it can be sooo frustrating until you do.<br><br>I spent quite a bit of needless time debugging working code.<br><br>I even thought about it a few times, thinking it looked a little long in the examples, but at the time that just seemed silly(always thinking "oh somebody would have made that clear if it weren&apos;t just a regular underscore...")<br><br>All the manuals I looked at, all the tuturials I read, all the examples I browsed through  - not once did anybody mention this!  <br><br>(please don&apos;t tell me it&apos;s explained somewhere on this page and I just missed it,  you&apos;ll only add to my pain.)<br><br>I hope this helps somebody else!  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.decon.php)

**[To root](/README.md)**
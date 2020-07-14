# The Basics



I was confused at first about object assignment, because it&apos;s not quite the same as normal assignment or assignment by reference. But I think I&apos;ve figured out what&apos;s going on.<br><br>First, think of variables in PHP as data slots. Each one is a name that points to a data slot that can hold a value that is one of the basic data types: a number, a string, a boolean, etc. When you create a reference, you are making a second name that points at the same data slot. When you assign one variable to another, you are copying the contents of one data slot to another data slot.<br><br>Now, the trick is that object instances are not like the basic data types. They cannot be held in the data slots directly. Instead, an object&apos;s "handle" goes in the data slot. This is an identifier that points at one particular instance of an obect. So, the object handle, although not directly visible to the programmer, is one of the basic datatypes. <br><br>What makes this tricky is that when you take a variable which holds an object handle, and you assign it to another variable, that other variable gets a copy of the same object handle. This means that both variables can change the state of the same object instance. But they are not references, so if one of the variables is assigned a new value, it does not affect the other variable.<br><br>

```
<?php
// Assignment of an object
Class Object{
   public $foo="bar";
};

$objectVar = new Object();
$reference =&amp; $objectVar;
$assignment = $objectVar

//
// $objectVar ---&gt;+---------+
//                |(handle1)----+
// $reference ---&gt;+---------+   |
//                              |
//                +---------+   |
// $assignment --&gt;|(handle1)----+
//                +---------+   |
//                              |
//                              v
//                  Object(1):foo="bar"
//
?>
```


$assignment has a different data slot from $objectVar, but its data slot holds a handle to the same object. This makes it behave in some ways like a reference. If you use the variable $objectVar to change the state of the Object instance, those changes also show up under $assignment, because it is pointing at that same Object instance.



```
<?php
$objectVar-&gt;foo = "qux";
print_r( $objectVar );
print_r( $reference );
print_r( $assignment );

//
// $objectVar ---&gt;+---------+
//                |(handle1)----+
// $reference ---&gt;+---------+   |
//                              |
//                +---------+   |
// $assignment --&gt;|(handle1)----+
//                +---------+   |
//                              |
//                              v
//                  Object(1):foo="qux"
//
?>
```


But it is not exactly the same as a reference. If you null out $objectVar, you replace the handle in its data slot with NULL. This means that $reference, which points at the same data slot, will also be NULL. But $assignment, which is a different data slot, will still hold its copy of the handle to the Object instance, so it will not be NULL.



```
<?php
$objectVar = null;
print_r($objectVar);
print_r($reference);
print_r($assignment);

//
// $objectVar ---&gt;+---------+
//                |  NULL   | 
// $reference ---&gt;+---------+
//                           
//                +---------+
// $assignment --&gt;|(handle1)----+
//                +---------+   |
//                              |
//                              v
//                  Object(1):foo="qux"
?>
```
  

#

You start using :: in second example although the static concept has not been explained. This is not easy to discover when you are starting from the basics.  

#

What is the difference between  $this  and  self ?<br><br>Inside a class definition, $this refers to the current object, while  self  refers to the current class.<br><br>It is necessary to refer to a class element using  self ,<br>and refer to an object element using  $this .<br>Note also how an object variable must be preceded by a keyword in its definition.<br><br>The following example illustrates a few cases:<br><br>

```
<?php
class Classy {

const       STAT = &apos;S&apos; ; // no dollar sign for constants (they are always static)
static     $stat = &apos;Static&apos; ;
public     $publ = &apos;Public&apos; ;
private    $priv = &apos;Private&apos; ;
protected  $prot = &apos;Protected&apos; ;

function __construct( ){  }

public function showMe( ){
    print &apos;&lt;br&gt; self::STAT: &apos;  .  self::STAT ; // refer to a (static) constant like this
    print &apos;&lt;br&gt; self::$stat: &apos; . self::$stat ; // static variable
    print &apos;&lt;br&gt;$this-&gt;stat: &apos;  . $this-&gt;stat ; // legal, but not what you might think: empty result
    print &apos;&lt;br&gt;$this-&gt;publ: &apos;  . $this-&gt;publ ; // refer to an object variable like this
    print &apos;&lt;br&gt;&apos; ;
}
}
$me = new Classy( ) ;
$me-&gt;showMe( ) ;

/* Produces this output:
self::STAT: S
self::$stat: Static
$this-&gt;stat:
$this-&gt;publ: Public
*/
?>
```
  

#

CLASSES and OBJECTS that represent the "Ideal World"<br><br>Wouldn&apos;t it be great to get the lawn mowed by saying $son-&gt;mowLawn()? Assuming the function mowLawn() is defined, and you have a son that doesn&apos;t throw errors, the lawn will be mowed. <br><br>In the following example; let objects of type Line3D measure their own length in 3-dimensional space. Why should I or PHP have to provide another method from outside this class to calculate length, when the class itself holds all the neccessary data and has the education to make the calculation for itself?<br><br>

```
<?php

/*
 * Point3D.php
 *
 * Represents one locaton or position in 3-dimensional space
 * using an (x, y, z) coordinate system.
 */
class Point3D
{
    public $x;
    public $y;
    public $z;                  // the x coordinate of this Point.

    /*
     * use the x and y variables inherited from Point.php.
     */
    public function __construct($xCoord=0, $yCoord=0, $zCoord=0)
    {
        $this-&gt;x = $xCoord;
    $this-&gt;y = $yCoord;
        $this-&gt;z = $zCoord;
    }

    /*
     * the (String) representation of this Point as "Point3D(x, y, z)".
     */
    public function __toString()
    {
        return &apos;Point3D(x=&apos; . $this-&gt;x . &apos;, y=&apos; . $this-&gt;y . &apos;, z=&apos; . $this-&gt;z . &apos;)&apos;;
    }
}

/*
 * Line3D.php
 *
 * Represents one Line in 3-dimensional space using two Point3D objects.
 */
class Line3D
{
    $start;
    $end;

    public function __construct($xCoord1=0, $yCoord1=0, $zCoord1=0, $xCoord2=1, $yCoord2=1, $zCoord2=1)
    {
        $this-&gt;start = new Point3D($xCoord1, $yCoord1, $zCoord1);
        $this-&gt;end = new Point3D($xCoord2, $yCoord2, $zCoord2);
    }

    /*
     * calculate the length of this Line in 3-dimensional space.
     */ 
    public function getLength()
    {
        return sqrt(
            pow($this-&gt;start-&gt;x - $this-&gt;end-&gt;x, 2) +
            pow($this-&gt;start-&gt;y - $this-&gt;end-&gt;y, 2) +
            pow($this-&gt;start-&gt;z - $this-&gt;end-&gt;z, 2)
        );
    }

    /*
     * The (String) representation of this Line as "Line3D[start, end, length]".
     */
    public function __toString()
    {
        return &apos;Line3D[start=&apos; . $this-&gt;start .
            &apos;, end=&apos; . $this-&gt;end .
            &apos;, length=&apos; . $this-&gt;getLength() . &apos;]&apos;;
    }
}

/*
 * create and display objects of type Line3D.
 */
echo &apos;&lt;p&gt;&apos; . (new Line3D()) . "&lt;/p&gt;\n";
echo &apos;&lt;p&gt;&apos; . (new Line3D(0, 0, 0, 100, 100, 0)) . "&lt;/p&gt;\n";
echo &apos;&lt;p&gt;&apos; . (new Line3D(0, 0, 0, 100, 100, 100)) . "&lt;/p&gt;\n";

?>
```
<br><br>  &lt;--  The results look like this  --&gt;<br><br>Line3D[start=Point3D(x=0, y=0, z=0), end=Point3D(x=1, y=1, z=1), length=1.73205080757]<br><br>Line3D[start=Point3D(x=0, y=0, z=0), end=Point3D(x=100, y=100, z=0), length=141.421356237]<br><br>Line3D[start=Point3D(x=0, y=0, z=0), end=Point3D(x=100, y=100, z=100), length=173.205080757]<br><br>My absolute favorite thing about OOP is that "good" objects keep themselves in check. I mean really, it&apos;s the exact same thing in reality... like, if you hire a plumber to fix your kitchen sink, wouldn&apos;t you expect him to figure out the best plan of attack? Wouldn&apos;t he dislike the fact that you want to control the whole job? Wouldn&apos;t you expect him to not give you additional problems? And for god&apos;s sake, it is too much to ask that he cleans up before he leaves?<br><br>I say, design your classes well, so they can do their jobs uninterrupted... who like bad news? And, if your classes and objects are well defined, educated, and have all the necessary data to work on (like the examples above do), you won&apos;t have to micro-manage the whole program from outside of the class. In other words... create an object, and LET IT RIP!  

#

stdClass is the default PHP object. stdClass has no properties, methods or parent. It does not support magic methods, and implements no interfaces.<br><br>When you cast a scalar or array as Object, you get an instance of stdClass. You can use stdClass whenever you need a generic object instance.<br>

```
<?php
// ways of creating stdClass instances
$x = new stdClass;
$y = (object) null;        // same as above
$z = (object) &apos;a&apos;;         // creates property &apos;scalar&apos; = &apos;a&apos;
$a = (object) array(&apos;property1&apos; =&gt; 1, &apos;property2&apos; =&gt; &apos;b&apos;);
?>
```


stdClass is NOT a base class! PHP classes do not automatically inherit from any class. All classes are standalone, unless they explicitly extend another class. PHP differs from many object-oriented languages in this respect.


```
<?php
// CTest does not derive from stdClass
class CTest {
    public $property1;
}
$t = new CTest;
var_dump($t instanceof stdClass);            // false
var_dump(is_subclass_of($t, &apos;stdClass&apos;));    // false
echo get_class($t) . "\n";                   // &apos;CTest&apos;
echo get_parent_class($t) . "\n";            // false (no parent)
?>
```
<br><br>You cannot define a class named &apos;stdClass&apos; in your code. That name is already used by the system. You can define a class named &apos;Object&apos;.<br><br>You could define a class that extends stdClass, but you would get no benefit, as stdClass does nothing.<br><br>(tested on PHP 5.2.8)  

#

Some thing that may be obvious to the seasoned PHP programmer, but may surprise someone coming over from C++:<br><br>

```
<?php
class Foo
{
$bar = &apos;Hi There&apos;;

public function Print(){
    echo $bar;
}
}
?>
```


Gives an error saying Print used undefined variable. One has to explicitly use (notice the use of 

```
<?php $this-&gt;bar ?>
```
):



```
<?php
class Foo
{
$bar = &apos;Hi There&apos;;

public function Print(){
    echo this-&gt;$bar;
}
}
?>
```


 

```
<?php echo $this-&gt;bar; ?>
```
 refers to the class member, while using $bar means using an uninitialized variable in the local context of the member function.  

#

I hope that this will help to understand how to work with static variables inside a class<br><br>

```
<?php

class a {

    public static $foo = &apos;I am foo&apos;;
    public $bar = &apos;I am bar&apos;;
    
    public static function getFoo() { echo self::$foo;    }
    public static function setFoo() { self::$foo = &apos;I am a new foo&apos;; }
    public function getBar() { echo $this-&gt;bar;    }            
}

$ob = new a();
a::getFoo();     // output: I am foo    
$ob-&gt;getFoo();    // output: I am foo
//a::getBar();     // fatal error: using $this not in object context
$ob-&gt;getBar();    // output: I am bar
                // If you keep $bar non static this will work
                // but if bar was static, then var_dump($this-&gt;bar) will output null 

// unset($ob);
a::setFoo();    // The same effect as if you called $ob-&gt;setFoo(); because $foo is static
$ob = new a();     // This will have no effects on $foo
$ob-&gt;getFoo();    // output: I am a new foo 

?>
```
<br><br>Regards<br>Motaz Abuthiab  

#

A PHP Class can be used for several things, but at the most basic level, you&apos;ll use classes to "organize and deal with like-minded data". Here&apos;s what I mean by "organizing like-minded data". First, start with unorganized data.<br><br>

```
<?php
$customer_name;
$item_name;
$item_price;
$customer_address;
$item_qty;
$item_total;
?>
```


Now to organize the data into PHP classes:



```
<?php
class Customer {
  $name;          // same as $customer_name
  $address;       // same as $customer_address
}

class Item {
  $name;          // same as $item_name
  $price;         // same as $item_price
  $qty;           // same as $item_qty
  $total;         // same as $item_total
}
?>
```


Now here&apos;s what I mean by "dealing" with the data. Note: The data is already organized, so that in itself makes writing new functions extremely easy.



```
<?php
class Customer {
  public $name, $address;                   // the data for this class...

  // function to deal with user-input / validation
  // function to build string for output
  // function to write -&gt; database
  // function to  read &lt;- database
  // etc, etc
}

class Item {
  public $name, $price, $qty, $total;        // the data for this class...

  // function to calculate total
  // function to format numbers
  // function to deal with user-input / validation
  // function to build string for output
  // function to write -&gt; database
  // function to  read &lt;- database
  // etc, etc
}
?>
```
<br><br>Imagination that each function you write only calls the bits of data in that class. Some functions may access all the data, while other functions may only access one piece of data. If each function revolves around the data inside, then you have created a good class.  

#

Here&apos;s another simple example.<br><br>

```
<?php
// PHP 5

// class definition
class Bear {
    // define properties
    public $name;
    public $weight;
    public $age;
    public $sex;
    public $colour;

    // constructor
    public function __construct() {
        $this-&gt;age = 0;
        $this-&gt;weight = 100;
    }

    // define methods
    public function eat($units) {
        echo $this-&gt;name." is eating ".$units." units of food... ";
        $this-&gt;weight += $units;
    }

    public function run() {
        echo $this-&gt;name." is running... ";
    }

    public function kill() {
        echo $this-&gt;name." is killing prey... ";
    }

    public function sleep() {
        echo $this-&gt;name." is sleeping... ";
    }
}

// extended class definition
class PolarBear extends Bear {

    // constructor
    public function __construct() {
        parent::__construct();
        $this-&gt;colour = "white";
        $this-&gt;weight = 600;
    }

    // define methods
    public function swim() {
        echo $this-&gt;name." is swimming... ";
    }
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.basic.php)

**[To root](/README.md)**
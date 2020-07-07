# The Basics





I was confused at first about object assignment, because it&apos;s not quite the same as normal assignment or assignment by reference. But I think I&apos;ve figured out what&apos;s going on.

First, think of variables in PHP as data slots. Each one is a name that points to a data slot that can hold a value that is one of the basic data types: a number, a string, a boolean, etc. When you create a reference, you are making a second name that points at the same data slot. When you assign one variable to another, you are copying the contents of one data slot to another data slot.

Now, the trick is that object instances are not like the basic data types. They cannot be held in the data slots directly. Instead, an object&apos;s &quot;handle&quot; goes in the data slot. This is an identifier that points at one particular instance of an obect. So, the object handle, although not directly visible to the programmer, is one of the basic datatypes. 

What makes this tricky is that when you take a variable which holds an object handle, and you assign it to another variable, that other variable gets a copy of the same object handle. This means that both variables can change the state of the same object instance. But they are not references, so if one of the variables is assigned a new value, it does not affect the other variable.



```
<?php
// Assignment of an object
Class Object{
&#xA0;&#xA0; public $foo=&quot;bar&quot;;
};

$objectVar = new Object();
$reference =&amp; $objectVar;
$assignment = $objectVar

//
// $objectVar ---&gt;+---------+
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |(handle1)----+
// $reference ---&gt;+---------+&#xA0;&#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; +---------+&#xA0;&#xA0; |
// $assignment --&gt;|(handle1)----+
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; +---------+&#xA0;&#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; v
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; Object(1):foo=&quot;bar&quot;
//
?>
```


$assignment has a different data slot from $objectVar, but its data slot holds a handle to the same object. This makes it behave in some ways like a reference. If you use the variable $objectVar to change the state of the Object instance, those changes also show up under $assignment, because it is pointing at that same Object instance.



```
<?php
$objectVar-&gt;foo = &quot;qux&quot;;
print_r( $objectVar );
print_r( $reference );
print_r( $assignment );

//
// $objectVar ---&gt;+---------+
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |(handle1)----+
// $reference ---&gt;+---------+&#xA0;&#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; +---------+&#xA0;&#xA0; |
// $assignment --&gt;|(handle1)----+
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; +---------+&#xA0;&#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; v
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; Object(1):foo=&quot;qux&quot;
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
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |&#xA0; NULL&#xA0;&#xA0; | 
// $reference ---&gt;+---------+
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; +---------+
// $assignment --&gt;|(handle1)----+
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; +---------+&#xA0;&#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; |
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; v
//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; Object(1):foo=&quot;qux&quot;
?>
```



  

#



You start using :: in second example although the static concept has not been explained. This is not easy to discover when you are starting from the basics.

  

#



What is the difference between&#xA0; $this&#xA0; and&#xA0; self ?

Inside a class definition, $this refers to the current object, while&#xA0; self&#xA0; refers to the current class.

It is necessary to refer to a class element using&#xA0; self ,
and refer to an object element using&#xA0; $this .
Note also how an object variable must be preceded by a keyword in its definition.

The following example illustrates a few cases:



```
<?php
class Classy {

const&#xA0; &#xA0; &#xA0;&#xA0; STAT = &apos;S&apos; ; // no dollar sign for constants (they are always static)
static&#xA0; &#xA0;&#xA0; $stat = &apos;Static&apos; ;
public&#xA0; &#xA0;&#xA0; $publ = &apos;Public&apos; ;
private&#xA0; &#xA0; $priv = &apos;Private&apos; ;
protected&#xA0; $prot = &apos;Protected&apos; ;

function __construct( ){&#xA0; }

public function showMe( ){
&#xA0; &#xA0; print &apos;&lt;br&gt; self::STAT: &apos;&#xA0; .&#xA0; self::STAT ; // refer to a (static) constant like this
&#xA0; &#xA0; print &apos;&lt;br&gt; self::$stat: &apos; . self::$stat ; // static variable
&#xA0; &#xA0; print &apos;&lt;br&gt;$this-&gt;stat: &apos;&#xA0; . $this-&gt;stat ; // legal, but not what you might think: empty result
&#xA0; &#xA0; print &apos;&lt;br&gt;$this-&gt;publ: &apos;&#xA0; . $this-&gt;publ ; // refer to an object variable like this
&#xA0; &#xA0; print &apos;&lt;br&gt;&apos; ;
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



CLASSES and OBJECTS that represent the &quot;Ideal World&quot;

Wouldn&apos;t it be great to get the lawn mowed by saying $son-&gt;mowLawn()? Assuming the function mowLawn() is defined, and you have a son that doesn&apos;t throw errors, the lawn will be mowed. 

In the following example; let objects of type Line3D measure their own length in 3-dimensional space. Why should I or PHP have to provide another method from outside this class to calculate length, when the class itself holds all the neccessary data and has the education to make the calculation for itself?



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
&#xA0; &#xA0; public $x;
&#xA0; &#xA0; public $y;
&#xA0; &#xA0; public $z;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // the x coordinate of this Point.

&#xA0; &#xA0; /*
&#xA0; &#xA0;&#xA0; * use the x and y variables inherited from Point.php.
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function __construct($xCoord=0, $yCoord=0, $zCoord=0)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;x = $xCoord;
&#xA0; &#xA0; $this-&gt;y = $yCoord;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;z = $zCoord;
&#xA0; &#xA0; }

&#xA0; &#xA0; /*
&#xA0; &#xA0;&#xA0; * the (String) representation of this Point as &quot;Point3D(x, y, z)&quot;.
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function __toString()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;Point3D(x=&apos; . $this-&gt;x . &apos;, y=&apos; . $this-&gt;y . &apos;, z=&apos; . $this-&gt;z . &apos;)&apos;;
&#xA0; &#xA0; }
}

/*
 * Line3D.php
 *
 * Represents one Line in 3-dimensional space using two Point3D objects.
 */
class Line3D
{
&#xA0; &#xA0; $start;
&#xA0; &#xA0; $end;

&#xA0; &#xA0; public function __construct($xCoord1=0, $yCoord1=0, $zCoord1=0, $xCoord2=1, $yCoord2=1, $zCoord2=1)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;start = new Point3D($xCoord1, $yCoord1, $zCoord1);
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;end = new Point3D($xCoord2, $yCoord2, $zCoord2);
&#xA0; &#xA0; }

&#xA0; &#xA0; /*
&#xA0; &#xA0;&#xA0; * calculate the length of this Line in 3-dimensional space.
&#xA0; &#xA0;&#xA0; */ 
&#xA0; &#xA0; public function getLength()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return sqrt(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; pow($this-&gt;start-&gt;x - $this-&gt;end-&gt;x, 2) +
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; pow($this-&gt;start-&gt;y - $this-&gt;end-&gt;y, 2) +
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; pow($this-&gt;start-&gt;z - $this-&gt;end-&gt;z, 2)
&#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; }

&#xA0; &#xA0; /*
&#xA0; &#xA0;&#xA0; * The (String) representation of this Line as &quot;Line3D[start, end, length]&quot;.
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function __toString()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;Line3D[start=&apos; . $this-&gt;start .
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;, end=&apos; . $this-&gt;end .
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;, length=&apos; . $this-&gt;getLength() . &apos;]&apos;;
&#xA0; &#xA0; }
}

/*
 * create and display objects of type Line3D.
 */
echo &apos;&lt;p&gt;&apos; . (new Line3D()) . &quot;&lt;/p&gt;\n&quot;;
echo &apos;&lt;p&gt;&apos; . (new Line3D(0, 0, 0, 100, 100, 0)) . &quot;&lt;/p&gt;\n&quot;;
echo &apos;&lt;p&gt;&apos; . (new Line3D(0, 0, 0, 100, 100, 100)) . &quot;&lt;/p&gt;\n&quot;;

?>
```


&#xA0; &lt;--&#xA0; The results look like this&#xA0; --&gt;

Line3D[start=Point3D(x=0, y=0, z=0), end=Point3D(x=1, y=1, z=1), length=1.73205080757]

Line3D[start=Point3D(x=0, y=0, z=0), end=Point3D(x=100, y=100, z=0), length=141.421356237]

Line3D[start=Point3D(x=0, y=0, z=0), end=Point3D(x=100, y=100, z=100), length=173.205080757]

My absolute favorite thing about OOP is that &quot;good&quot; objects keep themselves in check. I mean really, it&apos;s the exact same thing in reality... like, if you hire a plumber to fix your kitchen sink, wouldn&apos;t you expect him to figure out the best plan of attack? Wouldn&apos;t he dislike the fact that you want to control the whole job? Wouldn&apos;t you expect him to not give you additional problems? And for god&apos;s sake, it is too much to ask that he cleans up before he leaves?

I say, design your classes well, so they can do their jobs uninterrupted... who like bad news? And, if your classes and objects are well defined, educated, and have all the necessary data to work on (like the examples above do), you won&apos;t have to micro-manage the whole program from outside of the class. In other words... create an object, and LET IT RIP!

  

#



stdClass is the default PHP object. stdClass has no properties, methods or parent. It does not support magic methods, and implements no interfaces.

When you cast a scalar or array as Object, you get an instance of stdClass. You can use stdClass whenever you need a generic object instance.


```
<?php
// ways of creating stdClass instances
$x = new stdClass;
$y = (object) null;&#xA0; &#xA0; &#xA0; &#xA0; // same as above
$z = (object) &apos;a&apos;;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // creates property &apos;scalar&apos; = &apos;a&apos;
$a = (object) array(&apos;property1&apos; =&gt; 1, &apos;property2&apos; =&gt; &apos;b&apos;);
?>
```


stdClass is NOT a base class! PHP classes do not automatically inherit from any class. All classes are standalone, unless they explicitly extend another class. PHP differs from many object-oriented languages in this respect.


```
<?php
// CTest does not derive from stdClass
class CTest {
&#xA0; &#xA0; public $property1;
}
$t = new CTest;
var_dump($t instanceof stdClass);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false
var_dump(is_subclass_of($t, &apos;stdClass&apos;));&#xA0; &#xA0; // false
echo get_class($t) . &quot;\n&quot;;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // &apos;CTest&apos;
echo get_parent_class($t) . &quot;\n&quot;;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false (no parent)
?>
```


You cannot define a class named &apos;stdClass&apos; in your code. That name is already used by the system. You can define a class named &apos;Object&apos;.

You could define a class that extends stdClass, but you would get no benefit, as stdClass does nothing.

(tested on PHP 5.2.8)

  

#



Some thing that may be obvious to the seasoned PHP programmer, but may surprise someone coming over from C++:



```
<?php
class Foo
{
$bar = &apos;Hi There&apos;;

public function Print(){
&#xA0; &#xA0; echo $bar;
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
&#xA0; &#xA0; echo this-&gt;$bar;
}
}
?>
```


 

```
<?php echo $this-&gt;bar; ?>
```
 refers to the class member, while using $bar means using an uninitialized variable in the local context of the member function.

  

#



I hope that this will help to understand how to work with static variables inside a class



```
<?php

class a {

&#xA0; &#xA0; public static $foo = &apos;I am foo&apos;;
&#xA0; &#xA0; public $bar = &apos;I am bar&apos;;
&#xA0; &#xA0; 
&#xA0; &#xA0; public static function getFoo() { echo self::$foo;&#xA0; &#xA0; }
&#xA0; &#xA0; public static function setFoo() { self::$foo = &apos;I am a new foo&apos;; }
&#xA0; &#xA0; public function getBar() { echo $this-&gt;bar;&#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
}

$ob = new a();
a::getFoo();&#xA0; &#xA0;&#xA0; // output: I am foo&#xA0; &#xA0; 
$ob-&gt;getFoo();&#xA0; &#xA0; // output: I am foo
//a::getBar();&#xA0; &#xA0;&#xA0; // fatal error: using $this not in object context
$ob-&gt;getBar();&#xA0; &#xA0; // output: I am bar
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // If you keep $bar non static this will work
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // but if bar was static, then var_dump($this-&gt;bar) will output null 

// unset($ob);
a::setFoo();&#xA0; &#xA0; // The same effect as if you called $ob-&gt;setFoo(); because $foo is static
$ob = new a();&#xA0; &#xA0;&#xA0; // This will have no effects on $foo
$ob-&gt;getFoo();&#xA0; &#xA0; // output: I am a new foo 

?>
```


Regards
Motaz Abuthiab

  

#



A PHP Class can be used for several things, but at the most basic level, you&apos;ll use classes to &quot;organize and deal with like-minded data&quot;. Here&apos;s what I mean by &quot;organizing like-minded data&quot;. First, start with unorganized data.



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
&#xA0; $name;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // same as $customer_name
&#xA0; $address;&#xA0; &#xA0; &#xA0;&#xA0; // same as $customer_address
}

class Item {
&#xA0; $name;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // same as $item_name
&#xA0; $price;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // same as $item_price
&#xA0; $qty;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // same as $item_qty
&#xA0; $total;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // same as $item_total
}
?>
```


Now here&apos;s what I mean by &quot;dealing&quot; with the data. Note: The data is already organized, so that in itself makes writing new functions extremely easy.



```
<?php
class Customer {
&#xA0; public $name, $address;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // the data for this class...

&#xA0; // function to deal with user-input / validation
&#xA0; // function to build string for output
&#xA0; // function to write -&gt; database
&#xA0; // function to&#xA0; read &lt;- database
&#xA0; // etc, etc
}

class Item {
&#xA0; public $name, $price, $qty, $total;&#xA0; &#xA0; &#xA0; &#xA0; // the data for this class...

&#xA0; // function to calculate total
&#xA0; // function to format numbers
&#xA0; // function to deal with user-input / validation
&#xA0; // function to build string for output
&#xA0; // function to write -&gt; database
&#xA0; // function to&#xA0; read &lt;- database
&#xA0; // etc, etc
}
?>
```


Imagination that each function you write only calls the bits of data in that class. Some functions may access all the data, while other functions may only access one piece of data. If each function revolves around the data inside, then you have created a good class.

  

#



Here&apos;s another simple example.



```
<?php
// PHP 5

// class definition
class Bear {
&#xA0; &#xA0; // define properties
&#xA0; &#xA0; public $name;
&#xA0; &#xA0; public $weight;
&#xA0; &#xA0; public $age;
&#xA0; &#xA0; public $sex;
&#xA0; &#xA0; public $colour;

&#xA0; &#xA0; // constructor
&#xA0; &#xA0; public function __construct() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;age = 0;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;weight = 100;
&#xA0; &#xA0; }

&#xA0; &#xA0; // define methods
&#xA0; &#xA0; public function eat($units) {
&#xA0; &#xA0; &#xA0; &#xA0; echo $this-&gt;name.&quot; is eating &quot;.$units.&quot; units of food... &quot;;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;weight += $units;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function run() {
&#xA0; &#xA0; &#xA0; &#xA0; echo $this-&gt;name.&quot; is running... &quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function kill() {
&#xA0; &#xA0; &#xA0; &#xA0; echo $this-&gt;name.&quot; is killing prey... &quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function sleep() {
&#xA0; &#xA0; &#xA0; &#xA0; echo $this-&gt;name.&quot; is sleeping... &quot;;
&#xA0; &#xA0; }
}

// extended class definition
class PolarBear extends Bear {

&#xA0; &#xA0; // constructor
&#xA0; &#xA0; public function __construct() {
&#xA0; &#xA0; &#xA0; &#xA0; parent::__construct();
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;colour = &quot;white&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;weight = 600;
&#xA0; &#xA0; }

&#xA0; &#xA0; // define methods
&#xA0; &#xA0; public function swim() {
&#xA0; &#xA0; &#xA0; &#xA0; echo $this-&gt;name.&quot; is swimming... &quot;;
&#xA0; &#xA0; }
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.basic.php)

**[To root](/README.md)**
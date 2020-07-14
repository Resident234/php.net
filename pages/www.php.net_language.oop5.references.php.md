# Objects and references



Notes on reference:<br>A reference is not a pointer. However, an object handle IS a pointer. Example:<br>

```
<?php
class Foo {
  private static $used;
  private $id;
  public function __construct() {
    $id = $used++;
  }
  public function __clone() {
    $id = $used++;
  }
}

$a = new Foo; // $a is a pointer pointing to Foo object 0
$b = $a; // $b is a pointer pointing to Foo object 0, however, $b is a copy of $a
$c = &amp;$a; // $c and $a are now references of a pointer pointing to Foo object 0
$a = new Foo; // $a and $c are now references of a pointer pointing to Foo object 1, $b is still a pointer pointing to Foo object 0
unset($a); // A reference with reference count 1 is automatically converted back to a value. Now $c is a pointer to Foo object 1
$a = &amp;$b; // $a and $b are now references of a pointer pointing to Foo object 0
$a = NULL; // $a and $b now become a reference to NULL. Foo object 0 can be garbage collected now
unset($b); // $b no longer exists and $a is now NULL
$a = clone $c; // $a is now a pointer to Foo object 2, $c remains a pointer to Foo object 1
unset($c); // Foo object 1 can be garbage collected now.
$c = $a; // $c and $a are pointers pointing to Foo object 2
unset($a); // Foo object 2 is still pointed by $c
$a = &amp;$c; // Foo object 2 has 1 pointers pointing to it only, that pointer has 2 references: $a and $c;
const ABC = TRUE;
if(ABC) {
  $a = NULL; // Foo object 2 can be garbage collected now because $a and $c are now a reference to the same NULL value
} else {
  unset($a); // Foo object 2 is still pointed to $c
}?>
```
  

#

There seems to be some confusion here. The distinction between pointers and references is not particularly helpful.<br>The behavior in some of the "comprehensive" examples already posted can be explained in simpler unifying terms. Hayley&apos;s code, for example, is doing EXACTLY what you should expect it should. (Using &gt;= 5.3)<br><br>First principle:<br>A pointer stores a memory address to access an object. Any time an object is assigned, a pointer is generated. (I haven&apos;t delved TOO deeply into the Zend engine yet, but as far as I can see, this applies)<br><br>2nd principle, and source of the most confusion:<br>Passing a variable to a function is done by default as a value pass, ie, you are working with a copy. "But objects are passed by reference!" A common misconception both here and in the Java world. I never said a copy OF WHAT. The default passing is done by value. Always. WHAT is being copied and passed, however, is the pointer. When using the "-&gt;", you will of course be accessing the same internals as the original variable in the caller function. Just using "=" will only play with copies.<br><br>3rd principle:<br>"&amp;" automatically and permanently sets another variable name/pointer to the same memory address as something else until you decouple them. It is correct to use the term "alias" here. Think of it as joining two pointers at the hip until forcibly separated with "unset()". This functionality exists both in the same scope and when an argument is passed to a function. Often the passed argument is called a "reference," due to certain distinctions between "passing by value" and "passing by reference" that were clearer in C and C++.<br><br>Just remember: pointers to objects, not objects themselves, are passed to functions. These pointers are COPIES of the original unless you use "&amp;" in your parameter list to actually pass the originals. Only when you dig into the internals of an object will the originals change.<br><br>Example:<br><br>

```
<?php

//The two are meant to be the same
$a = "Clark Kent"; //a==Clark Kent
$b = &amp;$a; //The two will now share the same fate.

$b="Superman"; // $a=="Superman" too.
echo $a; 
echo $a="Clark Kent"; // $b=="Clark Kent" too.
unset($b); // $b divorced from $a
$b="Bizarro";
echo $a; // $a=="Clark Kent" still, since $b is a free agent pointer now.

//The two are NOT meant to be the same.
$c="King";
$d="Pretender to the Throne";
echo $c."\n"; // $c=="King"
echo $d."\n"; // $d=="Pretender to the Throne"
swapByValue($c, $d);
echo $c."\n"; // $c=="King"
echo $d."\n"; // $d=="Pretender to the Throne"
swapByRef($c, $d);
echo $c."\n"; // $c=="Pretender to the Throne"
echo $d."\n"; // $d=="King"

function swapByValue($x, $y){
$temp=$x;
$x=$y;
$y=$temp;
//All this beautiful work will disappear
//because it was done on COPIES of pointers.
//The originals pointers still point as they did.
}

function swapByRef(&amp;$x, &amp;$y){
 $temp=$x;
 $x=$y;
 $y=$temp;
 //Note the parameter list: now we switched 'em REAL good.
}

?>
```
  

#

I&apos;ve bumped into a behavior that helped clarify the difference between objects and identifiers for me.<br><br>When we hand off an object variable, we get an identifier to that object&apos;s value.  This means that if I were to mutate the object from a passed variable, ALL variables originating from that instance of the object will change.  <br><br>HOWEVER, if I set that object variable to new instance, it replaces the identifier itself with a new identifier and leaves the old instance in tact.<br><br>Take the following example:<br><br>

```
<?php
class A {
    public $foo = 1;
}  

class B {
    public function foo(A $bar)
    {
        $bar->foo = 42;
    }
    
    public function bar(A $bar)
    {
        $bar = new A;
    }
}

$f = new A;
$g = new B;
echo $f->foo . "\n";

$g->foo($f);
echo $f->foo . "\n";

$g->bar($f);
echo $f->foo . "\n";

?>
```


If object variables were always references, we'd expect the following output:
1
42
1

However, we get:
1
42
42

The reason for this is simple.  In the bar function of the B class, we replace the identifier you passed in, which identified the same instance of the A class as your $f variable, with a brand new A class identifier.  Creating a new instance of A doesn't mutate $f because $f wasn't passed as a reference.

To get the reference behavior, one would have to enter the following for class B:



```
<?php
class B {
    public function foo(A $bar)
    {
        $bar->foo = 42;
    }
    
    public function bar(A &amp;$bar)
    {
        $bar = new A;
    }
}
?>
```
<br><br>The foo function doesn&apos;t require a reference, because it is MUTATING an object instance that $bar identifies.  But bar will be REPLACING the object instance.  If only an identifier is passed, the variable identifier will be overwritten but the object instance will be left in place.  

#

I hope this clarifies references a bit more:<br><br>

```
<?php
class A {
    public $foo = 1;
}  

$a = new A;
$b = $a;
$a->foo = 2;
$a = NULL;
echo $b->foo."\n"; // 2

$c = new A;
$d = &amp;$c;
$c->foo = 2;
$c = NULL;
echo $d->foo."\n"; // Notice:  Trying to get property of non-object...
?>
```
  

#

Ultimate explanation to object references:<br>NOTE: wording &apos;points to&apos; could be easily replaced with &apos;refers &apos; and is used loosly.<br>

```
<?php
$a1 = new A(1);  // $a1 == handle1-1 to A(1)
$a2 = $a1;     // $a2 == handle1-2 to A(1) - assigned by value (copy)
$a3 = &amp;$a1;  // $a3 points to $a1 (handle1-1)
$a3 = null;      // makes $a1==null, $a3 (still) points to $a1, $a2 == handle1-2 (same object instance A(1))
$a2 = null;      // makes $a2 == null
$a1 = new A(2); //makes $a1 == handle2-1 to new object and $a3 (still) points to $a1 => handle2-1 (new object), so value of $a1 and $a3 is the new object and $a2 == null
//By reference:
$a4 = &amp;new A(4);  //$a4 points to handle4-1 to A(4)
$a5 = $a4;   // $a5 == handle4-2 to A(4) (copy)
$a6 = &amp;$a4;  //$a6 points to (handle4-1), not to $a4 (reference to reference references the referenced object handle4-1 not the reference itself)

$a4 = &amp;new A(40); // $a4 points to handle40-1, $a5 == handle4-2 and $a6 still points to handle4-1 to A(4)
$a6 = null;  // sets handle4-1 to null; $a5 == handle4-2 = A(4); $a4 points to handle40-1; $a6 points to null
$a6 =&amp;$a4; // $a6 points to handle40-1
$a7 = &amp;$a6; //$a7 points to handle40-1
$a8 = &amp;$a7; //$a8 points to handle40-1
$a5 = $a7;  //$a5 == handle40-2 (copy)
$a6 = null; //makes handle40-1 null, all variables pointing to (hanlde40-1 ==null) are null, except ($a5 == handle40-2 = A(40))
?>
```
<br>Hope this helps.  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.references.php)

**[To root](/README.md)**
# Objects and references





Notes on reference:
A reference is not a pointer. However, an object handle IS a pointer. Example:


```
<?php
class Foo {
&#xA0; private static $used;
&#xA0; private $id;
&#xA0; public function __construct() {
&#xA0; &#xA0; $id = $used++;
&#xA0; }
&#xA0; public function __clone() {
&#xA0; &#xA0; $id = $used++;
&#xA0; }
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
&#xA0; $a = NULL; // Foo object 2 can be garbage collected now because $a and $c are now a reference to the same NULL value
} else {
&#xA0; unset($a); // Foo object 2 is still pointed to $c
}


  

#



There seems to be some confusion here. The distinction between pointers and references is not particularly helpful.
The behavior in some of the &quot;comprehensive&quot; examples already posted can be explained in simpler unifying terms. Hayley&apos;s code, for example, is doing EXACTLY what you should expect it should. (Using &gt;= 5.3)

First principle:
A pointer stores a memory address to access an object. Any time an object is assigned, a pointer is generated. (I haven&apos;t delved TOO deeply into the Zend engine yet, but as far as I can see, this applies)

2nd principle, and source of the most confusion:
Passing a variable to a function is done by default as a value pass, ie, you are working with a copy. &quot;But objects are passed by reference!&quot; A common misconception both here and in the Java world. I never said a copy OF WHAT. The default passing is done by value. Always. WHAT is being copied and passed, however, is the pointer. When using the &quot;-&gt;&quot;, you will of course be accessing the same internals as the original variable in the caller function. Just using &quot;=&quot; will only play with copies.

3rd principle:
&quot;&amp;&quot; automatically and permanently sets another variable name/pointer to the same memory address as something else until you decouple them. It is correct to use the term &quot;alias&quot; here. Think of it as joining two pointers at the hip until forcibly separated with &quot;unset()&quot;. This functionality exists both in the same scope and when an argument is passed to a function. Often the passed argument is called a &quot;reference,&quot; due to certain distinctions between &quot;passing by value&quot; and &quot;passing by reference&quot; that were clearer in C and C++.

Just remember: pointers to objects, not objects themselves, are passed to functions. These pointers are COPIES of the original unless you use &quot;&amp;&quot; in your parameter list to actually pass the originals. Only when you dig into the internals of an object will the originals change.

Example:



```
<?php

//The two are meant to be the same
$a = &quot;Clark Kent&quot;; //a==Clark Kent
$b = &amp;$a; //The two will now share the same fate.

$b=&quot;Superman&quot;; // $a==&quot;Superman&quot; too.
echo $a; 
echo $a=&quot;Clark Kent&quot;; // $b==&quot;Clark Kent&quot; too.
unset($b); // $b divorced from $a
$b=&quot;Bizarro&quot;;
echo $a; // $a==&quot;Clark Kent&quot; still, since $b is a free agent pointer now.

//The two are NOT meant to be the same.
$c=&quot;King&quot;;
$d=&quot;Pretender to the Throne&quot;;
echo $c.&quot;\n&quot;; // $c==&quot;King&quot;
echo $d.&quot;\n&quot;; // $d==&quot;Pretender to the Throne&quot;
swapByValue($c, $d);
echo $c.&quot;\n&quot;; // $c==&quot;King&quot;
echo $d.&quot;\n&quot;; // $d==&quot;Pretender to the Throne&quot;
swapByRef($c, $d);
echo $c.&quot;\n&quot;; // $c==&quot;Pretender to the Throne&quot;
echo $d.&quot;\n&quot;; // $d==&quot;King&quot;

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
 //Note the parameter list: now we switched &apos;em REAL good.
}

?>
```



  

#



I&apos;ve bumped into a behavior that helped clarify the difference between objects and identifiers for me.

When we hand off an object variable, we get an identifier to that object&apos;s value.&#xA0; This means that if I were to mutate the object from a passed variable, ALL variables originating from that instance of the object will change.&#xA0; 

HOWEVER, if I set that object variable to new instance, it replaces the identifier itself with a new identifier and leaves the old instance in tact.

Take the following example:



```
<?php
class A {
&#xA0; &#xA0; public $foo = 1;
}&#xA0; 

class B {
&#xA0; &#xA0; public function foo(A $bar)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $bar-&gt;foo = 42;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function bar(A $bar)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $bar = new A;
&#xA0; &#xA0; }
}

$f = new A;
$g = new B;
echo $f-&gt;foo . &quot;\n&quot;;

$g-&gt;foo($f);
echo $f-&gt;foo . &quot;\n&quot;;

$g-&gt;bar($f);
echo $f-&gt;foo . &quot;\n&quot;;

?>
```


If object variables were always references, we&apos;d expect the following output:
1
42
1

However, we get:
1
42
42

The reason for this is simple.&#xA0; In the bar function of the B class, we replace the identifier you passed in, which identified the same instance of the A class as your $f variable, with a brand new A class identifier.&#xA0; Creating a new instance of A doesn&apos;t mutate $f because $f wasn&apos;t passed as a reference.

To get the reference behavior, one would have to enter the following for class B:



```
<?php
class B {
&#xA0; &#xA0; public function foo(A $bar)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $bar-&gt;foo = 42;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function bar(A &amp;$bar)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $bar = new A;
&#xA0; &#xA0; }
}
?>
```


The foo function doesn&apos;t require a reference, because it is MUTATING an object instance that $bar identifies.&#xA0; But bar will be REPLACING the object instance.&#xA0; If only an identifier is passed, the variable identifier will be overwritten but the object instance will be left in place.

  

#



I hope this clarifies references a bit more:



```
<?php
class A {
&#xA0; &#xA0; public $foo = 1;
}&#xA0; 

$a = new A;
$b = $a;
$a-&gt;foo = 2;
$a = NULL;
echo $b-&gt;foo.&quot;\n&quot;; // 2

$c = new A;
$d = &amp;$c;
$c-&gt;foo = 2;
$c = NULL;
echo $d-&gt;foo.&quot;\n&quot;; // Notice:&#xA0; Trying to get property of non-object...
?>
```



  

#



Ultimate explanation to object references:
NOTE: wording &apos;points to&apos; could be easily replaced with &apos;refers &apos; and is used loosly.


```
<?php
$a1 = new A(1);&#xA0; // $a1 == handle1-1 to A(1)
$a2 = $a1;&#xA0; &#xA0;&#xA0; // $a2 == handle1-2 to A(1) - assigned by value (copy)
$a3 = &amp;$a1;&#xA0; // $a3 points to $a1 (handle1-1)
$a3 = null;&#xA0; &#xA0; &#xA0; // makes $a1==null, $a3 (still) points to $a1, $a2 == handle1-2 (same object instance A(1))
$a2 = null;&#xA0; &#xA0; &#xA0; // makes $a2 == null
$a1 = new A(2); //makes $a1 == handle2-1 to new object and $a3 (still) points to $a1 =&gt; handle2-1 (new object), so value of $a1 and $a3 is the new object and $a2 == null
//By reference:
$a4 = &amp;new A(4);&#xA0; //$a4 points to handle4-1 to A(4)
$a5 = $a4;&#xA0;&#xA0; // $a5 == handle4-2 to A(4) (copy)
$a6 = &amp;$a4;&#xA0; //$a6 points to (handle4-1), not to $a4 (reference to reference references the referenced object handle4-1 not the reference itself)

$a4 = &amp;new A(40); // $a4 points to handle40-1, $a5 == handle4-2 and $a6 still points to handle4-1 to A(4)
$a6 = null;&#xA0; // sets handle4-1 to null; $a5 == handle4-2 = A(4); $a4 points to handle40-1; $a6 points to null
$a6 =&amp;$a4; // $a6 points to handle40-1
$a7 = &amp;$a6; //$a7 points to handle40-1
$a8 = &amp;$a7; //$a8 points to handle40-1
$a5 = $a7;&#xA0; //$a5 == handle40-2 (copy)
$a6 = null; //makes handle40-1 null, all variables pointing to (hanlde40-1 ==null) are null, except ($a5 == handle40-2 = A(40))
?>
```

Hope this helps.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.references.php)

**[To root](/README.md)**
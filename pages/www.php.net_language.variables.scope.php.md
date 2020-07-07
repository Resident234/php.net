# Variable scope





Some interesting behavior (tested with PHP5), using the static-scope-keyword inside of class-methods.



```
<?php

class sample_class
{
&#xA0; public function func_having_static_var($x = NULL)
&#xA0; {
&#xA0; &#xA0; static $var = 0;
&#xA0; &#xA0; if ($x === NULL)
&#xA0; &#xA0; { return $var; }
&#xA0; &#xA0; $var = $x;
&#xA0; }
}

$a = new sample_class();
$b = new sample_class();

echo $a-&gt;func_having_static_var().&quot;\n&quot;;
echo $b-&gt;func_having_static_var().&quot;\n&quot;;
// this will output (as expected):
//&#xA0; 0
//&#xA0; 0

$a-&gt;func_having_static_var(3);

echo $a-&gt;func_having_static_var().&quot;\n&quot;;
echo $b-&gt;func_having_static_var().&quot;\n&quot;;
// this will output:
//&#xA0; 3
//&#xA0; 3
// maybe you expected:
//&#xA0; 3
//&#xA0; 0

?>
```


One could expect &quot;3 0&quot; to be outputted, as you might think that $a-&gt;func_having_static_var(3); only alters the value of the static $var of the function &quot;in&quot; $a - but as the name says, these are class-methods. Having an object is just a collection of properties, the functions remain at the class. So if you declare a variable as static inside a function, it&apos;s static for the whole class and all of its instances, not for each object.

Maybe it&apos;s senseless to post that.. cause if you want to have the behaviour that I expected, you can simply use a variable of the object itself:



```
<?php
class sample_class
{ protected $var = 0; 
&#xA0; function func($x = NULL)
&#xA0; { $this-&gt;var = $x; }
} ?>
```


I believe that all normal-thinking people would never even try to make this work with the static-keyword, for those who try (like me), this note maybe helpfull.

  

#



Note that unlike Java and C++, variables declared inside blocks such as loops or if&apos;s, will also be recognized and accessible outside of the block, so:


```
<?php
for($j=0; $j&lt;3; $j++)
{
&#xA0; &#xA0;&#xA0; if($j == 1)
&#xA0; &#xA0; &#xA0; &#xA0; $a = 4;
}
echo $a;
?>
```


Would print 4.

  

#



Please note for using global variable in child functions:



This won&apos;t work correctly...





```
<?php

function foo(){

&#xA0; &#xA0; $f_a = &apos;a&apos;;

&#xA0; &#xA0; 

&#xA0; &#xA0; function bar(){

&#xA0; &#xA0; &#xA0; &#xA0; global $f_a;

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&quot;f_a&quot; in BAR is: &apos; . $f_a . &apos;&lt;br /&gt;&apos;;&#xA0; // doesn&apos;t work, var is empty!

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; bar();

&#xA0; &#xA0; echo &apos;&quot;f_a&quot; in FOO is: &apos; . $f_a . &apos;&lt;br /&gt;&apos;;

}

?>
```




This will...





```
<?php

function foo(){

&#xA0; &#xA0; global $f_a;&#xA0;&#xA0; // &lt;- Notice to this

&#xA0; &#xA0; $f_a = &apos;a&apos;;

&#xA0; &#xA0; 

&#xA0; &#xA0; function bar(){

&#xA0; &#xA0; &#xA0; &#xA0; global $f_a;

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&quot;f_a&quot; in BAR is: &apos; . $f_a . &apos;&lt;br /&gt;&apos;;&#xA0; // work!, var is &apos;a&apos;

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; bar();

&#xA0; &#xA0; echo &apos;&quot;f_a&quot; in FOO is: &apos; . $f_a . &apos;&lt;br /&gt;&apos;;

}

?>
```



  

#



Static variables do not hold through inheritance.&#xA0; Let class A have a function Z with a static variable.&#xA0; Let class B extend class A in which function Z is not overwritten.&#xA0; Two static variables will be created, one for class A and one for class B.

Look at this example:



```
<?php
class A {
&#xA0; &#xA0; function Z() {
&#xA0; &#xA0; &#xA0; &#xA0; static $count = 0;&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; printf(&quot;%s: %d\n&quot;, get_class($this), ++$count);
&#xA0; &#xA0; }
}

class B extends A {}

$a = new A();
$b = new B();
$a-&gt;Z();
$a-&gt;Z();
$b-&gt;Z();
$a-&gt;Z();
?>
```


This code returns:

A: 1
A: 2
B: 1
A: 3

As you can see, class A and B are using different static variables even though the same function was being used.

  

#



Took me longer than I expected to figure this out, and thought others might find it useful.

I created a function (safeinclude), which I use to include files; it does processing before the file is actually included (determine full path, check it exists, etc).

Problem: Because the include was occurring inside the function, all of the variables inside the included file were inheriting the variable scope of the function; since the included files may or may not require global variables that are declared else where, it creates a problem.

Most places (including here) seem to address this issue by something such as:


```
<?php
//declare this before include
global $myVar;
//or declare this inside the include file
$nowglobal = $GLOBALS[&apos;myVar&apos;];
?>
```


But, to make this work in this situation (where a standard PHP file is included within a function, being called from another PHP script; where it is important to have access to whatever global variables there may be)... it is not practical to employ the above method for EVERY variable in every PHP file being included by &apos;safeinclude&apos;, nor is it practical to staticly name every possible variable in the &quot;global $this&quot; approach. (namely because the code is modulized, and &apos;safeinclude&apos; is meant to be generic)

My solution: Thus, to make all my global variables available to the files included with my safeinclude function, I had to add the following code to my safeinclude function (before variables are used or file is included)



```
<?php
foreach ($GLOBALS as $key =&gt; $val) { global $$key; }
?>
```


Thus, complete code looks something like the following (very basic model):



```
<?php
function safeinclude($filename)
{
&#xA0; &#xA0; //This line takes all the global variables, and sets their scope within the function:
&#xA0; &#xA0; foreach ($GLOBALS as $key =&gt; $val) { global $$key; }
&#xA0; &#xA0; /* Pre-Processing here: validate filename input, determine full path
&#xA0; &#xA0; &#xA0; &#xA0; of file, check that file exists, etc. This is obviously not
&#xA0; &#xA0; &#xA0; &#xA0; necessary, but steps I found useful. */
&#xA0; &#xA0; if ($exists==true) { include(&quot;$file&quot;); }
&#xA0; &#xA0; return $exists;
}
?>
```


In the above, &apos;exists&apos; &amp; &apos;file&apos; are determined in the pre-processing. File is the full server path to the file, and exists is set to true if the file exists. This basic model can be expanded of course.&#xA0; In my own, I added additional optional parameters so that I can call safeinclude to see if a file exists without actually including it (to take advantage of my path/etc preprocessing, verses just calling the file exists function).

Pretty simple approach that I could not find anywhere online; only other approach I could find was using PHP&apos;s eval().

  

#



Note that if you declare a variable in a function, then set it as global in that function, its value will not be retained outside of that function.&#xA0; This was tripping me up for a while so I thought it would be worth noting.

&lt;?PHP

foo();
echo $a; // echoes nothing

bar();
echo $b; //echoes &quot;b&quot;;

function foo() {
&#xA0; $a = &quot;a&quot;; 
&#xA0; global $a;
}

function bar() {
&#xA0; global $b;
&#xA0; $b = &quot;b&quot;;
}

?>
```



  

#



If you have a static variable in a method of a class, all DIRECT instances of that class share that one static variable.

However if you create a derived class, all DIRECT instances of that derived class will share one, but DISTINCT, copy of that static variable in method.

To put it the other way around, a static variable in a method is bound to a class (not to instance). Each subclass has own copy of that variable, to be shared among its instances.

To put it yet another way around, when you create a derived class, it &apos;seems&#xA0; to&apos; create a copy of methods from the base class, and thusly create copy of the static variables in those methods.

Tested with PHP 7.0.16.



```
<?php

require &apos;libs.php&apos;;
require &apos;setup.php&apos;;

class Base {
&#xA0; &#xA0; function test($delta = 0) {
&#xA0; &#xA0; &#xA0; &#xA0; static $v = 0;
&#xA0; &#xA0; &#xA0; &#xA0; $v += $delta;
&#xA0; &#xA0; &#xA0; &#xA0; return $v;
&#xA0; &#xA0; }
}

class Derived extends Base {}

$base1 = new Base();
$base2 = new Base();
$derived1 = new Derived();
$derived2 = new Derived();

$base1-&gt;test(3);
$base2-&gt;test(4);
$derived1-&gt;test(5);
$derived2-&gt;test(6);

var_dump([ $base1-&gt;test(), $base2-&gt;test(), $derived1-&gt;test(), $derived2-&gt;test() ]);

# =&gt; array(4) { [0]=&gt; int(7) [1]=&gt; int(7) [2]=&gt; int(11) [3]=&gt; int(11) }

# $base1 and $base2 share one copy of static variable $v
# derived1 and $derived2 share another copy of static variable $v


  

#



About more complex situation using global variables..

Let&apos;s say we have two files:
a.php


```
<?php 
&#xA0; &#xA0; function a() { 
&#xA0; &#xA0; &#xA0; &#xA0; include(&quot;b.php&quot;); 
&#xA0; &#xA0; }
&#xA0; &#xA0; a();
?>
```


b.php


```
<?php
&#xA0; &#xA0; $b = &quot;something&quot;;
&#xA0; &#xA0; function b() {
&#xA0; &#xA0; &#xA0; &#xA0; global $b;
&#xA0; &#xA0; &#xA0; &#xA0; $b = &quot;something new&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; b();
&#xA0; &#xA0; echo $b;
?>
```


You could expect that this script will return &quot;something new&quot; but no, it will return &quot;something&quot;. To make it working properly, you must add global keyword in $b definition, in above example it will be:

global $b;
$b = &quot;something&quot;;

  

#



It will be obvious for most of you: changing value of a static in one instance changes value in all instances.



```
<?php

&#xA0; &#xA0; class example {
&#xA0; &#xA0; &#xA0; &#xA0; public static $s = &apos;unchanged&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; public function set() {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this::$s = &apos;changed&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; $o = new example;
&#xA0; &#xA0; $p = new example;

&#xA0; &#xA0; $o-&gt;set();

&#xA0; &#xA0; print &quot;$o static: {$o::$i}\n$p static: {$p::$i}&quot;;

?>
```


Output will be:

$o static: changed
$p static: changed

  

#



Sometimes a variable available in global scope is not accessible via the &apos;global&apos; keyword or the $GLOBALS superglobal array. I have not been able to replicate it in original code, but it occurs when a script is run under PHPUnit.

PHPUnit provides a variable &quot;$filename&quot; that reflects the name of the file loaded on its command line. This is available in global scope, but not in object scope. For example, the following phpUnit script (call it GlobalScope.php):



```
<?php
print &quot;Global scope FILENAME [$filename]\n&quot;;
class MyTestClass extends PHPUnit_Framework_TestCase {
&#xA0; function testMyTest() {
&#xA0; &#xA0; global $filename;
&#xA0; &#xA0; print &quot;Method scope global FILENAME [$filename]\n&quot;;
&#xA0; &#xA0; print &quot;Method scope GLOBALS[FILENAME] [&quot;.$GLOBALS[&quot;filename&quot;].&quot;]\n&quot;;
&#xA0; }
}
?>
```


If you run this script via &quot;phpunit GlobalScope.php&quot;, you will get:

Global scope FILENAME [/home/ktyler/GlobalScope.?>
```

PHPUnit 3.4.5 by Sebastian Bergmann.

Method scope global FILENAME []
Method scope GLOBALS[FILENAME] []
.

You have to -- strange as it seems -- do the following:



```
<?php
$GLOBALS[&quot;filename&quot;]=$filename;
print &quot;Global scope FILENAME [$filename]\n&quot;;
class MyTestClass extends PHPUnit_Framework_TestCase {
&#xA0; function testMyTest() {
&#xA0; &#xA0; global $filename;
&#xA0; &#xA0; print &quot;Method scope global FILENAME [$filename]\n&quot;;
&#xA0; &#xA0; print &quot;Method scope GLOBALS[FILENAME] [&quot;.$GLOBALS[&quot;filename&quot;].&quot;]\n&quot;;
&#xA0; }
}
?>
```


By doing this, both &quot;global&quot; and $GLOBALS work!

I don&apos;t know what it is that PHPUnit does (I know it uses Reflection) that causes a globally available variable to be implicitly unavailable via &quot;global&quot; or $GLOBALS. But there it is.

  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.scope.php)

**[To root](/README.md)**
# Variable scope



Some interesting behavior (tested with PHP5), using the static-scope-keyword inside of class-methods.<br><br>

```
<?php

class sample_class
{
  public function func_having_static_var($x = NULL)
  {
    static $var = 0;
    if ($x === NULL)
    { return $var; }
    $var = $x;
  }
}

$a = new sample_class();
$b = new sample_class();

echo $a->func_having_static_var()."\n";
echo $b->func_having_static_var()."\n";
// this will output (as expected):
//  0
//  0

$a->func_having_static_var(3);

echo $a->func_having_static_var()."\n";
echo $b->func_having_static_var()."\n";
// this will output:
//  3
//  3
// maybe you expected:
//  3
//  0

?>
```


One could expect "3 0" to be outputted, as you might think that $a->func_having_static_var(3); only alters the value of the static $var of the function "in" $a - but as the name says, these are class-methods. Having an object is just a collection of properties, the functions remain at the class. So if you declare a variable as static inside a function, it's static for the whole class and all of its instances, not for each object.

Maybe it's senseless to post that.. cause if you want to have the behaviour that I expected, you can simply use a variable of the object itself:



```
<?php
class sample_class
{ protected $var = 0; 
  function func($x = NULL)
  { $this->var = $x; }
} ?>
```
<br><br>I believe that all normal-thinking people would never even try to make this work with the static-keyword, for those who try (like me), this note maybe helpfull.  

#

Note that unlike Java and C++, variables declared inside blocks such as loops or if&apos;s, will also be recognized and accessible outside of the block, so:<br>

```
<?php
for($j=0; $j<3; $j++)
{
     if($j == 1)
        $a = 4;
}
echo $a;
?>
```
<br><br>Would print 4.  

#

Please note for using global variable in child functions:<br><br>This won&apos;t work correctly...<br><br>

```
<?php
function foo(){
    $f_a = 'a';
    
    function bar(){
        global $f_a;
        echo '"f_a" in BAR is: ' . $f_a . '<br />';  // doesn't work, var is empty!
    }
    
    bar();
    echo '"f_a" in FOO is: ' . $f_a . '<br />';
}
?>
```


This will...



```
<?php
function foo(){
    global $f_a;   // <- Notice to this
    $f_a = 'a';
    
    function bar(){
        global $f_a;
        echo '"f_a" in BAR is: ' . $f_a . '<br />';  // work!, var is 'a'
    }
    
    bar();
    echo '"f_a" in FOO is: ' . $f_a . '<br />';
}
?>
```
  

#

Static variables do not hold through inheritance.  Let class A have a function Z with a static variable.  Let class B extend class A in which function Z is not overwritten.  Two static variables will be created, one for class A and one for class B.<br><br>Look at this example:<br><br>

```
<?php
class A {
    function Z() {
        static $count = 0;        
        printf("%s: %d\n", get_class($this), ++$count);
    }
}

class B extends A {}

$a = new A();
$b = new B();
$a->Z();
$a->Z();
$b->Z();
$a->Z();
?>
```
<br><br>This code returns:<br><br>A: 1<br>A: 2<br>B: 1<br>A: 3<br><br>As you can see, class A and B are using different static variables even though the same function was being used.  

#

Took me longer than I expected to figure this out, and thought others might find it useful.<br><br>I created a function (safeinclude), which I use to include files; it does processing before the file is actually included (determine full path, check it exists, etc).<br><br>Problem: Because the include was occurring inside the function, all of the variables inside the included file were inheriting the variable scope of the function; since the included files may or may not require global variables that are declared else where, it creates a problem.<br><br>Most places (including here) seem to address this issue by something such as:<br>

```
<?php
//declare this before include
global $myVar;
//or declare this inside the include file
$nowglobal = $GLOBALS['myVar'];
?>
```


But, to make this work in this situation (where a standard PHP file is included within a function, being called from another PHP script; where it is important to have access to whatever global variables there may be)... it is not practical to employ the above method for EVERY variable in every PHP file being included by 'safeinclude', nor is it practical to staticly name every possible variable in the "global $this" approach. (namely because the code is modulized, and 'safeinclude' is meant to be generic)

My solution: Thus, to make all my global variables available to the files included with my safeinclude function, I had to add the following code to my safeinclude function (before variables are used or file is included)



```
<?php
foreach ($GLOBALS as $key => $val) { global $key; }
?>
```


Thus, complete code looks something like the following (very basic model):



```
<?php
function safeinclude($filename)
{
    //This line takes all the global variables, and sets their scope within the function:
    foreach ($GLOBALS as $key => $val) { global $key; }
    /* Pre-Processing here: validate filename input, determine full path
        of file, check that file exists, etc. This is obviously not
        necessary, but steps I found useful. */
    if ($exists==true) { include("$file"); }
    return $exists;
}
?>
```
<br><br>In the above, &apos;exists&apos; &amp; &apos;file&apos; are determined in the pre-processing. File is the full server path to the file, and exists is set to true if the file exists. This basic model can be expanded of course.  In my own, I added additional optional parameters so that I can call safeinclude to see if a file exists without actually including it (to take advantage of my path/etc preprocessing, verses just calling the file exists function).<br><br>Pretty simple approach that I could not find anywhere online; only other approach I could find was using PHP&apos;s eval().  

#

If you have a static variable in a method of a class, all DIRECT instances of that class share that one static variable.<br><br>However if you create a derived class, all DIRECT instances of that derived class will share one, but DISTINCT, copy of that static variable in method.<br><br>To put it the other way around, a static variable in a method is bound to a class (not to instance). Each subclass has own copy of that variable, to be shared among its instances.<br><br>To put it yet another way around, when you create a derived class, it &apos;seems  to&apos; create a copy of methods from the base class, and thusly create copy of the static variables in those methods.<br><br>Tested with PHP 7.0.16.<br><br>

```
<?php

require 'libs.php';
require 'setup.php';

class Base {
    function test($delta = 0) {
        static $v = 0;
        $v += $delta;
        return $v;
    }
}

class Derived extends Base {}

$base1 = new Base();
$base2 = new Base();
$derived1 = new Derived();
$derived2 = new Derived();

$base1->test(3);
$base2->test(4);
$derived1->test(5);
$derived2->test(6);

var_dump([ $base1->test(), $base2->test(), $derived1->test(), $derived2->test() ]);

# => array(4) { [0]=> int(7) [1]=> int(7) [2]=> int(11) [3]=> int(11) }

# $base1 and $base2 share one copy of static variable $v
# derived1 and $derived2 share another copy of static variable $v?>
```
  

#

Note that if you declare a variable in a function, then set it as global in that function, its value will not be retained outside of that function.  This was tripping me up for a while so I thought it would be worth noting.<br><br>

```
<?php

foo();
echo $a; // echoes nothing

bar();
echo $b; //echoes "b";

function foo() {
  $a = "a"; 
  global $a;
}

function bar() {
  global $b;
  $b = "b";
}

?>
```
  

#

About more complex situation using global variables..<br><br>Let&apos;s say we have two files:<br>a.php<br>

```
<?php 
    function a() { 
        include("b.php"); 
    }
    a();
?>
```


b.php


```
<?php
    $b = "something";
    function b() {
        global $b;
        $b = "something new";
    }
    b();
    echo $b;
?>
```
<br><br>You could expect that this script will return "something new" but no, it will return "something". To make it working properly, you must add global keyword in $b definition, in above example it will be:<br><br>global $b;<br>$b = "something";  

#

It will be obvious for most of you: changing value of a static in one instance changes value in all instances.<br><br>

```
<?php

    class example {
        public static $s = 'unchanged';
        
        public function set() {
            $this::$s = 'changed';
        }
    }

    $o = new example;
    $p = new example;

    $o->set();

    print "$o static: {$o::$i}\n$p static: {$p::$i}";

?>
```
<br><br>Output will be:<br><br>$o static: changed<br>$p static: changed  

#

Sometimes a variable available in global scope is not accessible via the &apos;global&apos; keyword or the $GLOBALS superglobal array. I have not been able to replicate it in original code, but it occurs when a script is run under PHPUnit.<br><br>PHPUnit provides a variable "$filename" that reflects the name of the file loaded on its command line. This is available in global scope, but not in object scope. For example, the following phpUnit script (call it GlobalScope.php):<br><br>

```
<?php
print "Global scope FILENAME [$filename]\n";
class MyTestClass extends PHPUnit_Framework_TestCase {
  function testMyTest() {
    global $filename;
    print "Method scope global FILENAME [$filename]\n";
    print "Method scope GLOBALS[FILENAME] [".$GLOBALS["filename"]."]\n";
  }
}
?>
```


If you run this script via "phpunit GlobalScope.php", you will get:

Global scope FILENAME [/home/ktyler/GlobalScope.php]
PHPUnit 3.4.5 by Sebastian Bergmann.

Method scope global FILENAME []
Method scope GLOBALS[FILENAME] []
.

You have to -- strange as it seems -- do the following:



```
<?php
$GLOBALS["filename"]=$filename;
print "Global scope FILENAME [$filename]\n";
class MyTestClass extends PHPUnit_Framework_TestCase {
  function testMyTest() {
    global $filename;
    print "Method scope global FILENAME [$filename]\n";
    print "Method scope GLOBALS[FILENAME] [".$GLOBALS["filename"]."]\n";
  }
}
?>
```
<br><br>By doing this, both "global" and $GLOBALS work!<br><br>I don&apos;t know what it is that PHPUnit does (I know it uses Reflection) that causes a globally available variable to be implicitly unavailable via "global" or $GLOBALS. But there it is.  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.scope.php)

**[To root](/README.md)**
# Function arguments



To experiment on performance of pass-by-reference and pass-by-value, I used this  script. Conclusions are below. <br><br>#!/usr/bin/php<br>

```
<?php
function sum($array,$max){   //For Reference, use:  "&amp;$array"
    $sum=0;
    for ($i=0; $i<2; $i++){
        #$array[$i]++;        //Uncomment this line to modify the array within the function.
        $sum += $array[$i];  
    }
    return ($sum);
}

$max = 1E7                  //10 M data points.
$data = range(0,$max,1);

$start = microtime(true);
for ($x = 0 ; $x < 100; $x++){
    $sum = sum($data, $max);
}
$end =  microtime(true);
echo "Time: ".($end - $start)." s\n";

/* Run times:
#    PASS BY    MODIFIED?   Time
-    -------    ---------   ----
1    value      no          56 us
2    reference  no          58 us

3    valuue     yes         129 s
4    reference  yes         66 us

Conclusions:

1. PHP is already smart about zero-copy / copy-on-write. A function call does NOT copy the data unless it needs to; the data is
   only copied on write. That's why  #1 and #2 take similar times, whereas #3 takes 2 million times longer than #4.
   [You never need to use &amp;$array to ask the compiler to do a zero-copy optimisation; it can work that out for itself.]

2. You do use &amp;$array  to tell the compiler "it is OK for the function to over-write my argument in place, I don't need the original
   any more." This can make a huge difference to performance when we have large amounts of memory to copy.
   (This is the only way it is done in C, arrays are always passed as pointers)

3. The other use of &amp; is as a way to specify where data should be *returned*. (e.g. as used by exec() ).
   (This is a C-like way of passing pointers for outputs, whereas PHP functions normally return complex types, or multiple answers
   in an array)

4. It's  unhelpful that only the function definition has &amp;. The caller should have it, at least as syntactic sugar. Otherwise
   it leads to unreadable code: because the person reading the function call doesn't expect it to pass by reference. At the moment,
   it's necessary to write a by-reference function call with a comment, thus:
    $sum = sum($data,$max);  //warning, $data passed by reference, and may be modified.

5. Sometimes, pass by reference could be at the choice of the caller, NOT the function definitition. PHP doesn't allow it, but it
   would be meaningful for the caller to decide to pass data in as a reference. i.e. "I'm done with the variable, it's OK to stomp
   on it in memory".
*/
?>
```
  

---

A function&apos;s argument that is an object, will have its properties modified by the function although you don&apos;t need to pass it by reference.<br><br>

```
<?php
$x = new stdClass();
$x->prop = 1;

function f ( $o ) // Notice the absence of &amp;
{
  $o->prop ++;
}

f($x);

echo $x->prop; // shows: 2
?>
```


This is different for arrays:



```
<?php
$y = [ 'prop' => 1 ];

function g( $a )
{
  $a['prop'] ++;
  echo $a['prop'];  // shows: 2
}

g($y);

echo $y['prop'];  // shows: 1
?>
```
  

---

In function calls, PHP clearly distinguishes between missing arguments and present but empty arguments.  Thus:<br><br>

```
<?php
function f( $x = 4 ) { echo $x . "\\n"; }
f(); // prints 4
f( null ); // prints blank line
f( $y ); // $y undefined, prints blank line
?>
```


The utility of the optional argument feature is thus somewhat diminished.  Suppose you want to call the function f many times from function g, allowing the caller of g to specify if f should be called with a specific value or with its default value:



```
<?php
function f( $x = 4 ) {echo $x . "\\n"; }

// option 1: cut and paste the default value from f's interface into g's
function g( $x = 4 ) { f( $x ); f( $x ); }

// option 2: branch based on input to g
function g( $x = null ) { if ( !isset( $x ) ) { f(); f() } else { f( $x ); f( $x ); } }
?>
```


Both options suck.

The best approach, it seems to me, is to always use a sentinel like null as the default value of an optional argument.  This way, callers like g and g's clients have many options, and furthermore, callers always know how to omit arguments so they can omit one in the middle of the parameter list.



```
<?php
function f( $x = null ) { if ( !isset( $x ) ) $x = 4; echo $x . "\\n"; }

function g( $x = null ) { f( $x ); f( $x ); }

f(); // prints 4
f( null ); // prints 4
f( $y ); // $y undefined, prints 4
g(); // prints 4 twice
g( null ); // prints 4 twice
g( 5 ); // prints 5 twice

?>
```
  

---

PASSING A "VARIABLE-LENGTH ARGUMENT LIST OF REFERENCES" TO A FUNCTION<br>As of PHP 5, Call-time pass-by-reference has been deprecated, this represents no problem in most cases, since instead of calling a function like this:<br>   myfunction($arg1, &amp;$arg2, &amp;$arg3);<br><br>you can call it<br>   myfunction($arg1, $arg2, $arg3);<br><br>provided you have defined your function as <br>   function myfuncion($a1, &amp;$a2, &amp;$a3) { // so &amp;$a2 and &amp;$a3 are <br>                                                             // declared to be refs.<br>    ... &lt;function-code&gt;<br>   }<br><br>However, what happens if you wanted to pass an undefined number of references, i.e., something like:<br>   myfunction(&amp;$arg1, &amp;$arg2, ..., &amp;$arg-n);?<br>This doesn&apos;t work in PHP 5 anymore.<br><br>In the following code I tried to amend this by using the <br>array() language-construct as the actual argument in the <br>call to the function.<br><br>

```
<?php

  function aa ($A) {
    // This function increments each 
    // "pseudo-argument" by 2s
    foreach ($A as &amp;$x) { 
      $x += 2;
    }
  }
 
  $x = 1; $y = 2; $z = 3;
  
  aa(array(&amp;$x, &amp;$y, &amp;$z));
  echo "--$x--$y--$z--\n";
  // This will output:
  // --3--4--5--
?>
```
<br><br>I hope this is useful.<br><br>Sergio.  

---

You can use (very) limited signatures for your functions, specifing type of arguments allowed. <br><br>For example:<br><br>public function Right( My_Class $a, array $b )<br><br>tells first argument have to by object of My_Class, second an array. My_Class means that you can pass also object of class that either extends My_Class or implements (if My_Class is abstract class) My_Class. If you need exactly My_Class you need to either make it final, or add some code to check what $a really.<br><br>Also note, that (unfortunately) "array" is the only built-in type you can use in signature. Any other types i.e.:<br><br>public function Wrong( string $a, boolean $b )<br><br>will cause an error, because PHP will complain that $a is not an *object* of class string (and $b is not an object of class boolean).<br><br>So if you need to know if $a is a string or $b bool, you need to write some code in your function body and i.e. throw exception if you detect type mismatch (or you can try to cast if it&apos;s doable).  

---

[Official documentation page](https://www.php.net/manual/en/functions.arguments.php)

**[To root](/README.md)**
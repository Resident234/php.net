# Function arguments





To experiment on performance of pass-by-reference and pass-by-value, I used this&#xA0; script. Conclusions are below. 

#!/usr/bin/php


```
<?php
function sum($array,$max){&#xA0;&#xA0; //For Reference, use:&#xA0; &quot;&amp;$array&quot;
&#xA0; &#xA0; $sum=0;
&#xA0; &#xA0; for ($i=0; $i&lt;2; $i++){
&#xA0; &#xA0; &#xA0; &#xA0; #$array[$i]++;&#xA0; &#xA0; &#xA0; &#xA0; //Uncomment this line to modify the array within the function.
&#xA0; &#xA0; &#xA0; &#xA0; $sum += $array[$i];&#xA0; 
&#xA0; &#xA0; }
&#xA0; &#xA0; return ($sum);
}

$max = 1E7&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //10 M data points.
$data = range(0,$max,1);

$start = microtime(true);
for ($x = 0 ; $x &lt; 100; $x++){
&#xA0; &#xA0; $sum = sum($data, $max);
}
$end =&#xA0; microtime(true);
echo &quot;Time: &quot;.($end - $start).&quot; s\n&quot;;

/* Run times:
#&#xA0; &#xA0; PASS BY&#xA0; &#xA0; MODIFIED?&#xA0;&#xA0; Time
-&#xA0; &#xA0; -------&#xA0; &#xA0; ---------&#xA0;&#xA0; ----
1&#xA0; &#xA0; value&#xA0; &#xA0; &#xA0; no&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 56 us
2&#xA0; &#xA0; reference&#xA0; no&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 58 us

3&#xA0; &#xA0; valuue&#xA0; &#xA0;&#xA0; yes&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 129 s
4&#xA0; &#xA0; reference&#xA0; yes&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 66 us

Conclusions:

1. PHP is already smart about zero-copy / copy-on-write. A function call does NOT copy the data unless it needs to; the data is
&#xA0;&#xA0; only copied on write. That&apos;s why&#xA0; #1 and #2 take similar times, whereas #3 takes 2 million times longer than #4.
&#xA0;&#xA0; [You never need to use &amp;$array to ask the compiler to do a zero-copy optimisation; it can work that out for itself.]

2. You do use &amp;$array&#xA0; to tell the compiler &quot;it is OK for the function to over-write my argument in place, I don&apos;t need the original
&#xA0;&#xA0; any more.&quot; This can make a huge difference to performance when we have large amounts of memory to copy.
&#xA0;&#xA0; (This is the only way it is done in C, arrays are always passed as pointers)

3. The other use of &amp; is as a way to specify where data should be *returned*. (e.g. as used by exec() ).
&#xA0;&#xA0; (This is a C-like way of passing pointers for outputs, whereas PHP functions normally return complex types, or multiple answers
&#xA0;&#xA0; in an array)

4. It&apos;s&#xA0; unhelpful that only the function definition has &amp;. The caller should have it, at least as syntactic sugar. Otherwise
&#xA0;&#xA0; it leads to unreadable code: because the person reading the function call doesn&apos;t expect it to pass by reference. At the moment,
&#xA0;&#xA0; it&apos;s necessary to write a by-reference function call with a comment, thus:
&#xA0; &#xA0; $sum = sum($data,$max);&#xA0; //warning, $data passed by reference, and may be modified.

5. Sometimes, pass by reference could be at the choice of the caller, NOT the function definitition. PHP doesn&apos;t allow it, but it
&#xA0;&#xA0; would be meaningful for the caller to decide to pass data in as a reference. i.e. &quot;I&apos;m done with the variable, it&apos;s OK to stomp
&#xA0;&#xA0; on it in memory&quot;.
*/
?>
```



  

#



A function&apos;s argument that is an object, will have its properties modified by the function although you don&apos;t need to pass it by reference.



```
<?php
$x = new stdClass();
$x-&gt;prop = 1;

function f ( $o ) // Notice the absence of &amp;
{
&#xA0; $o-&gt;prop ++;
}

f($x);

echo $x-&gt;prop; // shows: 2
?>
```


This is different for arrays:



```
<?php
$y = [ &apos;prop&apos; =&gt; 1 ];

function g( $a )
{
&#xA0; $a[&apos;prop&apos;] ++;
&#xA0; echo $a[&apos;prop&apos;];&#xA0; // shows: 2
}

g($y);

echo $y[&apos;prop&apos;];&#xA0; // shows: 1
?>
```



  

#



In function calls, PHP clearly distinguishes between missing arguments and present but empty arguments.&#xA0; Thus:



```
<?php
function f( $x = 4 ) { echo $x . &quot;\\n&quot;; }
f(); // prints 4
f( null ); // prints blank line
f( $y ); // $y undefined, prints blank line
?>
```


The utility of the optional argument feature is thus somewhat diminished.&#xA0; Suppose you want to call the function f many times from function g, allowing the caller of g to specify if f should be called with a specific value or with its default value:



```
<?php
function f( $x = 4 ) {echo $x . &quot;\\n&quot;; }

// option 1: cut and paste the default value from f&apos;s interface into g&apos;s
function g( $x = 4 ) { f( $x ); f( $x ); }

// option 2: branch based on input to g
function g( $x = null ) { if ( !isset( $x ) ) { f(); f() } else { f( $x ); f( $x ); } }
?>
```


Both options suck.

The best approach, it seems to me, is to always use a sentinel like null as the default value of an optional argument.&#xA0; This way, callers like g and g&apos;s clients have many options, and furthermore, callers always know how to omit arguments so they can omit one in the middle of the parameter list.



```
<?php
function f( $x = null ) { if ( !isset( $x ) ) $x = 4; echo $x . &quot;\\n&quot;; }

function g( $x = null ) { f( $x ); f( $x ); }

f(); // prints 4
f( null ); // prints 4
f( $y ); // $y undefined, prints 4
g(); // prints 4 twice
g( null ); // prints 4 twice
g( 5 ); // prints 5 twice

?>
```



  

#



PASSING A &quot;VARIABLE-LENGTH ARGUMENT LIST OF REFERENCES&quot; TO A FUNCTION
As of PHP 5, Call-time pass-by-reference has been deprecated, this represents no problem in most cases, since instead of calling a function like this:
&#xA0;&#xA0; myfunction($arg1, &amp;$arg2, &amp;$arg3);

you can call it
&#xA0;&#xA0; myfunction($arg1, $arg2, $arg3);

provided you have defined your function as 
&#xA0;&#xA0; function myfuncion($a1, &amp;$a2, &amp;$a3) { // so &amp;$a2 and &amp;$a3 are 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // declared to be refs.
&#xA0; &#xA0; ... &lt;function-code&gt;
&#xA0;&#xA0; }

However, what happens if you wanted to pass an undefined number of references, i.e., something like:
&#xA0;&#xA0; myfunction(&amp;$arg1, &amp;$arg2, ..., &amp;$arg-n);?
This doesn&apos;t work in PHP 5 anymore.

In the following code I tried to amend this by using the 
array() language-construct as the actual argument in the 
call to the function.



```
<?php

&#xA0; function aa ($A) {
&#xA0; &#xA0; // This function increments each 
&#xA0; &#xA0; // &quot;pseudo-argument&quot; by 2s
&#xA0; &#xA0; foreach ($A as &amp;$x) { 
&#xA0; &#xA0; &#xA0; $x += 2;
&#xA0; &#xA0; }
&#xA0; }
 
&#xA0; $x = 1; $y = 2; $z = 3;
&#xA0; 
&#xA0; aa(array(&amp;$x, &amp;$y, &amp;$z));
&#xA0; echo &quot;--$x--$y--$z--\n&quot;;
&#xA0; // This will output:
&#xA0; // --3--4--5--
?>
```


I hope this is useful.

Sergio.

  

#



You can use (very) limited signatures for your functions, specifing type of arguments allowed. 

For example:

public function Right( My_Class $a, array $b )

tells first argument have to by object of My_Class, second an array. My_Class means that you can pass also object of class that either extends My_Class or implements (if My_Class is abstract class) My_Class. If you need exactly My_Class you need to either make it final, or add some code to check what $a really.

Also note, that (unfortunately) &quot;array&quot; is the only built-in type you can use in signature. Any other types i.e.:

public function Wrong( string $a, boolean $b )

will cause an error, because PHP will complain that $a is not an *object* of class string (and $b is not an object of class boolean).

So if you need to know if $a is a string or $b bool, you need to write some code in your function body and i.e. throw exception if you detect type mismatch (or you can try to cast if it&apos;s doable).

  

#



There is a nice trick to emulate variables/function calls/etc as default values:



```
<?php
$myVar = &quot;Using a variable as a default value!&quot;;

function myFunction($myArgument=null) {
&#xA0; &#xA0; if($myArgument===null)
&#xA0; &#xA0; &#xA0; &#xA0; $myArgument = $GLOBALS[&quot;myVar&quot;];
&#xA0; &#xA0; echo $myArgument;
}

// Outputs &quot;Hello World!&quot;:
myFunction(&quot;Hello World!&quot;);
// Outputs &quot;Using a variable as a default value!&quot;:
myFunction();
// Outputs the same again:
myFunction(null);
// Outputs &quot;Changing the variable affects the function!&quot;:
$myVar = &quot;Changing the variable affects the function!&quot;;
myFunction();
?>
```

In general, you define the default value as null (or whatever constant you like), and then check for that value at the start of the function, computing the actual default if needed, before using the argument for actual work.
Building upon this, it&apos;s also easy to provide fallback behaviors when the argument given is not valid: simply put a default that is known to be invalid in the prototype, and then check for general validity instead of a specific value: if the argument is not valid (either not given, so the default is used, or an invalid value was given), the function computes a (valid) default to use.

  

#

[Official documentation page](https://www.php.net/manual/en/functions.arguments.php)

**[To root](/README.md)**
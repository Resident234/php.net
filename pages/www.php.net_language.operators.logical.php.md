# Logical Operators





Note that PHP&apos;s boolean operators *always* return a boolean value... as opposed to other languages that return the value of the last evaluated expression.

For example:

$a = 0 || &apos;avacado&apos;;
print &quot;A: $a\n&quot;;

will print:

A: 1

in PHP -- as opposed to printing &quot;A: avacado&quot; as it would in a language like Perl or JavaScript.

This means you can&apos;t use the &apos;||&apos; operator to set a default value:

$a = $fruit || &apos;apple&apos;;

instead, you have to use the &apos;?:&apos; operator:

$a = ($fruit ? $fruit : &apos;apple&apos;);

  

#



In addition to what Lawrence said about assigning a default value, one can now use the Null Coalescing Operator (PHP 7). Hence when we want to assign a default value we can write:

$a = ($fruit ?? &apos;apple&apos;); 
//assigns the $fruit variable content to $a if the $fruit variable exists or has a value that is not NULL, or assigns the value &apos;apple&apos; to $a if the $fruit variable doesn&apos;t exists or it contains the NULL value

  

#



This works similar to javascripts short-curcuit assignments and setting defaults. (e.g.&#xA0; var a = getParm() || &apos;a default&apos;;)



```
<?php

($a = $_GET[&apos;var&apos;]) || ($a = &apos;a default&apos;);

?>
```


$a gets assigned $_GET[&apos;var&apos;] if there&apos;s anything in it or it will fallback to &apos;a default&apos;
Parentheses are required, otherwise you&apos;ll end up with $a being a boolean.

  

#



worth reading for people learning about php and programming: (adding extras 

```
<?php ?>
```
 to get highlighted code)

about the following example in this page manual: 
Example#1 Logical operators illustrated

...


```
<?php
// &quot;||&quot; has a greater precedence than &quot;or&quot;
$e = false || true; // $e will be assigned to (false || true) which is true
$f = false or true; // $f will be assigned to false
var_dump($e, $f);

// &quot;&amp;&amp;&quot; has a greater precedence than &quot;and&quot;
$g = true &amp;&amp; false; // $g will be assigned to (true &amp;&amp; false) which is false
$h = true and false; // $h will be assigned to true
var_dump($g, $h); 
?>
```

_______________________________________________end of my quote...

If necessary, I wanted to give further explanation on this and say that when we write:
$f = false or true; // $f will be assigned to false
the explanation: 

&quot;||&quot; has a greater precedence than &quot;or&quot; 

its true. But a more acurate one would be

&quot;||&quot; has greater precedence than &quot;or&quot; and than &quot;=&quot;, whereas &quot;or&quot; doesnt have greater precedence than &quot;=&quot;, so



```
<?php
$f = false or true;

//is like writting

($f = false ) or true;

//and

$e = false || true;

is the same as

$e = (false || true);

?>
```
 

same goes for &quot;&amp;&amp;&quot; and &quot;AND&quot;. 

If you find it hard to remember operators precedence you can always use parenthesys - &quot;(&quot; and &quot;)&quot;. And even if you get to learn it remember that being a good programmer is not showing you can do code with fewer words. The point of being a good programmer is writting code that is easy to understand (comment your code when necessary!), easy to maintain and with high efficiency, among other things.

  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.logical.php)

**[To root](/README.md)**
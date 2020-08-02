# Logical Operators



Note that PHP&apos;s boolean operators *always* return a boolean value... as opposed to other languages that return the value of the last evaluated expression.<br><br>For example:<br><br>$a = 0 || &apos;avacado&apos;;<br>print "A: $a\n";<br><br>will print:<br><br>A: 1<br><br>in PHP -- as opposed to printing "A: avacado" as it would in a language like Perl or JavaScript.<br><br>This means you can&apos;t use the &apos;||&apos; operator to set a default value:<br><br>$a = $fruit || &apos;apple&apos;;<br><br>instead, you have to use the &apos;?:&apos; operator:<br><br>$a = ($fruit ? $fruit : &apos;apple&apos;);  

---

In addition to what Lawrence said about assigning a default value, one can now use the Null Coalescing Operator (PHP 7). Hence when we want to assign a default value we can write:<br><br>$a = ($fruit ?? &apos;apple&apos;); <br>//assigns the $fruit variable content to $a if the $fruit variable exists or has a value that is not NULL, or assigns the value &apos;apple&apos; to $a if the $fruit variable doesn&apos;t exists or it contains the NULL value  

---

This works similar to javascripts short-curcuit assignments and setting defaults. (e.g.  var a = getParm() || &apos;a default&apos;;)<br><br>

```
<?php

($a = $_GET['var']) || ($a = 'a default');

?>
```
<br><br>$a gets assigned $_GET[&apos;var&apos;] if there&apos;s anything in it or it will fallback to &apos;a default&apos;<br>Parentheses are required, otherwise you&apos;ll end up with $a being a boolean.  

---

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
// "||" has a greater precedence than "or"
$e = false || true; // $e will be assigned to (false || true) which is true
$f = false or true; // $f will be assigned to false
var_dump($e, $f);

// "&amp;&amp;" has a greater precedence than "and"
$g = true &amp;&amp; false; // $g will be assigned to (true &amp;&amp; false) which is false
$h = true and false; // $h will be assigned to true
var_dump($g, $h); 
?>
```

_______________________________________________end of my quote...

If necessary, I wanted to give further explanation on this and say that when we write:
$f = false or true; // $f will be assigned to false
the explanation: 

"||" has a greater precedence than "or" 

its true. But a more acurate one would be

"||" has greater precedence than "or" and than "=", whereas "or" doesnt have greater precedence than "=", so



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
 <br><br>same goes for "&amp;&amp;" and "AND". <br><br>If you find it hard to remember operators precedence you can always use parenthesys - "(" and ")". And even if you get to learn it remember that being a good programmer is not showing you can do code with fewer words. The point of being a good programmer is writting code that is easy to understand (comment your code when necessary!), easy to maintain and with high efficiency, among other things.  

---

[Official documentation page](https://www.php.net/manual/en/language.operators.logical.php)

**[To root](/README.md)**
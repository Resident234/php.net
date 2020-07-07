# Assignment Operators





This page really ought to have table of assignment operators,
namely,

See the Arithmetic Operators page (http://www.php.net/manual/en/language.operators.arithmetic.php)
Assignment&#xA0; &#xA0; Same as:
$a += $b&#xA0; &#xA0;&#xA0; $a = $a + $b&#xA0; &#xA0; Addition
$a -= $b&#xA0; &#xA0;&#xA0; $a = $a - $b&#xA0; &#xA0;&#xA0; Subtraction
$a *= $b&#xA0; &#xA0;&#xA0; $a = $a * $b&#xA0; &#xA0;&#xA0; Multiplication
$a /= $b&#xA0; &#xA0;&#xA0; $a = $a / $b&#xA0; &#xA0; Division
$a %= $b&#xA0; &#xA0;&#xA0; $a = $a % $b&#xA0; &#xA0; Modulus

See the String Operators page(http://www.php.net/manual/en/language.operators.string.php)
$a .= $b&#xA0; &#xA0;&#xA0; $a = $a . $b&#xA0; &#xA0; &#xA0;&#xA0; Concatenate

See the Bitwise Operators page (http://www.php.net/manual/en/language.operators.bitwise.php)
$a &amp;= $b&#xA0; &#xA0;&#xA0; $a = $a &amp; $b&#xA0; &#xA0;&#xA0; Bitwise And
$a |= $b&#xA0; &#xA0;&#xA0; $a = $a | $b&#xA0; &#xA0; &#xA0; Bitwise Or
$a ^= $b&#xA0; &#xA0;&#xA0; $a = $a ^ $b&#xA0; &#xA0; &#xA0;&#xA0; Bitwise Xor
$a &lt;&lt;= $b&#xA0; &#xA0;&#xA0; $a = $a &lt;&lt; $b&#xA0; &#xA0;&#xA0; Left shift
$a &gt;&gt;= $b&#xA0; &#xA0;&#xA0; $a = $a &gt;&gt; $b&#xA0; &#xA0; &#xA0; Right shift

  

#



Using $text .= &quot;additional text&quot;; instead of $text =&#xA0; $text .&quot;additional text&quot;; can seriously enhance performance due to memory allocation efficiency. 

I reduced execution time from 5 sec to .5 sec (10 times) by simply switching to the first pattern for a loop with 900 iterations over a string $text that reaches 800K by the end.

  

#



Be aware of assignments with conditionals. The assignment operator is stronger as &apos;and&apos;, &apos;or&apos; and &apos;xor&apos;.



```
<?php 
$x = true and false;&#xA0;&#xA0; //$x will be true
$y = (true and false); //$y will be false
?>
```



  

#



bradlis7 at bradlis7 dot com&apos;s description is a bit confusing. Here it is rephrased.



```
<?php
$a = &apos;a&apos;;
$b = &apos;b&apos;;

$a .= $b .= &quot;foo&quot;;

echo $a,&quot;\n&quot;,$b;?>
```

outputs

abfoo
bfoo

Because the assignment operators are right-associative and evaluate to the result of the assignment


```
<?php
$a .= $b .= &quot;foo&quot;;
?>
```

is equivalent to


```
<?php
$a .= ($b .= &quot;foo&quot;);
?>
```

and therefore


```
<?php
$b .= &quot;foo&quot;;
$a .= $b;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.assignment.php)

**[To root](/README.md)**
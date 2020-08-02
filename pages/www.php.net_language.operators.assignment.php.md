# Assignment Operators



This page really ought to have table of assignment operators,<br>namely,<br><br>See the Arithmetic Operators page (http://www.php.net/manual/en/language.operators.arithmetic.php)<br>Assignment    Same as:<br>$a += $b     $a = $a + $b    Addition<br>$a -= $b     $a = $a - $b     Subtraction<br>$a *= $b     $a = $a * $b     Multiplication<br>$a /= $b     $a = $a / $b    Division<br>$a %= $b     $a = $a % $b    Modulus<br><br>See the String Operators page(http://www.php.net/manual/en/language.operators.string.php)<br>$a .= $b     $a = $a . $b       Concatenate<br><br>See the Bitwise Operators page (http://www.php.net/manual/en/language.operators.bitwise.php)<br>$a &amp;= $b     $a = $a &amp; $b     Bitwise And<br>$a |= $b     $a = $a | $b      Bitwise Or<br>$a ^= $b     $a = $a ^ $b       Bitwise Xor<br>$a &lt;&lt;= $b     $a = $a &lt;&lt; $b     Left shift<br>$a &gt;&gt;= $b     $a = $a &gt;&gt; $b      Right shift  

---

Using $text .= "additional text"; instead of $text =  $text ."additional text"; can seriously enhance performance due to memory allocation efficiency. <br><br>I reduced execution time from 5 sec to .5 sec (10 times) by simply switching to the first pattern for a loop with 900 iterations over a string $text that reaches 800K by the end.  

---

Be aware of assignments with conditionals. The assignment operator is stronger as &apos;and&apos;, &apos;or&apos; and &apos;xor&apos;.<br><br>

```
<?php 
$x = true and false;   //$x will be true
$y = (true and false); //$y will be false
?>
```
  

---

bradlis7 at bradlis7 dot com&apos;s description is a bit confusing. Here it is rephrased.<br><br>

```
<?php
$a = 'a';
$b = 'b';

$a .= $b .= "foo";

echo $a,"\n",$b;?>
```

outputs

abfoo
bfoo

Because the assignment operators are right-associative and evaluate to the result of the assignment


```
<?php
$a .= $b .= "foo";
?>
```

is equivalent to


```
<?php
$a .= ($b .= "foo");
?>
```

and therefore


```
<?php
$b .= "foo";
$a .= $b;
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/language.operators.assignment.php)

**[To root](/README.md)**
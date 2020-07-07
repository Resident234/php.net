# Arithmetic Operators





The modulus operator is very poorly suited for such a simple operation as determining if an int is even or odd. On most common systems, modulus performs a division, which is a very slow operation.
A much better way to find if a number is even or odd is to use the bitwise &amp; operator.

e.g.

$is_odd = $x &amp; 1; //using and
$is_odd = $x % 2; //using modulus

  

#



A very simple yet maybe not obvious use of the modulus (%) operator is to check if an integer is odd or even.


```
<?php
&#xA0; if (($a % 2) == 1)
&#xA0; { echo &quot;$a is odd.&quot; ;}
&#xA0; if (($a % 2) == 0)
&#xA0; { echo &quot;$a is even.&quot; ;}
?>
```


This is nice when you want to make alternating-color rows on a table, or divs.



```
<?php
&#xA0; for ($i = 1; $i &lt;= 10; $i++) {
&#xA0; &#xA0; if(($i % 2) == 1)&#xA0; //odd
&#xA0; &#xA0; &#xA0; {echo &quot;&lt;div class=\&quot;dark\&quot;&gt;$i&lt;/div&gt;&quot;;}
&#xA0; &#xA0; else&#xA0;&#xA0; //even
&#xA0; &#xA0; &#xA0; {echo &quot;&lt;div class=\&quot;light\&quot;&gt;$i&lt;/div&gt;&quot;;}
&#xA0;&#xA0; }
?>
```



  

#



Note that operator % (modulus) works just with integers (between -214748348 and 2147483647) while fmod() works with short and large numbers.

Modulus with non integer numbers will give unpredictable results.

  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.arithmetic.php)

**[To root](/README.md)**
# Arithmetic Operators



The modulus operator is very poorly suited for such a simple operation as determining if an int is even or odd. On most common systems, modulus performs a division, which is a very slow operation.<br>A much better way to find if a number is even or odd is to use the bitwise &amp; operator.<br><br>e.g.<br><br>$is_odd = $x &amp; 1; //using and<br>$is_odd = $x % 2; //using modulus  

#

A very simple yet maybe not obvious use of the modulus (%) operator is to check if an integer is odd or even.<br>

```
<?php
  if (($a % 2) == 1)
  { echo "$a is odd." ;}
  if (($a % 2) == 0)
  { echo "$a is even." ;}
?>
```


This is nice when you want to make alternating-color rows on a table, or divs.



```
<?php
  for ($i = 1; $i &lt;= 10; $i++) {
    if(($i % 2) == 1)  //odd
      {echo "&lt;div class=\"dark\"&gt;$i&lt;/div&gt;";}
    else   //even
      {echo "&lt;div class=\"light\"&gt;$i&lt;/div&gt;";}
   }
?>
```
  

#

Note that operator % (modulus) works just with integers (between -214748348 and 2147483647) while fmod() works with short and large numbers.<br><br>Modulus with non integer numbers will give unpredictable results.  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.arithmetic.php)

**[To root](/README.md)**
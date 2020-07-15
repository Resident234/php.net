# intval



Not mentioned elsewhere: intval(NULL) also returns 0.  

#

It seems intval is interpreting valid numeric strings differently between PHP 5.6 and 7.0 on one hand, and PHP 7.1 on the other hand.<br><br>

```
<?php
echo intval('1e5');
?>
```
<br><br>will return 1 on PHP 5.6 and PHP 7.0,<br>but it will return 100000 on PHP 7.1.  

#

Be careful :<br><br>

```
<?php
$n="19.99";
print intval($n*100); // prints 1998
print intval(strval($n*100)); // prints 1999
?>
```
  

#

intval converts doubles to integers by truncating the fractional component of the number.<br><br>When dealing with some values, this can give odd results.  Consider the following:<br><br>print intval ((0.1 + 0.7) * 10);<br><br>This will most likely print out 7, instead of the expected value of 8.<br><br>For more information, see the section on floating point numbers in the PHP manual (http://www.php.net/manual/language.types.double.php)<br><br>Also note that if you try to convert a string to an integer, the result is often 0.<br><br>However, if the leftmost character of a string looks like a valid numeric value, then PHP will keep reading the string until a character that is not valid in a number is encountered.<br><br>For example:<br><br>"101 Dalmations" will convert to 101<br><br>"$1,000,000" will convert to 0 (the 1st character is not a valid start for a number<br><br>"80,000 leagues ..." will convert to 80<br><br>"1.4e98 microLenats were generated when..." will convert to 1.4e98<br><br>Also note that only decimal base numbers are recognized in strings.<br><br>"099" will convert to 99, while "0x99" will convert to 0.<br><br>One additional note on the behavior of intval.  If you specify the base argument, the var argument should be a string - otherwise the base will not be applied.<br><br>For Example:<br><br>print intval (77, 8);   // Prints 77<br>print intval (&apos;77&apos;, 8); // Prints 63  

#

You guys are going to love this.  I found something that I found quite disturbing.<br><br>$test1 = intVal(1999);<br><br>$amount = 19.99 * 100;<br>$test2 = intVal($amount);<br>$test3 = intVal("$amount");<br><br>echo $test1 . "&lt;br /&gt;\n";<br>echo $test2 . "&lt;br /&gt;\n";<br>echo $test3 . "&lt;br /&gt;\n";<br><br>expected output:<br>1999<br>1999<br>1999<br><br>actual output<br>1999<br>1998<br>1999<br><br>Appears to be a floating point issue, but the number 1999 is the only number that I was able to get to do this.  19.99 is the price of many things, and for our purpose we must pass it as 1999 instead of 19.99.  

#

Here is a really useful undocumented feature:<br><br>You can have it automatically deduce the base of the number from the prefix of the string using the same syntax as integer literals in PHP ("0x" for hexadecimal, "0" for octal, non-"0" for decimal) by passing a base of 0 to intval():<br><br>

```
<?php
echo intval("0x1a", 0), "\n"; // hex; prints "26"
echo intval("057", 0), "\n"; // octal; prints "47"
echo intval("42", 0), "\n"; // decimal; prints "42"
?>
```
  

#

if you want to take a number from a string, no matter what it may contain, here is a good solution:<br><br>

```
<?php
function int($s){return(int)preg_replace('/[^\-\d]*(\-?\d*).*/','$1',$s);}

echo int('j18ugj9hu0gj5hg');
//output: 18
?>
```

this example returns an int, so it will follow the int rules, and has support for negative values.



```
<?php
function int($s){return($a=preg_replace('/[^\-\d]*(\-?\d*).*/','$1',$s))?$a:'0';}

echo int('j-1809809808908099878758765ugj9hu0gj5hg');
//output: -1809809808908099878758765
?>
```
<br><br>this one returns a string with just the numeric value.<br>it also supports negative values.<br><br>the latter is better when you have a 32 bit system and you want a huge int that is higher than PHP_MAX_INT.  

#

[Official documentation page](https://www.php.net/manual/en/function.intval.php)

**[To root](/README.md)**
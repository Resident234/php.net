# sprintf



1.  A plus sign (&apos;+&apos;) means put a &apos;+&apos; before positive numbers while a minus sign (&apos;-&apos;) means left justify.  The documentation incorrectly states that they are interchangeable.  They produce unique results that can be combined:<br><br>

```
<?php
echo sprintf ("|%+4d|%+4d|\n",   1, -1);
echo sprintf ("|%-4d|%-4d|\n",   1, -1);
echo sprintf ("|%+-4d|%+-4d|\n", 1, -1);
?>
```


outputs:

|  +1|  -1|
|1   |-1  |
|+1  |-1  |

2.  Padding with a '0' is different than padding with other characters.  Zeros will only be added at the front of a number, after any sign.  Other characters will be added before the sign, or after the number:



```
<?php
echo sprintf ("|%04d|\n",   -2);
echo sprintf ("|%':4d|\n",  -2);
echo sprintf ("|%-':4d|\n", -2);

// Specifying both "-" and "0" creates a conflict with unexpected results:
echo sprintf ("|%-04d|\n",  -2);

// Padding with other digits behaves like other non-zero characters:
echo sprintf ("|%-'14d|\n", -2);
echo sprintf ("|%-'04d|\n", -2);
?>
```
<br><br>outputs:<br><br>|-002|<br>|::-2|<br>|-2::|<br>|-2  |<br>|-211|<br>|-2  |  

---

With printf() and sprintf() functions, escape character is not backslash &apos;\&apos; but rather &apos;%&apos;.<br><br>Ie. to print &apos;%&apos; character you need to escape it with itself:<br>

```
<?php
printf('%%%s%%', 'koko'); #output: '%koko%'
?>
```
  

---

There are already some comments on using sprintf to force leading leading zeros but the examples only include integers. I needed leading zeros on floating point numbers and was surprised that it didn&apos;t work as expected.<br><br>Example:<br>

```
<?php
sprintf('%02d', 1);
?>
```


This will result in 01. However, trying the same for a float with precision doesn't work:



```
<?php
sprintf('%02.2f', 1);
?>
```


Yields 1.00. 

This threw me a little off. To get the desired result, one needs to add the precision (2) and the length of the decimal seperator "." (1). So the correct pattern would be



```
<?php
sprintf('%05.2f', 1);
?>
```
<br><br>Output: 01.00<br><br>Please see http://stackoverflow.com/a/28739819/413531 for a more detailed explanation.  

---

Here is how to print a floating point number with 16 significant digits regardless of magnitude:<br><br>

```
<?php
    $result = sprintf(sprintf('%%.%dF', max(15 - floor(log10($value)), 0)), $value);
?>
```
<br><br>This works more reliably than doing something like sprintf(&apos;%.15F&apos;, $value) as the latter may cut off significant digits for very small numbers, or prints bogus digits (meaning extra digits beyond what can reliably be represented in a floating point number) for very large numbers.  

---

I created this function a while back to save on having to combine mysql_real_escape_string onto all the params passed into a sprintf. it works literally the same as the sprintf other than that it doesn&apos;t require you to escape your inputs. Hope its of some use to people<br><br>

```
<?php
function mressf()
{
    $args = func_get_args();
    if (count($args) < 2)
        return false;
    $query = array_shift($args);
    $args = array_map('mysql_real_escape_string', $args);
    array_unshift($args, $query);
    $query = call_user_func_array('sprintf', $args);
    return $query;
}
?>
```
<br><br>Regards<br>Jay<br>Jaygilford.com  

---

[Official documentation page](https://www.php.net/manual/en/function.sprintf.php)

**[To root](/README.md)**
# Booleans



Ah, yes, booleans - bit values that are either set (TRUE) or not set (FALSE).  Now that we have 64 bit compilers using an int variable for booleans, there is *one* value which is FALSE (zero) and 2**64-1 values that are TRUE (everything else).  It appears there&apos;s a lot more truth in this universe, but false can trump anything that&apos;s true...<br><br>PHP&apos;s handling of strings as booleans is *almost* correct - an empty string is FALSE, and a non-empty string is TRUE - with one exception:  A string containing a single zero is considered FALSE.  Why?  If *any* non-empty strings are going to be considered FALSE, why *only* a single zero?  Why not "FALSE" (preferably case insensitive), or "0.0" (with how many decimal places), or "NO" (again, case insensitive), or ... ?<br><br>The *correct* design would have been that *any* non-empty string is TRUE - period, end of story.  Instead, there&apos;s another GOTCHA for the less-than-completely-experienced programmer to watch out for, and fixing the language&apos;s design error at this late date would undoubtedly break so many things that the correction is completely out of the question.<br><br>Speaking of GOTCHAs, consider this code sequence:<br>

```
<?php
$x=TRUE;
$y=FALSE;
$z=$y OR $x;
?>
```


Is $z TRUE or FALSE?

In this case, $z will be FALSE because the above code is equivalent to 

```
<?php ($z=$y) OR $x ?>
```
 rather than 

```
<?php $z=($y OR $x) ?>
```
 as might be expected - because the OR operator has lower precedence than assignment operators.

On the other hand, after this code sequence:


```
<?php
$x=TRUE;
$y=FALSE;
$z=$y || $x;
?>
```
<br><br>$z will be TRUE, as expected, because the || operator has higher precedence than assignment:  The code is equivalent to $z=($y OR $x).<br><br>This is why you should NEVER use the OR operator without explicit parentheses around the expression where it is being used.  

---

Note for JavaScript developers:<br><br>In PHP, an empty array evaluates to false, while in JavaScript an empty array evaluates to true.<br><br>In PHP, you can test an empty array as 

```
<?php if(!$stuff) &#x2026;; ?>
```
 which won&#x2019;t work in JavaScript where you need to test the array length.<br><br>This is because in JavaScript, an array is an object, and, while it may not have any elements, it is still regarded as something.<br><br>Just a trap for young players who routinely work in both langauges.  

---

Beware of certain control behavior with boolean and non boolean values :<br><br>

```
<?php
// Consider that the 0 could by any parameters including itself
var_dump(0 == 1); // false
var_dump(0 == (bool)'all'); // false
var_dump(0 == 'all'); // TRUE, take care
var_dump(0 === 'all'); // false

// To avoid this behavior, you need to cast your parameter as string like that :
var_dump((string)0 == 'all'); // false
?>
```
  

---

Just something that will probably save time for many new developers: beware of interpreting FALSE and TRUE as integers. <br>For example, a small function for deleting elements of an array may give unexpected results if you are not fully aware of what happens: <br><br>

```
<?php

function remove_element($element, $array)
{
   //array_search returns index of element, and FALSE if nothing is found
   $index = array_search($element, $array);
   unset ($array[$index]);
   return $array; 
}

// this will remove element 'A'
$array = ['A', 'B', 'C'];
$array = remove_element('A', $array);

//but any non-existent element will also remove 'A'!
$array = ['A', 'B', 'C'];
$array = remove_element('X', $array);
?>
```


The problem here is, although array_search returns boolean false when it doesn't find specific element, it is interpreted as zero when used as array index.

So you have to explicitly check for FALSE, otherwise you'll probably loose some elements:



```
<?php
//correct
function remove_element($element, $array)
{
   $index = array_search($element, $array);
   if ($index !== FALSE) 
   {
       unset ($array[$index]);
   }
   return $array; 
}?>
```
  

---

Beware that "0.00" converts to boolean TRUE !<br><br>You may get such a string from your database, if you have columns of type DECIMAL or CURRENCY. In such cases you have to explicitly check if the value is != 0 or to explicitly convert the value to int also, not only to boolean.  

---

PHP does not break any rules with the values of true and false.  The value false is not a constant for the number 0, it is a boolean value that indicates false.  The value true is also not a constant for 1, it is a special boolean value that indicates true.  It just happens to cast to integer 1 when you print it or use it in an expression, but it&apos;s not the same as a constant for the integer value 1 and you shouldn&apos;t use it as one.  Notice what it says at the top of the page:<br><br>A boolean expresses a truth value.<br><br>It does not say "a boolean expresses a 0 or 1".<br><br>It&apos;s true that symbolic constants are specifically designed to always and only reference their constant value.  But booleans are not symbolic constants, they are values.  If you&apos;re trying to add 2 boolean values you might have other problems in your application.  

---

It is correct that TRUE or FALSE should not be used as constants for the numbers 0 and 1. But there may be times when it might be helpful to see the value of the Boolean as a 1 or 0. Here&apos;s how to do it.<br><br>

```
<?php
$var1 = TRUE;
$var2 = FALSE;

echo $var1; // Will display the number 1

echo $var2; //Will display nothing

/* To get it to display the number 0 for
a false value you have to typecast it: */

echo (int)$var2; //This will display the number 0 for false.
?>
```
  

---

Note you can also use the &apos;!&apos; to convert a number to a boolean, as if it was an explicit (bool) cast then NOT.<br><br>So you can do something like:<br><br>

```
<?php
$t = !0; // This will === true;
$f = !1; // This will === false;
?>
```


And non-integers are casted as if to bool, then NOT.

Example:



```
<?php
$a = !array();      // This will === true;
$a = !array('a');   // This will === false;
$s = !"";           // This will === true;
$s = !"hello";      // This will === false;
?>
```


To cast as if using a (bool) you can NOT the NOT with "!!" (double '!'), then you are casting to the correct (bool).

Example:



```
<?php
$a = !!array();   // This will === false; (as expected)
/* 
This can be a substitute for count($array) > 0 or !(empty($array)) to check to see if an array is empty or not  (you would use: !!$array).
*/

$status = (!!$array ? 'complete' : 'incomplete');

$s = !!"testing"; // This will === true; (as expected)
/* 
Note: normal casting rules apply so a !!"0" would evaluate to an === false
*/
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/language.types.boolean.php)

**[To root](/README.md)**
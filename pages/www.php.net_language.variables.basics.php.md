# Basics



This page should include a note on variable lifecycle:<br><br>Before a variable is used, it has no existence. It is unset. It is possible to check if a variable doesn&apos;t exist by using isset(). This returns true provided the variable exists and isn&apos;t set to null. With the exception of null, the value a variable holds plays no part in determining whether a variable is set. <br><br>Setting an existing variable to null is a way of unsetting a variable. Another way is variables may be destroyed by using the unset() construct. <br><br>

```
<?php
print isset($a); // $a is not set. Prints false. (Or more accurately prints &apos;&apos;.)
$b = 0; // isset($b) returns true (or more accurately &apos;1&apos;)
$c = array(); // isset($c) returns true
$b = null; // Now isset($b) returns false;
unset($c); // Now isset($c) returns false;
?>
```


is_null() is an equivalent test to checking that isset() is false.

The first time that a variable is used in a scope, it&apos;s automatically created. After this isset is true. At the point at which it is created it also receives a type according to the context.



```
<?php
$a_bool = true;   // a boolean
$a_str = &apos;foo&apos;;    // a string
?>
```


If it is used without having been given a value then it is uninitalized and it receives the default value for the type. The default values are the _empty_ values. E.g  Booleans default to FALSE, integers and floats default to zero, strings to the empty string &apos;&apos;, arrays to the empty array.

A variable can be tested for emptiness using empty();



```
<?php
$a = 0; //This isset, but is empty
?>
```


Unset variables are also empty.



```
<?php
empty($vessel); // returns true. Also $vessel is unset.
?>
```


Everything above applies to array elements too. 



```
<?php
$item = array(); 
//Now isset($item) returns true. But isset($item[&apos;unicorn&apos;]) is false.
//empty($item) is true, and so is empty($item[&apos;unicorn&apos;]

$item[&apos;unicorn&apos;] = &apos;&apos;;
//Now isset($item[&apos;unicorn&apos;]) is true. And empty($item) is false. 
//But empty($item[&apos;unicorn&apos;]) is still true;

$item[&apos;unicorn&apos;] = &apos;Pink unicorn&apos;;
//isset($item[&apos;unicorn&apos;]) is still true. And empty($item) is still false. 
//But now empty($item[&apos;unicorn&apos;]) is false;
?>
```
<br><br>For arrays, this is important because accessing a non-existent array item can trigger errors; you may want to test arrays and array items for existence with isset before using them.  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.basics.php)

**[To root](/README.md)**
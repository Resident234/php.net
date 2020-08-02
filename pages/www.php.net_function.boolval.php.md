# boolval





```
<?php

// Hack for old php versions to use boolval()

if (!function_exists('boolval')) {
        function boolval($val) {
                return (bool) $val;
        }
}
?>
```
  

---

To anyone like me who came here looking for a way to turn any value into a 0/1 that will fit into a MySQL boolean (tinyint) field:<br><br>

```
<?php
$tinyint = (int) filter_var($valToCheck, FILTER_VALIDATE_BOOLEAN);
?>
```
<br><br>tinyint will be 0 (zero) for values like string "false", boolean false, int 0<br><br>tinyint will be 1 for values like string "true", boolean true, int 1<br><br>Useful if you are accepting data that might be from a language like Javascript that sends string "false" for a boolean false.  

---

I believe that the double negation !! can perform the same task with the same result if your PHP is not up2date<br><br>var_dump(!!1, !!0, !!"test", !!"");<br><br>outputs:<br>boolean true<br><br>boolean false<br><br>boolean true<br><br>boolean false<br><br>May the life be good to you.  

---

function is_true($val, $return_null=false){<br>    $boolval = ( is_string($val) ? filter_var($val, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) : (bool) $val );<br>    return ( $boolval===null &amp;&amp; !$return_null ? false : $boolval );<br>}<br><br>// Return Values:<br><br>is_true(new stdClass);      // true<br>is_true([1,2]);             // true<br>is_true([1]);               // true<br>is_true([0]);               // true<br>is_true(42);                // true<br>is_true(-42);               // true<br>is_true(&apos;true&apos;);            // true<br>is_true(&apos;on&apos;)               // true<br>is_true(&apos;off&apos;)              // false<br>is_true(&apos;yes&apos;)              // true<br>is_true(&apos;no&apos;)               // false<br>is_true(&apos;ja&apos;)               // false<br>is_true(&apos;nein&apos;)             // false<br>is_true(&apos;1&apos;);               // true<br>is_true(NULL);              // false<br>is_true(0);                 // false<br>is_true(&apos;false&apos;);           // false<br>is_true(&apos;string&apos;);          // false<br>is_true(&apos;0.0&apos;);             // false<br>is_true(&apos;4.2&apos;);             // false<br>is_true(&apos;0&apos;);               // false<br>is_true(&apos;&apos;);                // false<br>is_true([]);                // false  

---

[Official documentation page](https://www.php.net/manual/en/function.boolval.php)

**[To root](/README.md)**
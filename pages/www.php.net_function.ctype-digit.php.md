# ctype_digit



All basic PHP functions which i tried returned unexpected results. I would just like to check whether some variable only contains numbers. For example: when i spread my script to the public i cannot require users to only use numbers as string or as integer. For those situation i wrote my own function which handles all inconveniences of other functions and which is not depending on regular expressions. Some people strongly believe that regular functions slow down your script.<br><br>The reason to write this function:<br>1. is_numeric() accepts values like: +0123.45e6 (but you would expect it would not)<br>2. is_int() does not accept HTML form fields (like: 123) because they are treated as strings (like: "123").<br>3. ctype_digit() excepts all numbers to be strings (like: "123") and does not validate real integers (like: 123).<br>4. Probably some functions would parse a boolean (like: true or false) as 0 or 1 and validate it in that manner.<br><br>My function only accepts numbers regardless whether they are in string or in integer format.<br>

```
<?php
    /**
     * Check input for existing only of digits (numbers)
     * @author Tim Boormans <info@directwebsolutions.nl>
     * @param $digit
     * @return bool
     */
    function is_digit($digit) {
        if(is_int($digit)) {
            return true;
        } elseif(is_string($digit)) {
            return ctype_digit($digit);
        } else {
            // booleans, floats and others
            return false;
        }
    }
?>
```
  

---

I just wanted to clarify a flaw in the function is_digit() suggested by "info at directwebsolutions dot nl " .. <br>It returns true in case of negative integers and false in case of strings that contain negative integers .<br> example:<br>is_digit(-10); // returns ture<br>is_digit(&apos;-10&apos;); // returns false  

---

Also note that<br><br>

```
<?php ctype_digit("-1");   //false ?>
```
  

---

Interesting to note that you must pass a STRING to this function, other values won&apos;t be typecasted (I figured it would even though above explicitly says string $text).<br><br>I.E.<br><br>

```
<?phpPHP
$val = 42; //Answer to life
$x = ctype_digit($val);
?>
```


Will return false, even though, when typecasted to string, it would be true.



```
<?phpPHP
$val = '42';
$x = ctype_digit($val);
?>
```


Returns True.

Could do this too:



```
<?phpPHP
$val = 42;
$x = ctype_digit((string) $val);
?>
```
<br><br>Which will also return true, as it should.  

---

ctype_digit() will treat all passed integers below 256 as character-codes. It returns true for 48 through 57 (ASCII &apos;0&apos;-&apos;9&apos;) and false for the rest.<br><br>ctype_digit(5) -&gt; false<br>ctype_digit(48) -&gt; true<br>ctype_digit(255) -&gt; false<br>ctype_digit(256) -&gt; true<br><br>(Note: the PHP type must be an int; if you pass strings it works as expected)  

---

Note that an empty string is also false:<br>ctype_digit("") // false  

---

[Official documentation page](https://www.php.net/manual/en/function.ctype-digit.php)

**[To root](/README.md)**
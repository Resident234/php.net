# is_int



I&apos;ve found that both that is_int and ctype_digit don&apos;t behave quite as I&apos;d expect, so I made a simple function called isInteger which does. I hope somebody finds it useful.<br><br>

```
<?php
function isInteger($input){
    return(ctype_digit(strval($input)));
}

var_dump(is_int(23)); //bool(true)
var_dump(is_int("23")); //bool(false)
var_dump(is_int(23.5)); //bool(false)
var_dump(is_int(NULL)); //bool(false)
var_dump(is_int("")); //bool(false)

var_dump(ctype_digit(23)); //bool(true)
var_dump(ctype_digit("23")); //bool(false)
var_dump(ctype_digit(23.5)); //bool(false)
var_dump(ctype_digit(NULL)); //bool(false)
var_dump(ctype_digit("")); //bool(true)

var_dump(isInteger(23)); //bool(true)
var_dump(isInteger("23")); //bool(true)
var_dump(isInteger(23.5)); //bool(false)
var_dump(isInteger(NULL)); //bool(false)
var_dump(isInteger("")); //bool(false)
?>
```
  

#

Keep in mind that is_int() operates in signed fashion, not unsigned, and is limited to the word size of the environment php is running in.<br><br>In a 32-bit environment:<br><br>

```
<?php
is_int( 2147483647 );           // true
is_int( 2147483648 );           // false
is_int( 9223372036854775807 );  // false
is_int( 9223372036854775808 );  // false
?>
```


In a 64-bit environment:



```
<?php
is_int( 2147483647 );           // true
is_int( 2147483648 );           // true
is_int( 9223372036854775807 );  // true
is_int( 9223372036854775808 );  // false
?>
```


If you find yourself deployed in a 32-bit environment where you are required to deal with numeric confirmation of integers (and integers only) potentially breaching the 32-bit span, you can combine is_int() with is_float() to guarantee a cover of the full, signed 64-bit span:



```
<?php
$small = 2147483647;         // will always be true for is_int(), but never for is_float()
$big = 9223372036854775807;  // will only be true for is_int() in a 64-bit environment

if( is_int($small) || is_float($small) );  // passes in a 32-bit environment
if( is_int($big) || is_float($big) );      // passes in a 32-bit environment
?>
```
  

#

Simon Neaves was close on explaining why his function is perfect choice for testing for an int (as possibly most people would need).  He made some errors on his ctype_digit() output though - possibly a typo, or maybe a bug in his version of PHP at the time.<br><br>The correct output for parts of his examples should be:<br><br>

```
<?php
var_dump(ctype_digit(23)); //bool(false)
var_dump(ctype_digit("23")); //bool(true)
var_dump(ctype_digit(23.5)); //bool(false)
var_dump(ctype_digit(NULL)); //bool(false)
var_dump(ctype_digit("")); //bool(false)
?>
```
<br><br>As you can see, the reason why using *just* ctype_digit() may not always work is because it only returns TRUE when given a string as input - given a number value and it returns FALSE (which may be unexpected).  

#

With this function you can check if every of multiple variables are int. This is a little more comfortable than writing &apos;is_int&apos; for every variable you&apos;ve got.<br><br>

```
<?php
function are_int ( ) {
    $args = func_get_args ();
    foreach ( $args as $arg )
        if ( ! is_int ( $arg ) )
            return false;
    return true;
}

// Example:
are_int ( 4, 9 ); // true
are_int ( 22, 08, &apos;foo&apos; ); // false
?>
```
  

#

Just a shorter way to check if your variable is an int or a string containing a int without others digit than 0 to 9 :<br><br>

```
<?php 
$bool = ( !is_int($value) ? (ctype_digit($value)) : true );

$value = 42; //true
$value = &apos;42&apos;; //true
$value = &apos;1e9&apos;; //false
$value = &apos;0155&apos;; //true
$value = 0155; //true
$value = 0xFF; //true while it&apos;s just the same as 255
$value = &apos;0xFF&apos;; //false
$value = &apos;a&apos;; //false
$value = array(); //false
$value = array(&apos;5&apos;); //false
$value = array(5); false
$value = &apos;&apos;; //false
$value = NULL; //false

?>
```
<br><br>Short &amp; cool :)  

#

[Official documentation page](https://www.php.net/manual/en/function.is-int.php)

**[To root](/README.md)**
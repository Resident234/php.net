# strlen



I want to share something seriously important for newbies or beginners of PHP who plays with strings of UTF8 encoded characters or the languages like: Arabic, Persian, Pashto, Dari, Chinese (simplified), Chinese (traditional), Japanese, Vietnamese, Urdu, Macedonian, Lithuanian, and etc.<br>As the manual says: "strlen() returns the number of bytes rather than the number of characters in a string.", so if you want to get the number of characters in a string of UTF8 so use mb_strlen() instead of strlen().<br><br>Example:<br><br>

```
<?php
// the Arabic (Hello) string below is: 59 bytes and 32 characters
$utf8 = "&#x627;&#x644;&#x633;&#x644;&#x627;&#x645; &#x639;&#x644;&#x6CC;&#x6A9;&#x645; &#x648;&#x631;&#x62D;&#x645;&#x629; &#x627;&#x644;&#x644;&#x647; &#x648;&#x628;&#x631;&#x6A9;&#x627;&#x62A;&#x647;!";

var_export( strlen($utf8) ); // 59
echo "<br>";
var_export( mb_strlen($utf8, 'utf8') ); // 32
?>
```
  

#

The easiest way to determine the character count of a UTF8 string is to pass the text through utf8_decode() first:<br><br>

```
<?php
$length = strlen(utf8_decode($s));
?>
```
<br><br>utf8_decode() converts characters that are not in ISO-8859-1 to &apos;?&apos;, which, for the purpose of counting, is quite alright.  

#

We just ran into what we thought was a bug but turned out to be a documented difference in behavior between PHP 5.2 &amp; 5.3.  Take the following code example:<br><br>

```
<?php

$attributes = array('one', 'two', 'three');

if (strlen($attributes) == 0 &amp;&amp; !is_bool($attributes)) {
    echo "We are in the 'if'\n";  //  PHP 5.3
} else {
    echo "We are in the 'else'\n";  //  PHP 5.2
}

?>
```
<br><br>This is because in 5.2 strlen will automatically cast anything passed to it as a string, and casting an array to a string yields the string "Array".  In 5.3, this changed, as noted in the following point in the backward incompatible changes in 5.3 (http://www.php.net/manual/en/migration53.incompatible.php):<br><br>"The newer internal parameter parsing API has been applied across all the extensions bundled with PHP 5.3.x. This parameter parsing API causes functions to return NULL when passed incompatible parameters. There are some exceptions to this rule, such as the get_class() function, which will continue to return FALSE on error."<br><br>So, in PHP 5.3, strlen($attributes) returns NULL, while in PHP 5.2, strlen($attributes) returns the integer 5.  This likely affects other functions, so if you are getting different behaviors or new bugs suddenly, check if you have upgraded to 5.3 (which we did recently), and then check for some warnings in your logs like this:<br><br>strlen() expects parameter 1 to be string, array given in /var/www/sis/lib/functions/advanced_search_lib.php on line 1028<br><br>If so, then you are likely experiencing this changed behavior.  

#

I would like to demonstrate that you need more than just this function in order to truly test for an empty string. The reason being that 

```
<?php strlen(null); ?>
```
 will return 0. So how do you know if the value was null, or truly an empty string?



```
<?php
$foo = null;
$len = strlen(null);
$bar = '';

echo "Length: " . strlen($foo) . "<br>";
echo "Length: $len <br>";
echo "Length: " . strlen(null) . "<br>";

if (strlen($foo) === 0) echo 'Null length is Zero <br>';
if ($len === 0) echo 'Null length is still Zero <br>';

if (strlen($foo) == 0 &amp;&amp; !is_null($foo)) echo '!is_null(): $foo is truly an empty string <br>';
else echo '!is_null(): $foo is probably null <br>';

if (strlen($foo) == 0 &amp;&amp; isset($foo)) echo 'isset(): $foo is truly an empty string <br>';
else echo 'isset(): $foo is probably null <br>';

if (strlen($bar) == 0 &amp;&amp; !is_null($bar)) echo '!is_null(): $bar is truly an empty string <br>';
else echo '!is_null(): $foo is probably null <br>';

if (strlen($bar) == 0 &amp;&amp; isset($bar)) echo 'isset(): $bar is truly an empty string <br>';
else echo 'isset(): $foo is probably null <br>';
?>
```
<br><br>// Begin Output:<br>Length: 0<br>Length: 0 <br>Length: 0<br><br>Null length is Zero <br>Null length is still Zero <br><br>!is_null(): $foo is probably null <br>isset(): $foo is probably null <br><br>!is_null(): $bar is truly an empty string <br>isset(): $bar is truly an empty string <br>// End Output<br><br>So it would seem you need either is_null() or isset() in addition to strlen() if you care whether or not the original value was null.  

#

PHP&apos;s strlen function behaves differently than the C strlen function in terms of its handling of null bytes (&apos;\0&apos;).  <br><br>In PHP, a null byte in a string does NOT count as the end of the string, and any null bytes are included in the length of the string.<br><br>For example, in PHP:<br><br>strlen( "te\0st" ) = 5<br><br>In C, the same call would return 2.<br><br>Thus, PHP&apos;s strlen function can be used to find the number of bytes in a binary string (for example, binary data returned by base64_decode).  

#

Attention with utf8:<br>$foo = "b&#xE4;r";<br>strlen($foo) will return 4 and not 3 as expected..  

#

[Official documentation page](https://www.php.net/manual/en/function.strlen.php)

**[To root](/README.md)**
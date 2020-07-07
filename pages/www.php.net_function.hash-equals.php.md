# hash_equals





To transparently support this function on older versions of PHP use this:



```
<?php
if(!function_exists(&apos;hash_equals&apos;)) {
&#xA0; function hash_equals($str1, $str2) {
&#xA0; &#xA0; if(strlen($str1) != strlen($str2)) {
&#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; $res = $str1 ^ $str2;
&#xA0; &#xA0; &#xA0; $ret = 0;
&#xA0; &#xA0; &#xA0; for($i = strlen($res) - 1; $i &gt;= 0; $i--) $ret |= ord($res[$i]);
&#xA0; &#xA0; &#xA0; return !$ret;
&#xA0; &#xA0; }
&#xA0; }
}
?>
```



  

#



I don&apos;t know why asphp at dsgml dot com got that many downvotes, the function seems to work.

I extended it a bit to support strings of diffent length and to handle errors and ran some tests:

The test results and how to reproduce them: http://pastebin.com/mLMXJeva

The function:


```
<?php

if (!function_exists(&apos;hash_equals&apos;)) {

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Timing attack safe string comparison
&#xA0; &#xA0;&#xA0; * 
&#xA0; &#xA0;&#xA0; * Compares two strings using the same time whether they&apos;re equal or not.
&#xA0; &#xA0;&#xA0; * This function should be used to mitigate timing attacks; for instance, when testing crypt() password hashes.
&#xA0; &#xA0;&#xA0; * 
&#xA0; &#xA0;&#xA0; * @param string $known_string The string of known length to compare against
&#xA0; &#xA0;&#xA0; * @param string $user_string The user-supplied string
&#xA0; &#xA0;&#xA0; * @return boolean Returns TRUE when the two strings are equal, FALSE otherwise.
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; function hash_equals($known_string, $user_string)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (func_num_args() !== 2) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // handle wrong parameter count as the native implentation
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&apos;hash_equals() expects exactly 2 parameters, &apos; . func_num_args() . &apos; given&apos;, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return null;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if (is_string($known_string) !== true) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&apos;hash_equals(): Expected known_string to be a string, &apos; . gettype($known_string) . &apos; given&apos;, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $known_string_len = strlen($known_string);
&#xA0; &#xA0; &#xA0; &#xA0; $user_string_type_error = &apos;hash_equals(): Expected user_string to be a string, &apos; . gettype($user_string) . &apos; given&apos;; // prepare wrong type error message now to reduce the impact of string concatenation and the gettype call
&#xA0; &#xA0; &#xA0; &#xA0; if (is_string($user_string) !== true) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; trigger_error($user_string_type_error, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // prevention of timing attacks might be still possible if we handle $user_string as a string of diffent length (the trigger_error() call increases the execution time a bit)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $user_string_len = strlen($user_string);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $user_string_len = $known_string_len + 1;
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $user_string_len = $known_string_len + 1;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $user_string_len = strlen($user_string);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if ($known_string_len !== $user_string_len) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $res = $known_string ^ $known_string; // use $known_string instead of $user_string to handle strings of diffrent length.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ret = 1; // set $ret to 1 to make sure false is returned
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $res = $known_string ^ $user_string;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ret = 0;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; for ($i = strlen($res) - 1; $i &gt;= 0; $i--) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ret |= ord($res[$i]);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $ret === 0;
&#xA0; &#xA0; }

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.hash-equals.php)

**[To root](/README.md)**
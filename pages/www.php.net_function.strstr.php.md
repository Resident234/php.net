# strstr



strstr() is not a way to avoid type-checking with strpos().<br><br>If $needle is the last character in $haystack, and testing $needle as a boolean by itself would evaluate to false, then testing strstr() as a boolean will evaluate to false (because, if successful, strstr() returns the first occurrence of $needle along with the rest of $haystack).<br><br>

```
<?php
findZero(&apos;01234&apos;);  // found a zero
findZero(&apos;43210&apos;);  // did not find a zero
findZero(&apos;0&apos;);      // did not find a zero
findZero(&apos;00&apos;);     // found a zero
findZero(&apos;000&apos;);    // found a zero
findZero(&apos;10&apos;);     // did not find a zero
findZero(&apos;100&apos;);    // found a zero

function findZero($numberString) {
    if (strstr($numberString, &apos;0&apos;)) {
        echo &apos;found a zero&apos;;
    } else {
        echo &apos;did not find a zero&apos;;
    }
}
?>
```
<br><br>Also, strstr() is far more memory-intensive than strpos(), especially with longer strings as your $haystack, so if you are not interested in the substring that strstr() returns, you shouldn&apos;t be using it anyway. <br><br>There is no PHP function just to check only _if_ $needle occurs in $haystack; strpos() tells you if it _doesn&apos;t_ by returning false, but, if it does occur, it tells you _where_ it occurs as an integer, which is 0 (zero) if $needle is the first part of $haystack, which is why testing if (strpos($needle, $haystack)===false) is the only way to know for sure if $needle is not part of $haystack.<br><br>My advice is to start loving type checking immediately, and to familiarize yourself with the return value of the functions you are using.<br><br>Cheers.  

#

Been using this for years:<br><br>

```
<?php
/**
*
* @author : Dennis T Kaplan
*
* @version : 1.0
* Date : June 17, 2007
* Function : reverse strstr()
* Purpose : Returns part of haystack string from start to the first occurrence of needle
* $haystack = &apos;this/that/whatever&apos;;
* $result = rstrstr($haystack, &apos;/&apos;)
* $result == this
*
* @access public
* @param string $haystack, string $needle
* @return string
**/

function rstrstr($haystack,$needle)
    {
        return substr($haystack, 0,strpos($haystack, $needle));
    }
?>
```


You could change it to:
rstrstr ( string $haystack , mixed $needle [, int $start] )


```
<?php

function rstrstr($haystack,$needle, $start=0)
    {
        return substr($haystack, $start,strpos($haystack, $needle));
    }

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.strstr.php)

**[To root](/README.md)**
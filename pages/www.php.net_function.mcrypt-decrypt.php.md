# mcrypt_decrypt



It appears that mcrypt_decrypt pads the *RETURN STRING* with nulls (&apos;\0&apos;) to fill out to n * blocksize.  For old C-programmers, like myself, it is easy to believe the string ends at the first null.  In PHP it does not:<br><br>    strlen("abc\0\0") returns 5 and *NOT* 3<br>    strcmp("abc", "abc\0\0") returns -2 and *NOT* 0<br><br>I learned this lesson painfully when I passed a string returned from mycrypt_decrypt into a NuSoap message, which happily passed the nulls along to the receiver, who couldn&apos;t figure out what I was talking about.<br><br>My solution was:<br>

```
<?php
    $retval = mcrypt_decrypt( ...etc ...);
    $retval = rtrim($retval, "\0");     // trim ONLY the nulls at the END
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.mcrypt-decrypt.php)

**[To root](/README.md)**
# Predefined Constants



[Editor&apos;s note: fixed E_COMPILE_* cases that incorrectly returned E_CORE_* strings. Thanks josiebgoode.]<br><br>The following code expands on Vlad&apos;s code to show all the flags that are set.  if not set, a blank line shows.<br><br>

```
<?php
$errLvl = error_reporting();
for ($i = 0; $i < 15;  $i++ ) {
    print FriendlyErrorType($errLvl &amp; pow(2, $i)) . "<br>\\n"; 
}

function FriendlyErrorType($type)
{
    switch($type)
    {
        case E_ERROR: // 1 //
            return 'E_ERROR';
        case E_WARNING: // 2 //
            return 'E_WARNING';
        case E_PARSE: // 4 //
            return 'E_PARSE';
        case E_NOTICE: // 8 //
            return 'E_NOTICE';
        case E_CORE_ERROR: // 16 //
            return 'E_CORE_ERROR';
        case E_CORE_WARNING: // 32 //
            return 'E_CORE_WARNING';
        case E_COMPILE_ERROR: // 64 //
            return 'E_COMPILE_ERROR';
        case E_COMPILE_WARNING: // 128 //
            return 'E_COMPILE_WARNING';
        case E_USER_ERROR: // 256 //
            return 'E_USER_ERROR';
        case E_USER_WARNING: // 512 //
            return 'E_USER_WARNING';
        case E_USER_NOTICE: // 1024 //
            return 'E_USER_NOTICE';
        case E_STRICT: // 2048 //
            return 'E_STRICT';
        case E_RECOVERABLE_ERROR: // 4096 //
            return 'E_RECOVERABLE_ERROR';
        case E_DEPRECATED: // 8192 //
            return 'E_DEPRECATED';
        case E_USER_DEPRECATED: // 16384 //
            return 'E_USER_DEPRECATED';
    }
    return "";
}
?>
```
  

---

-1 is also semantically meaningless as a bit field, and only works in 2s-complement numeric representations.  On a 1s-complement system -1 would not set E_ERROR.  On a sign-magnitude system -1 would set nothing at all! (see e.g. http://en.wikipedia.org/wiki/Ones%27_complement)<br><br>If you want to set all bits, ~0 is the correct way to do it.<br><br>But setting undefined bits could result in undefined behaviour and that means *absolutely anything* could happen :-)  

---

[Official documentation page](https://www.php.net/manual/en/errorfunc.constants.php)

**[To root](/README.md)**
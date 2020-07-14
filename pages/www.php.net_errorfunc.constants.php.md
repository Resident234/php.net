# Predefined Constants



[Editor&apos;s note: fixed E_COMPILE_* cases that incorrectly returned E_CORE_* strings. Thanks josiebgoode.]<br><br>The following code expands on Vlad&apos;s code to show all the flags that are set.  if not set, a blank line shows.<br><br>

```
<?php
$errLvl = error_reporting();
for ($i = 0; $i &lt; 15;  $i++ ) {
    print FriendlyErrorType($errLvl &amp; pow(2, $i)) . "&lt;br&gt;\\n"; 
}

function FriendlyErrorType($type)
{
    switch($type)
    {
        case E_ERROR: // 1 //
            return &apos;E_ERROR&apos;;
        case E_WARNING: // 2 //
            return &apos;E_WARNING&apos;;
        case E_PARSE: // 4 //
            return &apos;E_PARSE&apos;;
        case E_NOTICE: // 8 //
            return &apos;E_NOTICE&apos;;
        case E_CORE_ERROR: // 16 //
            return &apos;E_CORE_ERROR&apos;;
        case E_CORE_WARNING: // 32 //
            return &apos;E_CORE_WARNING&apos;;
        case E_COMPILE_ERROR: // 64 //
            return &apos;E_COMPILE_ERROR&apos;;
        case E_COMPILE_WARNING: // 128 //
            return &apos;E_COMPILE_WARNING&apos;;
        case E_USER_ERROR: // 256 //
            return &apos;E_USER_ERROR&apos;;
        case E_USER_WARNING: // 512 //
            return &apos;E_USER_WARNING&apos;;
        case E_USER_NOTICE: // 1024 //
            return &apos;E_USER_NOTICE&apos;;
        case E_STRICT: // 2048 //
            return &apos;E_STRICT&apos;;
        case E_RECOVERABLE_ERROR: // 4096 //
            return &apos;E_RECOVERABLE_ERROR&apos;;
        case E_DEPRECATED: // 8192 //
            return &apos;E_DEPRECATED&apos;;
        case E_USER_DEPRECATED: // 16384 //
            return &apos;E_USER_DEPRECATED&apos;;
    }
    return "";
}
?>
```
  

#

-1 is also semantically meaningless as a bit field, and only works in 2s-complement numeric representations.  On a 1s-complement system -1 would not set E_ERROR.  On a sign-magnitude system -1 would set nothing at all! (see e.g. http://en.wikipedia.org/wiki/Ones%27_complement)<br><br>If you want to set all bits, ~0 is the correct way to do it.<br><br>But setting undefined bits could result in undefined behaviour and that means *absolutely anything* could happen :-)  

#

[Official documentation page](https://www.php.net/manual/en/errorfunc.constants.php)

**[To root](/README.md)**
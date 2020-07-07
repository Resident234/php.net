# Predefined Constants





[Editor&apos;s note: fixed E_COMPILE_* cases that incorrectly returned E_CORE_* strings. Thanks josiebgoode.]



The following code expands on Vlad&apos;s code to show all the flags that are set.&#xA0; if not set, a blank line shows.





```
<?php

$errLvl = error_reporting();

for ($i = 0; $i &lt; 15;&#xA0; $i++ ) {

&#xA0; &#xA0; print FriendlyErrorType($errLvl &amp; pow(2, $i)) . &quot;&lt;br&gt;\\n&quot;; 

}



function FriendlyErrorType($type)

{

&#xA0; &#xA0; switch($type)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; case E_ERROR: // 1 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_ERROR&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_WARNING: // 2 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_WARNING&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_PARSE: // 4 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_PARSE&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_NOTICE: // 8 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_NOTICE&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_CORE_ERROR: // 16 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_CORE_ERROR&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_CORE_WARNING: // 32 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_CORE_WARNING&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_COMPILE_ERROR: // 64 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_COMPILE_ERROR&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_COMPILE_WARNING: // 128 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_COMPILE_WARNING&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_ERROR: // 256 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_USER_ERROR&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_WARNING: // 512 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_USER_WARNING&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_NOTICE: // 1024 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_USER_NOTICE&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_STRICT: // 2048 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_STRICT&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_RECOVERABLE_ERROR: // 4096 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_RECOVERABLE_ERROR&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_DEPRECATED: // 8192 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_DEPRECATED&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_DEPRECATED: // 16384 //

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;E_USER_DEPRECATED&apos;;

&#xA0; &#xA0; }

&#xA0; &#xA0; return &quot;&quot;;

}

?>
```



  

#



-1 is also semantically meaningless as a bit field, and only works in 2s-complement numeric representations.&#xA0; On a 1s-complement system -1 would not set E_ERROR.&#xA0; On a sign-magnitude system -1 would set nothing at all! (see e.g. http://en.wikipedia.org/wiki/Ones%27_complement)

If you want to set all bits, ~0 is the correct way to do it.

But setting undefined bits could result in undefined behaviour and that means *absolutely anything* could happen :-)

  

#

[Official documentation page](https://www.php.net/manual/en/errorfunc.constants.php)

**[To root](/README.md)**
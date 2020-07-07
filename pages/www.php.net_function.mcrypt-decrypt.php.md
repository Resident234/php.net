# mcrypt_decrypt





It appears that mcrypt_decrypt pads the *RETURN STRING* with nulls (&apos;\0&apos;) to fill out to n * blocksize.&#xA0; For old C-programmers, like myself, it is easy to believe the string ends at the first null.&#xA0; In PHP it does not:



&#xA0; &#xA0; strlen(&quot;abc\0\0&quot;) returns 5 and *NOT* 3

&#xA0; &#xA0; strcmp(&quot;abc&quot;, &quot;abc\0\0&quot;) returns -2 and *NOT* 0



I learned this lesson painfully when I passed a string returned from mycrypt_decrypt into a NuSoap message, which happily passed the nulls along to the receiver, who couldn&apos;t figure out what I was talking about.



My solution was:



```
<?php

&#xA0; &#xA0; $retval = mcrypt_decrypt( ...etc ...);

&#xA0; &#xA0; $retval = rtrim($retval, &quot;\0&quot;);&#xA0; &#xA0;&#xA0; // trim ONLY the nulls at the END

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.mcrypt-decrypt.php)

**[To root](/README.md)**
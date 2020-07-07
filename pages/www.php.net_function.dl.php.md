# dl





dl is awkward because the filename format is OS-dependent and because it can complain if the extension is already loaded. This wrapper function fixes that:



```
<?php

function load_lib($n, $f = null) {
&#xA0; &#xA0; return extension_loaded($n) or dl(((PHP_SHLIB_SUFFIX === &apos;dll&apos;) ? &apos;php_&apos; : &apos;&apos;) . ($f ? $f : $n) . &apos;.&apos; . PHP_SHLIB_SUFFIX);
}

php?>
```


Examples:



```
<?php

// ensure we have SSL and MySQL support
load_lib(&apos;openssl&apos;);
load_lib(&apos;mysql&apos;);

// a rare few extensions have a different filename to their extension name, such as the image (gd) library, so we specify them like this:
load_lib(&apos;gd&apos;, &apos;gd2&apos;);

php?>
```



  

#



It would be nice to know which SAPIs removed the function. 

Telling me &apos;This function has been removed from some SAPIs in PHP 5.3.&apos; is pretty much useless and I feel mocked. Or do the writers of the documentation don&apos;t know from which SAPIs it has been removed?

  

#

[Official documentation page](https://www.php.net/manual/en/function.dl.php)

**[To root](/README.md)**
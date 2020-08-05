# dl



dl is awkward because the filename format is OS-dependent and because it can complain if the extension is already loaded. This wrapper function fixes that:<br><br>

```
<?php

function load_lib($n, $f = null) {
    return extension_loaded($n) or dl(((PHP_SHLIB_SUFFIX === 'dll') ? 'php_' : '') . ($f ? $f : $n) . '.' . PHP_SHLIB_SUFFIX);
}

?>
```


Examples:



```
<?php

// ensure we have SSL and MySQL support
load_lib('openssl');
load_lib('mysql');

// a rare few extensions have a different filename to their extension name, such as the image (gd) library, so we specify them like this:
load_lib('gd', 'gd2');

?>
```
  

---

It would be nice to know which SAPIs removed the function. <br><br>Telling me &apos;This function has been removed from some SAPIs in PHP 5.3.&apos; is pretty much useless and I feel mocked. Or do the writers of the documentation don&apos;t know from which SAPIs it has been removed?  

---

[Official documentation page](https://www.php.net/manual/en/function.dl.php)

**[To root](/README.md)**
# strcasecmp



A simple multibyte-safe case-insensitive string comparison:<br><br>

```
<?php

function mb_strcasecmp($str1, $str2, $encoding = null) {
    if (null === $encoding) { $encoding = mb_internal_encoding(); }
    return strcmp(mb_strtoupper($str1, $encoding), mb_strtoupper($str2, $encoding));
}

?>
```
<br><br>Caveat: watch out for edge cases like "&#xDF;".  

#

The sample above is only true on some platforms that only use a simple &apos;C&apos; locale, where individual bytes are considered as complete characters that are converted to lowercase before being differentiated.<br><br>Other locales (see LC_COLLATE and LC_ALL) use the difference of collation order of characters, where characters may be groups of bytes taken from the input strings, or simply return -1, 0, or 1 as the collation order is not simply defined by comparing individual characters but by more complex rules.<br><br>Don&apos;t base your code on a specific non null value returned by strcmp() or strcasecmp(): it is not portable. Just consider the sign of the result and be sure to use the correct locale!  

#

[Official documentation page](https://www.php.net/manual/en/function.strcasecmp.php)

**[To root](/README.md)**
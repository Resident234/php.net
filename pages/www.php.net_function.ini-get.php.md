# ini_get





another version of return_bytes which returns faster and does not use multiple multiplications (sorry:). even if it is resolved at compile time it is not a good practice;
no local variables are allocated;
the trim() is omitted (php already trimmed values when reading php.ini file);
strtolower() is replaced by second case which wins us one more function call for the price of doubling the number of cases to process (may slower the worst-case scenario when ariving to default: takes six comparisons instead of three comparisons and a function call);
cases are ordered by most frequent goes first (uppercase M-values being the default sizes);
specs say we must handle integer sizes so float values are converted to integers and 0.8G becomes 0;
&apos;Gb&apos;, &apos;Mb&apos;, &apos;Kb&apos; shorthand byte options are not implemented since are not in specs, see
http://www.php.net/manual/en/faq.using.php#faq.using.shorthandbytes



```
<?php
function return_bytes ($size_str)
{
&#xA0; &#xA0; switch (substr ($size_str, -1))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; case &apos;M&apos;: case &apos;m&apos;: return (int)$size_str * 1048576;
&#xA0; &#xA0; &#xA0; &#xA0; case &apos;K&apos;: case &apos;k&apos;: return (int)$size_str * 1024;
&#xA0; &#xA0; &#xA0; &#xA0; case &apos;G&apos;: case &apos;g&apos;: return (int)$size_str * 1073741824;
&#xA0; &#xA0; &#xA0; &#xA0; default: return $size_str;
&#xA0; &#xA0; }
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.ini-get.php)

**[To root](/README.md)**
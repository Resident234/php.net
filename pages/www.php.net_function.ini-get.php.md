# ini_get



another version of return_bytes which returns faster and does not use multiple multiplications (sorry:). even if it is resolved at compile time it is not a good practice;<br>no local variables are allocated;<br>the trim() is omitted (php already trimmed values when reading php.ini file);<br>strtolower() is replaced by second case which wins us one more function call for the price of doubling the number of cases to process (may slower the worst-case scenario when ariving to default: takes six comparisons instead of three comparisons and a function call);<br>cases are ordered by most frequent goes first (uppercase M-values being the default sizes);<br>specs say we must handle integer sizes so float values are converted to integers and 0.8G becomes 0;<br>&apos;Gb&apos;, &apos;Mb&apos;, &apos;Kb&apos; shorthand byte options are not implemented since are not in specs, see<br>http://www.php.net/manual/en/faq.using.php#faq.using.shorthandbytes<br><br>

```
<?php
function return_bytes ($size_str)
{
    switch (substr ($size_str, -1))
    {
        case 'M': case 'm': return (int)$size_str * 1048576;
        case 'K': case 'k': return (int)$size_str * 1024;
        case 'G': case 'g': return (int)$size_str * 1073741824;
        default: return $size_str;
    }
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.ini-get.php)

**[To root](/README.md)**
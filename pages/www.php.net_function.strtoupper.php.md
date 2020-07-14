# strtoupper



One might think that setting the correct locale would do the trick with for example german umlauts, but this is not the case. You have to use mb_strtoupper() instead:<br><br>

```
<?php

setlocale(LC_CTYPE, &apos;de_DE.UTF8&apos;);

echo strtoupper(&apos;Umlaute &#xE4;&#xF6;&#xFC; in uppercase&apos;); // outputs "UMLAUTE &#xE4;&#xF6;&#xFC; IN UPPERCASE"
echo mb_strtoupper(&apos;Umlaute &#xE4;&#xF6;&#xFC; in uppercase&apos;, &apos;UTF-8&apos;); // outputs "UMLAUTE &#xC4;&#xD6;&#xDC; IN UPPERCASE"

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.strtoupper.php)

**[To root](/README.md)**
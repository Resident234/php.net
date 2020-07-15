# Transliterator::transliterate



I pretty much like the idea of hdogan, but there&apos;s at least one group of characters he&apos;s missing: ligature characters.<br>They&apos;re at least used in Norwegian and I read something about French, too ... Some are just used for styling (f.e. &#xFB01;)<br><br>Here&apos;s an example that supports all characters (should at least, according to the documentation):<br>

```
<?php
var_dump(transliterator_transliterate('Any-Latin; Latin-ASCII; Lower()', "A &#xE6; &#xDC;b&#xE9;rmensch p&#xE5; h&#xF8;yeste niv&#xE5;! &#x418; &#x44F; &#x43B;&#x44E;&#x431;&#x43B;&#x44E; PHP! &#xFB01;"));
// string(41) "a ae ubermensch pa hoyeste niva! i a lublu php! fi"
?>
```
<br><br>In this example any character will firstly be converted to a latin character. If that&apos;s finished, replace all latin characters by their ASCII replacement.  

#

[Official documentation page](https://www.php.net/manual/en/transliterator.transliterate.php)

**[To root](/README.md)**
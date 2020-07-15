# get_html_translation_table



The fact that MS-word and some other sources use CP-1252, and that it is so close to Latin1 (&apos;ISO-8859-1&apos;) causes a lot of confusion. What confused me the most was finding that mySQL uses CP-1252 by default.<br><br>You may run into trouble if you find yourself tempted to do something like this:<br>

```
<?php
    $trans[chr(149)] = '&amp;bull;';    // Bullet
    $trans[chr(150)] = '&amp;ndash;';    // En Dash
    $trans[chr(151)] = '&amp;mdash;';    // Em Dash
    $trans[chr(152)] = '&amp;tilde;';    // Small Tilde
    $trans[chr(153)] = '&amp;trade;';    // Trade Mark Sign
?>
```


Don't do it. DON'T DO IT!

You can use:


```
<?php
    $translationTable = get_html_translation_table(HTML_ENTITIES, ENT_NOQUOTES, 'WINDOWS-1252');
?>
```


or just convert directly:


```
<?php
    $output = htmlentities($input, ENT_NOQUOTES, 'WINDOWS-1252');
?>
```


But your web page is probably encoded UTF-8, and you probably don't really want CP-1252 text flying around, so fix the character encoding first:


```
<?php
    $output = mb_convert_encoding($input, 'UTF-8', 'WINDOWS-1252');
    $ouput = htmlentities($output);
?>
```
  

#

Be careful using get_html_translation_table() in a loop, as it&apos;s very slow.  

#

[Official documentation page](https://www.php.net/manual/en/function.get-html-translation-table.php)

**[To root](/README.md)**
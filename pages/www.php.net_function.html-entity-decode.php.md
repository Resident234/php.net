# html_entity_decode



If you need something that converts &amp;#[0-9]+ entities to UTF-8, this is simple and works:<br><br>

```
<?php
/* Entity crap. /
$input = "Fovi&amp;#269;";

$output = preg_replace_callback("/(&amp;#[0-9]+;)/", function($m) { return mb_convert_encoding($m[1], "UTF-8", "HTML-ENTITIES"); }, $input);

/* Plain UTF-8. */
echo $output;
?>
```
  

#

Use the following to decode all entities:<br>

```
<?php html_entity_decode($string, ENT_QUOTES | ENT_XML1, 'UTF-8') ?>
```
<br><br>I&apos;ve checked these special entities: <br>- double quotes (&amp;#34;)<br>- single quotes (&amp;#39; and &amp;apos;) <br>- non printable chars (e.g. &amp;#13;)<br>With other $flags some or all won&apos;t be decoded.<br><br>It seems that ENT_XML1 and ENT_XHTML are identical when decoding.  

#

[Official documentation page](https://www.php.net/manual/en/function.html-entity-decode.php)

**[To root](/README.md)**
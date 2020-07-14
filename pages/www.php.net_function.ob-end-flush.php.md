# ob_end_flush



A note on the above example...<br><br>with PHP 4 &gt;= 4.2.0, PHP 5 you can use a combination of ob_get_level() and ob_end_flush() to avoid using the @ (error suppresion) which should probably be a little faaster.<br><br>

```
<?php

while (ob_get_level() &gt; 0) {
    ob_end_flush();
}

?>
```
  

#

best way to compress a css code:<br><br>

```
<?php
  header(&apos;Content-type: text/css&apos;);

  ob_start("compress");
  function compress($buffer) {
    // remove comments
    $buffer = preg_replace(&apos;!/\*[^*]*\*+([^/][^*]*\*+)*/!&apos;, &apos;&apos;, $buffer);
    // remove tabs, spaces, newlines, etc.
    $buffer = str_replace(array("\r\n", "\r", "\n", "\t", &apos;  &apos;, &apos;    &apos;, &apos;    &apos;), &apos;&apos;, $buffer);
    return $buffer;
  }

  include(&apos;./template/main.css&apos;);
  include(&apos;./template/classes.css&apos;);

&lt;?php
  ob_end_flush();
?>
```
<br><br>Include in &lt;head&gt;:<br>&lt;link rel="stylesheet" type="text/css" href="/design.php" media="all" /&gt;  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-end-flush.php)

**[To root](/README.md)**
# ob_end_flush





A note on the above example...

with PHP 4 &gt;= 4.2.0, PHP 5 you can use a combination of ob_get_level() and ob_end_flush() to avoid using the @ (error suppresion) which should probably be a little faaster.



```
<?php

while (ob_get_level() &gt; 0) {
&#xA0; &#xA0; ob_end_flush();
}

php?>
```



  

#



best way to compress a css code:



```
<?php
&#xA0; header(&apos;Content-type: text/css&apos;);

&#xA0; ob_start(&quot;compress&quot;);
&#xA0; function compress($buffer) {
&#xA0; &#xA0; // remove comments
&#xA0; &#xA0; $buffer = preg_replace(&apos;!/\*[^*]*\*+([^/][^*]*\*+)*/!&apos;, &apos;&apos;, $buffer);
&#xA0; &#xA0; // remove tabs, spaces, newlines, etc.
&#xA0; &#xA0; $buffer = str_replace(array(&quot;\r\n&quot;, &quot;\r&quot;, &quot;\n&quot;, &quot;\t&quot;, &apos;&#xA0; &apos;, &apos;&#xA0; &#xA0; &apos;, &apos;&#xA0; &#xA0; &apos;), &apos;&apos;, $buffer);
&#xA0; &#xA0; return $buffer;
&#xA0; }

&#xA0; include(&apos;./template/main.css&apos;);
&#xA0; include(&apos;./template/classes.css&apos;);

&lt;?php
&#xA0; ob_end_flush();
php?>
```


Include in &lt;head&gt;:
&lt;link rel=&quot;stylesheet&quot; type=&quot;text/css&quot; href=&quot;/design.php&quot; media=&quot;all&quot; /&gt;

  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-end-flush.php)

**[To root](/README.md)**
# Predefined Constants



To get a really clean json string use these three constants like so:<br><br>

```
<?php
$array = ['&#x20AC;', 'http://example.com/some/cool/page', '337'];
$bad   = json_encode($array); 
$good  = json_encode($array,  JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES | JSON_NUMERIC_CHECK);

// $bad would be  ["\u20ac","http:\/\/example.com\/some\/cool\/page","337"]
// $good would be ["&#x20AC;","http://example.com/some/cool/page",337]
?>
```
  

---

If you curious of the numeric values of the constants, as of JSON 1.2.1, the constants have the following values (not that you should use the numbers directly):<br><br>JSON_HEX_TAG =&gt; 1<br>JSON_HEX_AMP =&gt; 2<br>JSON_HEX_APOS =&gt; 4<br>JSON_HEX_QUOT =&gt; 8<br>JSON_FORCE_OBJECT =&gt; 16<br>JSON_NUMERIC_CHECK =&gt; 32<br>JSON_UNESCAPED_SLASHES =&gt; 64<br>JSON_PRETTY_PRINT =&gt; 128<br>JSON_UNESCAPED_UNICODE =&gt; 256<br><br>JSON_ERROR_DEPTH =&gt; 1<br>JSON_ERROR_STATE_MISMATCH =&gt; 2<br>JSON_ERROR_CTRL_CHAR =&gt; 3<br><br>JSON_ERROR_SYNTAX =&gt; 4<br><br>JSON_ERROR_UTF8 =&gt; 5<br>JSON_OBJECT_AS_ARRAY =&gt; 1<br><br>JSON_BIGINT_AS_STRING =&gt; 2  

---

[Official documentation page](https://www.php.net/manual/en/json.constants.php)

**[To root](/README.md)**
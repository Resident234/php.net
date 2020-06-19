# array_key_last




<div class="phpcode"><span class="html">
For PHP &lt;= 7.3.0 :<br><br>if (! function_exists(&quot;array_key_last&quot;)) {<br>&#xA0; &#xA0; function array_key_last($array) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!is_array($array) || empty($array)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return NULL;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return array_keys($array)[count($array)-1];<br>&#xA0; &#xA0; }<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-key-last.php)

**[To root](/README.md)**
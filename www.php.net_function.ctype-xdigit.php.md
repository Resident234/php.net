# ctype_xdigit




<div class="phpcode"><span class="html">
This function shows its usefulness on a web site where a user is asked to entered a hexidecimal color code for a color. To prevent breaking W3C standard and having them enter in &quot;neon-green&quot; or the wrong type of code like 355511235.<br><br>In conjunction with strlen()&#xA0; you could create a function like this:<br><br>function check_valid_colorhex($colorCode) {<br>&#xA0; &#xA0; // If user accidentally passed along the # sign, strip it off<br>&#xA0; &#xA0; $colorCode = ltrim($colorCode, &apos;#&apos;);<br><br>&#xA0; &#xA0; if (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ctype_xdigit($colorCode) &amp;&amp;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (strlen($colorCode) == 6 || strlen($colorCode) == 3))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return true;<br><br>&#xA0; &#xA0; else return false;<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ctype-xdigit.php)

**[To root](/README.md)**
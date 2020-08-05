# urlencode



urlencode function and rawurlencode are mostly based on RFC 1738.<br><br>However, since 2005 the current RFC in use for URIs standard is RFC 3986.<br><br>Here is a function to encode URLs according to RFC 3986.<br><br>

```
<?php
function myUrlEncode($string) {
    $entities = array('%21', '%2A', '%27', '%28', '%29', '%3B', '%3A', '%40', '%26', '%3D', '%2B', '%24', '%2C', '%2F', '%3F', '%25', '%23', '%5B', '%5D');
    $replacements = array('!', '*', "'", "(", ")", ";", ":", "@", "&amp;", "=", "+", "

```
<?php<br>function myUrlEncode($string) {<br>    $entities = array(&apos;%21&apos;, &apos;%2A&apos;, &apos;%27&apos;, &apos;%28&apos;, &apos;%29&apos;, &apos;%3B&apos;, &apos;%3A&apos;, &apos;%40&apos;, &apos;%26&apos;, &apos;%3D&apos;, &apos;%2B&apos;, &apos;%24&apos;, &apos;%2C&apos;, &apos;%2F&apos;, &apos;%3F&apos;, &apos;%25&apos;, &apos;%23&apos;, &apos;%5B&apos;, &apos;%5D&apos;);<br>    $replacements = array(&apos;!&apos;, &apos;*&apos;, "&apos;", "(", ")", ";", ":", "@", "&amp;", "=", "+", "$", ",", "/", "?", "%", "#", "[", "]");<br>    return str_replace($entities, $replacements, urlencode($string));<br>}<br>?>
```
quot;, ",", "/", "?", "%", "#", "[", "]");
    return str_replace($entities, $replacements, urlencode($string));
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.urlencode.php)

**[To root](/README.md)**
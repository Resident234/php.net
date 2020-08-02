# simplexml_load_string



I had a hard time finding this documented, so posting it here in case it helps someone:<br><br>If you want to use multiple libxml options, separate them with a pipe, like so:<br><br>

```
<?php
$xml = simplexml_load_string($string, 'SimpleXMLElement', LIBXML_NOCDATA | LIBXML_NOBLANKS);
?>
```
  

---

A simpler way to transform the result into an array (requires json module).<br><br>

```
<?php
function object2array($object) { return @json_decode(@json_encode($object),1); }
?>
```


Example:


```
<?php
$xml_object=simplexml_load_string('<SOME XML DATA');
$xml_array=object2array($xml_object);
?>
```
  

---

There seems to be a lot of talk about SimpleXML having a "problem" with CDATA, and writing functions to rip it out, etc. I thought so too, at first, but it&apos;s actually behaving just fine under PHP 5.2.6<br><br>The key is noted above example #6 here:<br>http://uk2.php.net/manual/en/simplexml.examples.php<br><br>"To compare an element or attribute with a string or pass it into a function that requires a string, you must cast it to a string using (string). Otherwise, PHP treats the element as an object."<br><br>If a tag contains CDATA, SimpleXML remembers that fact, by representing it separately from the string content of the element. So some functions, including print_r(), might not show what you expect. But if you explicitly cast to a string, you get the whole content.<br><br>

```
<?php
$xml = simplexml_load_string('<foo>Text1 &amp;amp; XML entities</foo>');
print_r($xml);
/*
SimpleXMLElement Object
(
    [0] => Text1 &amp; XML entities
)
*/

$xml2 = simplexml_load_string('<foo><![CDATA[Text2 &amp; raw data]]></foo>');
print_r($xml2);
/*
SimpleXMLElement Object
(
)
*/
// Where's my CDATA?

// Let's try explicit casts
print_r( (string)$xml );
print_r( (string)$xml2 );
/*
Text1 &amp; XML entities
Text2 &amp; raw data
*/
// Much better
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.simplexml-load-string.php)

**[To root](/README.md)**
# The SimpleXMLElement class



To further previous comments and drive the point home:<br><br>What makes SimpleXMLElement tricky to work with is that it feels and behaves like an object, but is actually a system RESOURCE,  (specifically a libxml resource).  <br><br>That&apos;s why you can&apos;t store a SimpleXMLElement to $_SESSION or perform straight comparison operations on node values without first casting them to some type of object.  $_SESSION expects to store &apos;an object&apos; and comparison operators expect to compare 2 &apos;objects&apos; and SimpleXMLElements are not objects.  <br><br>When you echo or print a node&apos;s value, PHP converts the value (a resource) into a string object for you.  It&apos;s a time saver for sure, but can fool you into thinking that your SimpleXMLElement is an object.  <br><br>Hope this helps clarify  

---

To access the underlying element as a string, it&apos;s necessary to make the cast $x = (string)$my_xml_element.  

---

Map xml to array (with attributes)<br><br>

```
<?php declare(strict_types=1);

/**
 * @param SimpleXMLElement $xml
 * @return array
 */
function xmlToArray(SimpleXMLElement $xml): array
{
    $parser = function (SimpleXMLElement $xml, array $collection = []) use (&amp;$parser) {
        $nodes = $xml->children();
        $attributes = $xml->attributes();

        if (0 !== count($attributes)) {
            foreach ($attributes as $attrName => $attrValue) {
                $collection['attributes'][$attrName] = strval($attrValue);
            }
        }

        if (0 === $nodes->count()) {
            $collection['value'] = strval($xml);
            return $collection;
        }

        foreach ($nodes as $nodeName => $nodeValue) {
            if (count($nodeValue->xpath('../' . $nodeName)) < 2) {
                $collection[$nodeName] = $parser($nodeValue);
                continue;
            }

            $collection[$nodeName][] = $parser($nodeValue);
        }

        return $collection;
    };

    return [
        $xml->getName() => $parser($xml)
    ];
}?>
```
  

---

Warning to anyone trying to parse XML with a key name that includes a hyphen ie.)<br>&lt;subscribe&gt;<br>    &lt;callback-url&gt;example url&lt;/callback-url&gt;<br>&lt;/subscribe&gt;<br><br>In order to access the callback-url you will need to do something like the following:<br>

```
<?php
$xml = simplexml_load_string($input);
$callback = $xml->{"callback-url"};
?>
```
<br>If you attempt to do it without the curly braces and quotes you will find out that you are returned a 0 instead of what you want.  

---

Parsing an invalid XML string through SimpleXML causes the script to crash completely (usually) therefore it is best to make sure the XML is valid before parsing with something like this:<br><br>// Must be tested with ===, as in if(isXML($xml) === true){}<br>// Returns the error message on improper XML<br>function isXML($xml){<br>    libxml_use_internal_errors(true);<br><br>    $doc = new DOMDocument(&apos;1.0&apos;, &apos;utf-8&apos;);<br>    $doc-&gt;loadXML($xml);<br><br>    $errors = libxml_get_errors();<br><br>    if(empty($errors)){<br>        return true;<br>    }<br><br>    $error = $errors[0];<br>    if($error-&gt;level &lt; 3){<br>        return true;<br>    }<br><br>    $explodedxml = explode("r", $xml);<br>    $badxml = $explodedxml[($error-&gt;line)-1];<br><br>    $message = $error-&gt;message . &apos; at line &apos; . $error-&gt;line . &apos;. Bad XML: &apos; . htmlentities($badxml);<br>    return $message;<br>}  

---

XML to JSON conversion without &apos;@attributes&apos;<br>

```
<?php
function XML2JSON($xml) {

        function normalizeSimpleXML($obj, &amp;$result) {
            $data = $obj;
            if (is_object($data)) {
                $data = get_object_vars($data);
            }
            if (is_array($data)) {
                foreach ($data as $key => $value) {
                    $res = null;
                    normalizeSimpleXML($value, $res);
                    if (($key == '@attributes') &amp;&amp; ($key)) {
                        $result = $res;
                    } else {
                        $result[$key] = $res;
                    }
                }
            } else {
                $result = $data;
            }
        }
        normalizeSimpleXML(simplexml_load_string($xml), $result);
        return json_encode($result);
    }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.simplexmlelement.php)

**[To root](/README.md)**
# SimpleXML



Three line xml2array:<br><br>

```
<?php

$xml = simplexml_load_string($xmlstring);
$json = json_encode($xml);
$array = json_decode($json,TRUE);

?>
```
<br><br>Ta da!  

#

The BIGGEST differece between an XML and a PHP array is that in an XML file, the name of elements can be the same even if they are siblings, eg. "&lt;pa&gt;&lt;ch /&gt;&lt;ch /&gt;&lt;ch /&gt;&lt;/pa&gt;", while in an PHP array, the key of which must be different.<br><br>I think the array structure developed by svdmeer can fit for XML, and fits well.<br><br>here is an example array converted from an xml file:<br>array(<br>"@tag"=&gt;"name",<br>"@attr"=&gt;array(<br>    "id"=&gt;"1","class"=&gt;"2")<br>"@text"=&gt;"some text",<br>)<br><br>or if it has childrens, that can be:<br><br>array(<br>"@tag"=&gt;"name",<br>"@attr"=&gt;array(<br>    "id"=&gt;"1","class"=&gt;"2")<br>"@items"=&gt;array(<br>    0=&gt;array(<br>        "@tag"=&gt;"name","@text"=&gt;"some text"<br>    )<br>)<br><br>Also, I wrote a function that can change that array back to XML.<br><br>

```
<?php
function array2XML($arr,$root) {
$xml = new SimpleXMLElement("&lt;?xml version=\"1.0\" encoding=\"utf-8\" ?>
```
&lt;{$root}&gt;&lt;/{$root}&gt;"); 
$f = create_function('$f,$c,$a',' 
        foreach($a as $v) {
            if(isset($v["@text"])) {
                $ch = $c->addChild($v["@tag"],$v["@text"]);
            } else {
                $ch = $c->addChild($v["@tag"]);
                if(isset($v["@items"])) {
                    $f($f,$ch,$v["@items"]);
                }
            }
            if(isset($v["@attr"])) {
                foreach($v["@attr"] as $attr => $val) {
                    $ch->addAttribute($attr,$val);
                }
            }
        }');
$f($f,$xml,$arr);
return $xml->asXML();
}
?>
```
  

#

In reply to  soloman at textgrid dot com,<br><br>2 line XML2Array:<br><br>$xml = simplexml_load_string($file);<br>$array = (array)$xml;  

#

[Official documentation page](https://www.php.net/manual/en/book.simplexml.php)

**[To root](/README.md)**
# SimpleXMLElement::asXML



To prevent asXML from encoding vowels unwantedly, simply use an approriate XML header with encoding in advance.<br><br>If you do so, asXML will happily leave your vowels (and the header) entirely untouched.<br><br>

```
<?php

$xmlstr =
'``<?xml version="1.0" encoding="UTF-8" ?>``;
<keys>
  <key lang="en">&amp;lt;Insert&amp;gt;</key>
  <key lang="de">&amp;lt;Einf&#xFC;gen&amp;gt;</key>
</keys>';

$sxe = new SimpleXMLElement($xmlstr);

$output = $sxe->asXML();

?>
```
<br><br>$xmlstr and $output are identical now.<br><br>The subsequent use of html_entity_decode() (as proposed in the very beginning in another post) has several drawbacks:<br><br>1. It is slow<br>2. It is expensive<br>3. If there are already encoded arrow brackets or double quotes in your source for instance (as shown in the above example), markup will be broken.  

---

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.asxml.php)

**[To root](/README.md)**
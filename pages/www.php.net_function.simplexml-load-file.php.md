# simplexml_load_file



Sometimes we have xml&apos;s with hyphens nodes, like<br><br>&lt;my_xml&gt;<br> &lt;some-node&gt;value&lt;/some-node&gt;<br>&lt;/my_xml&gt;<br><br>You&apos;ll need to use<br>

```
<?php
$simpleXmlObj->{'some-node'}
?>
```


instead of 


```
<?php
$simpleXmlObj->some-node;
?>
```
  

#

To correctly extract a value from a CDATA just make sure you cast the SimpleXML Element to a string value by using the cast operator:<br><br>

```
<?php
$xml = '&lt;?xml version="1.0" encoding="UTF-8" ?>
```

&lt;rss&gt;
    &lt;channel&gt;
        &lt;item&gt;
            &lt;title&gt;&lt;![CDATA[Tom &amp; Jerry]]&gt;&lt;/title&gt;
        &lt;/item&gt;
    &lt;/channel&gt;
&lt;/rss&gt;';

$xml = simplexml_load_string($xml);

// echo does the casting for you
echo $xml->channel->item->title;

// but vardump (or print_r) not!
var_dump($xml->channel->item->title);

// so cast the SimpleXML Element to 'string' solve this issue
var_dump((string) $xml->channel->item->title);
?>
```
<br><br>Above will output:<br><br>Tom &amp; Jerry<br><br>object(SimpleXMLElement)#4 (0) {}<br><br>string(11) "Tom &amp; Jerry"  

#

[Official documentation page](https://www.php.net/manual/en/function.simplexml-load-file.php)

**[To root](/README.md)**
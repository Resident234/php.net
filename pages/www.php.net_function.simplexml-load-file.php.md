# simplexml_load_file



Sometimes we have xml&apos;s with hyphens nodes, like<br><br>&lt;my_xml&gt;<br> &lt;some-node&gt;value&lt;/some-node&gt;<br>&lt;/my_xml&gt;<br><br>You&apos;ll need to use<br>

```
<?php
$simpleXmlObj-&gt;{&apos;some-node&apos;}
?>
```


instead of 


```
<?php
$simpleXmlObj-&gt;some-node;
?>
```
  

#

To correctly extract a value from a CDATA just make sure you cast the SimpleXML Element to a string value by using the cast operator:<br><br>

```
<?php
$xml = &apos;&lt;?xml version="1.0" encoding="UTF-8" ?>
```

&lt;rss&gt;
    &lt;channel&gt;
        &lt;item&gt;
            &lt;title&gt;&lt;![CDATA[Tom &amp; Jerry]]&gt;&lt;/title&gt;
        &lt;/item&gt;
    &lt;/channel&gt;
&lt;/rss&gt;&apos;;

$xml = simplexml_load_string($xml);

// echo does the casting for you
echo $xml-&gt;channel-&gt;item-&gt;title;

// but vardump (or print_r) not!
var_dump($xml-&gt;channel-&gt;item-&gt;title);

// so cast the SimpleXML Element to &apos;string&apos; solve this issue
var_dump((string) $xml-&gt;channel-&gt;item-&gt;title);
?>
```
<br><br>Above will output:<br><br>Tom &amp; Jerry<br><br>object(SimpleXMLElement)#4 (0) {}<br><br>string(11) "Tom &amp; Jerry"  

#

[Official documentation page](https://www.php.net/manual/en/function.simplexml-load-file.php)

**[To root](/README.md)**
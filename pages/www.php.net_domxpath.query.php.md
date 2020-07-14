# DOMXPath::query



If the query() function seems to ignore your $contextnode, and instead returns all the tags in the document, try to use a relative path (use a . in front of the query):<br><br>

```
<?php
    $xml = "&lt;?xml version=&apos;1.0&apos; encoding=&apos;UTF-8&apos;?>
```

        &lt;test&gt;
            &lt;tag1&gt;
                &lt;uselesstag&gt;
                    &lt;tag2&gt;test&lt;/tag2&gt;
                &lt;/uselesstag&gt;
            &lt;/tag1&gt;
            &lt;tag2&gt;test2&lt;/tag2&gt;
        &lt;/test&gt;";
   
    $dom = new DomDocument();
    $dom-&gt;loadXML($xml);
    $xpath = new DomXPath($dom);
   
    $tag1 = $dom-&gt;getElementsByTagName("tag1")-&gt;item(0);
   
    echo $xpath-&gt;query("//tag2")-&gt;length; //output 2 -&gt; correct
    echo $xpath-&gt;query("//tag2", $tag1)-&gt;length; //output 2 -&gt; wrong, the query is not relative
    echo $xpath-&gt;query(".//tag2", $tag1)-&gt;length; //output 1 -&gt; correct (note the dot in front of //)
?>
```
<br><br>See that i couldn&apos;t use $xpath-&gt;query("tag2", $tag1) as per the documentation, since "tag2" is not a direct child of "tag1".<br>I don&apos;t know why this note was deleted, i just tested it and it&apos;s correct.<br>It&apos;s not a bug, it&apos;s simply not written in the documentation.  

#

Note that if your DOMDocument was loaded from HTML, where element and attribute names are case-insensitive, the DOM parser converts them all to lower-case, so your XPath queries will have to as well; &apos;//A/@HREF&apos; won&apos;t find anything even if the original HTML contained "&lt;A HREF=&apos;example.com&apos;&gt;".  

#

Please note that what clochix says is valid for *any* document which has a default namespace (as it is the case for XHTML).<br><br>This document :<br><br>&lt;?xml version="1.0" encoding="UTF-8" ?>
```
<br><br>&lt;root xmlns="http://www.exemple.org/namespace"&gt;<br><br>    &lt;element id="1"&gt;<br>    ...<br>    &lt;/element&gt;<br><br>    &lt;element id="2"&gt;<br>    ...<br>    &lt;/element&gt;<br><br>&lt;/element&gt;<br><br>must be accessed this way :<br><br>$document = new DOMDocument();<br>$document-&gt;load(&apos;document.xml&apos;);<br><br>$xpath = new DOMXPath($document);<br>$xpath-&gt;registerNameSpace(&apos;fakeprefix&apos;, &apos;http://www.exemple.org/namespace&apos;);<br><br>$elements = $xpath-&gt;query(&apos;//fakeprefix:element&apos;);<br><br>Of course, there is no prefix in the original document, but the DOMXPath class *needs* one, whatever it is, if you use a default namespace. It *doesn&apos;t work* if you specify an empty prefix like this :<br><br>$xpath-&gt;registerNameSpace(&apos;&apos;, &apos;http://www.exemple.org/namespace&apos;);<br><br>Hope this help to spare some time...  

#

[Official documentation page](https://www.php.net/manual/en/domxpath.query.php)

**[To root](/README.md)**
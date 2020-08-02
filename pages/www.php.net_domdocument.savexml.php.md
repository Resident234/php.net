# DOMDocument::saveXML



It took some searching to figure this one out. I didn&apos;t see much in the way of explaining this glitch in the manual thus far. (For PHP5 I believe)<br><br>formatOutput = true; appears to fail when the origin of the DOM came from a file via load(). EX:<br><br>

```
<?php
    $dom = new DOMDocument();
    $dom->load ("test.xml");
    $dom->formatOutput = true;

    $new_tag = $dom->createElement ('testNode');
    $new_tag->appendChild (
        $dom->createElement ('test', 'this is a test'));
    $dom->documentElement->appendChild ($new_tag);

    printf ("<pre>%s</pre>", htmlentities ($dom->saveXML()));
?>
```


Will not indent the output and will display the modified nodes all in one long line. Makes for editing a config.xml a bit difficult when saving to a file.

By adding the preserveWhiteSpace = false; BEFORE the load() the formatOutput works as expected. EX:



```
<?php
    $dom = new DOMDocument();
    $dom->preserveWhiteSpace = false;
    $dom->load ("test.xml");
    $dom->formatOutput = true;

    $new_tag = $dom->createElement ('testNode');
    $new_tag->appendChild (
        $dom->createElement ('test', 'this is a test'));
    $dom->documentElement->appendChild ($new_tag);

    printf ("<pre>%s</pre>", htmlentities ($dom->saveXML()));
?>
```
<br><br>CAUTION: If your loaded xml file (test.xml) has an empty root node that is not shortened or has no children this will NOT work.<br><br>Example:<br><br>DOES NOT WORK:<br><?xml version="1.0"?>;<br>&lt;root&gt;<br>&lt;/root&gt;<br><br>WORKS:<br><?xml version="1.0"?>;<br>&lt;root/&gt;<br><br>WORKS:<br><?xml version="1.0"?>;<br>&lt;root&gt;<br>  &lt;!-- comment --&gt;<br>&lt;/root&gt;<br><br>WORKS:<br><?xml version="1.0"?>;<br>&lt;root&gt;<br>  &lt;child/&gt;<br>&lt;/root&gt;  

---

if you are storing multi-byte characters in XML, then saving the XML using saveXML() will create problems. It will spit out the characters converted in encoded format.<br><br>

```
<?php
$str = domdoc->saveXML(); // gives "&amp;x#1245;" some encoded data
?>
```


Instead do the following



```
<?php
$str = domdoc->saveXML(domdoc->documentElement); // gives "&#x4FDD;&#x5B58;&#x3057;&#x307E;&#x3057;&#x305F;" correct multi-byte data
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/domdocument.savexml.php)

**[To root](/README.md)**
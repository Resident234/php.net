# DOMDocument::createElement



With regard to the note below about needing htmlentities to avoid warnings about unterminated entity references, I thought it worthwhile to mention that that you don&apos;t need to with createTextNode and DOMText::__construct.  If you mix both methods of setting text nodes and do (or don&apos;t) apply htmlentities consistently to all data to be displayed, you&apos;ll get &amp;amp;s (or warnings and badly-formed xml).<br><br>It&apos;s probably in one&apos;s best interest to extend DOMElement and DOMDocument so that it creates a DOMText node and appends it, rather than passing it up to the DOMElement constructor.  Otherwise, good luck using (or not using) htmlentities in all the right places in your code, especially as code changes get made.<br><br>

```
<?php

class XDOMElement extends DOMElement {
    function __construct($name, $value = null, $namespaceURI = null) {
        parent::__construct($name, null, $namespaceURI);
    }
}

class XDOMDocument extends DOMDocument {
    function __construct($version = null, $encoding = null) {
        parent::__construct($version, $encoding);
        $this-&gt;registerNodeClass(&apos;DOMElement&apos;, &apos;XDOMElement&apos;);
    }

    function createElement($name, $value = null, $namespaceURI = null) {
        $element = new XDOMElement($name, $value, $namespaceURI);
        $element = $this-&gt;importNode($element);
        if (!empty($value)) {
            $element-&gt;appendChild(new DOMText($value));
        }
        return $element;
    }
}

$doc1 = new XDOMDocument();
$doc1_e1 = $doc1-&gt;createElement(&apos;foo&apos;, &apos;bar &amp; baz&apos;);
$doc1-&gt;appendChild($doc1_e1);
echo $doc1-&gt;saveXML();

$doc2 = new XDOMDocument();
$doc2_e1 = $doc2-&gt;createElement(&apos;foo&apos;);
$doc2-&gt;appendChild($doc2_e1);
$doc2_e1-&gt;appendChild($doc2-&gt;createTextNode(&apos;bar &amp; baz&apos;));
echo $doc2-&gt;saveXML();

?>
```


Text specified in createElement:
&lt;?xml version=""?>
```

&lt;foo&gt;bar &amp;amp; baz&lt;/foo&gt;

Text added via createTextNode:
&lt;?xml version=""?>
```
<br>&lt;foo&gt;bar &amp;amp; baz&lt;/foo&gt;  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.createelement.php)

**[To root](/README.md)**
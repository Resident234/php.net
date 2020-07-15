# SimpleXMLElement::addChild



Here is a class with more functions for SimpleXMLElement :<br><br>

```
<?php
/**
 *
 * Extension for SimpleXMLElement
 * @author Alexandre FERAUD
 *
 */
class ExSimpleXMLElement extends SimpleXMLElement
{
    /**
     * Add CDATA text in a node
     * @param string $cdata_text The CDATA value  to add
     */
  private function addCData($cdata_text)
  {
   $node= dom_import_simplexml($this);
   $no = $node->ownerDocument;
   $node->appendChild($no->createCDATASection($cdata_text));
  }

  /**
   * Create a child with CDATA value
   * @param string $name The name of the child element to add.
   * @param string $cdata_text The CDATA value of the child element.
   */
    public function addChildCData($name,$cdata_text)
    {
        $child = $this->addChild($name);
        $child->addCData($cdata_text);
    }

    /**
     * Add SimpleXMLElement code into a SimpleXMLElement
     * @param SimpleXMLElement $append
     */
    public function appendXML($append)
    {
        if ($append) {
            if (strlen(trim((string) $append))==0) {
                $xml = $this->addChild($append->getName());
                foreach($append->children() as $child) {
                    $xml->appendXML($child);
                }
            } else {
                $xml = $this->addChild($append->getName(), (string) $append);
            }
            foreach($append->attributes() as $n => $v) {
                $xml->addAttribute($n, $v);
            }
        }
    }
}
?>
```
  

#

To complete Volker Grabsch&apos;s comment, stating :<br>"Note that although addChild() escapes "&lt;" and "&gt;", it does not escape the ampersand "&amp;"."<br><br>To work around that problem, you can use direct property assignment such as :<br><br>

```
<?php
$xmlelement->value = 'my value &lt; &gt; &amp;';
// results in &lt;value&gt;my value &amp;lt; &amp;gt; &amp;amp;&lt;/value&gt;
?>
```


instead of doing :



```
<?php
$xmlelement->addChild('value', 'my value &lt; &gt; &amp;');
// results in &lt;value&gt;my value &amp;lt; &amp;gt; &amp;&lt;/value&gt; (invalid XML)
?>
```
<br><br>See also: http://stackoverflow.com/questions/552957 (Rationale behind SimpleXMLElement&apos;s handling of text values in addChild and addAttribute)<br><br>HTH  

#

In the docs for google sitemaps it is required an element for mobile sitemaps that looks like this: &lt;mobile:mobile/&gt;<br><br>I used some time to figure out how to make it, but it is quite simple when understood.<br><br>$mobile_schema = &apos;http://www.google.com/schemas/sitemap-mobile/1.0&apos;;<br><br>//Create root element<br>$xml_mobile = new SimpleXMLElement(&apos;<br>&lt;?xml version="1.0" encoding="UTF-8"?>
```
<br>&lt;urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:mobile="&apos;.$mobile_schema.&apos;"&gt;&lt;/urlset&gt;<br>&apos;);<br><br>//Add required children<br>$url_mobile = $xml_b_list_mobile-&gt;addChild(&apos;url&apos;);<br>$url_mobile-&gt;addChild(&apos;loc&apos;, &apos;your-mobile-site-url&apos;);<br>$url_mobile-&gt;addChild(&apos;mobile:mobile&apos;, null, $mobile_schema);<br><br>For this to work properly the attribute xmlns:mobile must be set in the root node, and then used as namespace(third argument) when creating the mobile:mobile child with null as value.  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.addchild.php)

**[To root](/README.md)**
# xml_parse_into_struct



Perhaps the one true parser:? I modified xademax&apos;s fine code to tidy it up, codewise and style wise, rationalize some minor crazyness, and make names fit nomenclature from the XML spec. (There are no uses of eval, and shame on you people who do.)<br><br>

```
<?php 
class XmlElement {
  var $name;
  var $attributes;
  var $content;
  var $children;
};

function xml_to_object($xml) {
  $parser = xml_parser_create();
  xml_parser_set_option($parser, XML_OPTION_CASE_FOLDING, 0);
  xml_parser_set_option($parser, XML_OPTION_SKIP_WHITE, 1);
  xml_parse_into_struct($parser, $xml, $tags);
  xml_parser_free($parser);

  $elements = array();  // the currently filling [child] XmlElement array
  $stack = array();
  foreach ($tags as $tag) {
    $index = count($elements);
    if ($tag['type'] == "complete" || $tag['type'] == "open") {
      $elements[$index] = new XmlElement;
      $elements[$index]->name = $tag['tag'];
      $elements[$index]->attributes = $tag['attributes'];
      $elements[$index]->content = $tag['value'];
      if ($tag['type'] == "open") {  // push
        $elements[$index]->children = array();
        $stack[count($stack)] = &amp;$elements;
        $elements = &amp;$elements[$index]->children;
      }
    }
    if ($tag['type'] == "close") {  // pop
      $elements = &amp;$stack[count($stack) - 1];
      unset($stack[count($stack) - 1]);
    }
  }
  return $elements[0];  // the single top-level element
}

// For example:
$xml = '
&lt;parser&gt;
   &lt;name language="en-us"&gt;Fred Parser&lt;/name&gt;
   &lt;category&gt;
       &lt;name&gt;Nomenclature&lt;/name&gt;
       &lt;note&gt;Noteworthy&lt;/note&gt;
   &lt;/category&gt;
&lt;/parser&gt;
';
print_r(xml_to_object($xml));
?>
```
<br><br>will give:<br><br>xmlelement Object<br>(<br>    [name] =&gt; parser<br>    [attributes] =&gt; <br>    [content] =&gt; <br>    [children] =&gt; Array<br>        (<br>            [0] =&gt; xmlelement Object<br>                (<br>                    [name] =&gt; name<br>                    [attributes] =&gt; Array<br>                        (<br>                            [language] =&gt; en-us<br>                        )<br><br>                    [content] =&gt; Fred Parser<br>                    [children] =&gt; <br>                )<br><br>            [1] =&gt; xmlelement Object<br>                (<br>                    [name] =&gt; category<br>                    [attributes] =&gt; <br>                    [content] =&gt; <br>                    [children] =&gt; Array<br>                        (<br>                            [0] =&gt; xmlelement Object<br>                                (<br>                                    [name] =&gt; name<br>                                    [attributes] =&gt; <br>                                    [content] =&gt; Nomenclature<br>                                    [children] =&gt; <br>                                )<br><br>                            [1] =&gt; xmlelement Object<br>                                (<br>                                    [name] =&gt; note<br>                                    [attributes] =&gt; <br>                                    [content] =&gt; Noteworthy<br>                                    [children] =&gt; <br>                                )<br><br>                        )<br><br>                )<br><br>        )<br><br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.xml-parse-into-struct.php)

**[To root](/README.md)**
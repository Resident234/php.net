# SimpleXMLElement::attributes



It is really simple to access attributes using array form. However, you must convert them to strings or ints if you plan on passing the values to functions.<br><br>

```
<?php
SimpleXMLElement Object
(
    [@attributes] =&gt; Array
        (
            [id] =&gt; 55555
        )

    [text] =&gt; "hello world"
)
?>
```


Then using a function



```
<?php
function xml_attribute($object, $attribute)
{
    if(isset($object[$attribute]))
        return (string) $object[$attribute];
}
?>
```


I can get the "id" like this



```
<?php
print xml_attribute($xml, &apos;id&apos;); //prints "55555"
?>
```
  

#

Note that you must provide the namespace if you want to access an attribute of a non-default namespace:<br><br>Consider the following example:<br><br>

```
<?php
$xml = &lt;&lt;&lt;XML
&lt;?xml version="1.0"?>
```

&lt;Workbook xmlns="urn:schemas-microsoft-com:office:spreadsheet"
 xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet"&gt;
 &lt;Table Foo="Bar" ss:ExpandedColumnCount="7"&gt;
 &lt;/Table&gt;
&lt;/Workbook&gt;
XML;

$sxml = new SimpleXMLElement($xml);

/**
 * Access attribute of default namespace
 */
var_dump((string) $sxml-&gt;Table[0][&apos;Foo&apos;]);
// outputs: &apos;Bar&apos;

/**
 * Access attribute of non-default namespace
 */
var_dump((int) $sxml-&gt;Table[0][&apos;ExpandedColumnCount&apos;]);
// outputs: 0

var_dump((int) $sxml-&gt;Table[0]-&gt;attributes(&apos;ss&apos;, TRUE)-&gt;ExpandedColumnCount);
// outputs: &apos;7&apos;
?>
```
  

#



```
<?php
$att = &apos;attribueName&apos;;

// You can access an element&apos;s attribute just like this :
$attribute = $element-&gt;attributes()-&gt;$att;

// This will save the value of the attribute, and not the objet
$attribute = (string)$element-&gt;attributes()-&gt;$att;

// You also can edit it this way :
$element-&gt;attributes()-&gt;$att = &apos;New value of the attribute&apos;;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.attributes.php)

**[To root](/README.md)**
# SimpleXMLElement::attributes



It is really simple to access attributes using array form. However, you must convert them to strings or ints if you plan on passing the values to functions.<br><br>

```
<?php
SimpleXMLElement Object
(
    [@attributes] => Array
        (
            [id] => 55555
        )

    [text] => "hello world"
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
print xml_attribute($xml, 'id'); //prints "55555"
?>
```
  

#

Note that you must provide the namespace if you want to access an attribute of a non-default namespace:<br><br>Consider the following example:<br><br>

```
<?php
$xml = <<<XML
<?xml version="1.0"?>
```

<Workbook xmlns="urn:schemas-microsoft-com:office:spreadsheet"
 xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet">
 <Table Foo="Bar" ss:ExpandedColumnCount="7">
 </Table>
</Workbook>
XML;

$sxml = new SimpleXMLElement($xml);

/**
 * Access attribute of default namespace
 */
var_dump((string) $sxml->Table[0]['Foo']);
// outputs: 'Bar'

/**
 * Access attribute of non-default namespace
 */
var_dump((int) $sxml->Table[0]['ExpandedColumnCount']);
// outputs: 0

var_dump((int) $sxml->Table[0]->attributes('ss', TRUE)->ExpandedColumnCount);
// outputs: '7'
?>
```
  

#



```
<?php
$att = 'attribueName';

// You can access an element's attribute just like this :
$attribute = $element->attributes()->$att;

// This will save the value of the attribute, and not the objet
$attribute = (string)$element->attributes()->$att;

// You also can edit it this way :
$element->attributes()->$att = 'New value of the attribute';
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.attributes.php)

**[To root](/README.md)**
# Basic SimpleXML usage



There is a common "trick" often proposed to convert a SimpleXML object to an array, by running it through json_encode() and then json_decode(). I&apos;d like to explain why this is a bad idea.<br><br>Most simply, because the whole point of SimpleXML is to be easier to use and more powerful than a plain array. For instance, you can write 

```
<?php $foo->bar->baz['bing'] ?>
```
 and it means the same thing as 

```
<?php $foo->bar[0]->baz[0]['bing'] ?>
```
, regardless of how many bar or baz elements there are in the XML; and if you write 

```
<?php (string)$foo->bar[0]->baz[0] ?>
```
 you get all the string content of that node - including CDATA sections - regardless of whether it also has child elements or attributes. You also have access to namespace information, the ability to make simple edits to the XML, and even the ability to "import" into a DOM object, for much more powerful manipulation. All of this is lost by turning the object into an array rather than reading understanding the examples on this page.<br><br>Additionally, because it is not designed for this purpose, the conversion to JSON and back will actually lose information in some situations. For instance, any elements or attributes in a namespace will simply be discarded, and any text content will be discarded if an element also has children or attributes. Sometimes, this won&apos;t matter, but if you get in the habit of converting everything to arrays, it&apos;s going to sting you eventually.<br><br>Of course, you could write a smarter conversion, which didn&apos;t have these limitations, but at that point, you are getting no value out of SimpleXML at all, and should just use the lower level XML Parser functions, or the XMLReader class, to create your structure. You still won&apos;t have the extra convenience functionality of SimpleXML, but that&apos;s your loss.  

#

For me it was easier to use arrays than objects, <br><br>So, I used this code, <br><br>$xml = simplexml_load_file(&apos;xml_file.xml&apos;);<br>    <br>$json_string = json_encode($xml);<br>    <br>$result_array = json_decode($json_string, TRUE);<br><br>Hope it would help someone  

#

If you need to output valid xml in your response, don&apos;t forget to set your header content type to xml in addition to echoing out the result of asXML():<br><br>

```
<?php

$xml=simplexml_load_file('...');
...
...xml stuff
...

//output xml in your response:
header('Content-Type: text/xml');
echo $xml->asXML();
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/simplexml.examples-basic.php)

**[To root](/README.md)**
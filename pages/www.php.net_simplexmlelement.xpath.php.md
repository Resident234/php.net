# SimpleXMLElement::xpath



To run an xpath query on an XML document that has a namespace, the namespace must be registered with SimpleXMLElement::registerXPathNamespace() before running the query. If the XML document namespace does not include a prefix, you must make up an arbitrary one, and then use it in your query.<br><br>

```
<?php
$strXml= &lt;&lt;&lt;XML
&lt;?xml version="1.0" encoding="UTF-8"?>
```

&lt;mydoc xmlns="http://www.url.com/myns"&gt;
    &lt;message&gt;Test message&lt;/message&gt;
&lt;/mydoc&gt;
XML;

$xmlDoc=new \SimpleXMLElement($strXml);

foreach($xmlDoc-&gt;getDocNamespaces() as $strPrefix =&gt; $strNamespace) {
    if(strlen($strPrefix)==0) {
        $strPrefix="a"; //Assign an arbitrary namespace prefix.
    }
    $xmlDoc-&gt;registerXPathNamespace($strPrefix,$strNamespace);
}

print($xmlDoc-&gt;xpath("//a:message")[0]); //Use the arbitrary namespace prefix in the query.
?>
```
<br><br>This will output:<br><br>Test message  

#

On a xml that have namespace you need to do this before your xpath request (or empty array will be return) :<br><br>

```
<?php
$string = str_replace(&apos;xmlns=&apos;, &apos;ns=&apos;, $string); //$string is a string that contains xml...
?>
```
  

#

xpath() can also be used to select elements by their attributes. For a good XPath reference check out: http://www.w3schools.com/xpath/xpath_syntax.asp<br><br>

```
<?php
$string = &lt;&lt;&lt;XML
&lt;sizes&gt;
    &lt;size label="Square" width="75" height="75" /&gt;
    &lt;size label="Thumbnail" width="100" height="62" /&gt;
    &lt;size label="Small" width="112" height="69" /&gt;
    &lt;size label="Large" width="112" height="69" /&gt;
&lt;/sizes&gt;
XML;

$xml = simplexml_load_string($string);
$result = $xml-&gt;xpath("//size[@label=&apos;Large&apos;]");

// print the first (and only) member of the array
echo $result[0]-&gt;asXml();
?>
```
<br><br>The script would print: <br>&lt;size label="Large" width="112" height="69"/&gt;  

#

If you want to find easly all records satisfying some condition in XML data like <br><br>....<br>   &lt;book id="bk101"&gt;<br>      &lt;author&gt;Gambardella, Matthew&lt;/author&gt;<br>      &lt;title&gt;XML Developer&apos;s Guide&lt;/title&gt;<br>      &lt;genre&gt;Computer&lt;/genre&gt;<br>      &lt;price&gt;44.95&lt;/price&gt;<br>   &lt;/book&gt;<br>   &lt;book id="bk102"&gt;<br>      &lt;author&gt;Ralls, Kim&lt;/author&gt;<br>      &lt;title&gt;Midnight Rain&lt;/title&gt;<br>      &lt;genre&gt;Fantasy&lt;/genre&gt;<br>      &lt;price&gt;5.95&lt;/price&gt;<br>   &lt;/book&gt;<br>...<br><br>try example below<br><br>

```
<?php

$xmlStr = file_get_contents(&apos;data/books.xml&apos;);
$xml = new SimpleXMLElement($xmlStr);
// seach records by tag value:
// find all book records with price higher than 40$
$res = $xml-&gt;xpath("book/price[.&gt;&apos;40&apos;]/parent::*");
print_r($res);

?>
```
<br><br>You will see response like:<br>Array (<br>[0] =&gt; SimpleXMLElement Object<br>        (<br>            [@attributes] =&gt; Array<br>                (<br>                    [id] =&gt; bk101<br>                )<br><br>            [author] =&gt; Gambardella, Matthew<br>            [title] =&gt; XML Developer&apos;s Guide<br>            [genre] =&gt; Computer<br>            [price] =&gt; 44.95<br>            [publish_date] =&gt; 2000-10-01<br>            [description] =&gt; An in-depth look at creating applications <br>      with XML.<br>        )<br>...  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.xpath.php)

**[To root](/README.md)**
# The XMLReader class



Wrapper XMLReader class, for simple SAX-reading huge xml:<br>https://github.com/dkrnl/SimpleXMLReader<br><br>Usage example: http://github.com/dkrnl/SimpleXMLReader/blob/master/examples/example1.php<br><br>

```
<?php

/**
 * Simple XML Reader
 *
 * @license Public Domain
 * @author Dmitry Pyatkov(aka dkrnl) <dkrnl@yandex.ru>
 * @url http://github.com/dkrnl/SimpleXMLReader
 */
class SimpleXMLReader extends XMLReader
{

    /**
     * Callbacks
     *
     * @var array
     */
    protected $callback = array();

    /**
     * Add node callback
     *
     * @param  string   $name
     * @param  callback $callback
     * @param  integer  $nodeType
     * @return SimpleXMLReader
     */
    public function registerCallback($name, $callback, $nodeType = XMLREADER::ELEMENT)
    {
        if (isset($this->callback[$nodeType][$name])) {
            throw new Exception("Already exists callback $name($nodeType).");
        }
        if (!is_callable($callback)) {
            throw new Exception("Already exists parser callback $name($nodeType).");
        }
        $this->callback[$nodeType][$name] = $callback;
        return $this;
    }

    /**
     * Remove node callback
     *
     * @param  string  $name
     * @param  integer $nodeType
     * @return SimpleXMLReader
     */
    public function unRegisterCallback($name, $nodeType = XMLREADER::ELEMENT)
    {
        if (!isset($this->callback[$nodeType][$name])) {
            throw new Exception("Unknow parser callback $name($nodeType).");
        }
        unset($this->callback[$nodeType][$name]);
        return $this;
    }

    /**
     * Run parser
     *
     * @return void
     */
    public function parse()
    {
        if (empty($this->callback)) {
            throw new Exception("Empty parser callback.");
        }
        $continue = true;
        while ($continue &amp;&amp; $this->read()) {
            if (isset($this->callback[$this->nodeType][$this->name])) {
                $continue = call_user_func($this->callback[$this->nodeType][$this->name], $this);
            }
        }
    }

    /**
     * Run XPath query on current node
     *
     * @param  string $path
     * @param  string $version
     * @param  string $encoding
     * @return array(SimpleXMLElement)
     */
    public function expandXpath($path, $version = "1.0", $encoding = "UTF-8")
    {
        return $this->expandSimpleXml($version, $encoding)->xpath($path);
    }

    /**
     * Expand current node to string
     *
     * @param  string $version
     * @param  string $encoding
     * @return SimpleXMLElement
     */
    public function expandString($version = "1.0", $encoding = "UTF-8")
    {
        return $this->expandSimpleXml($version, $encoding)->asXML();
    }

    /**
     * Expand current node to SimpleXMLElement
     *
     * @param  string $version
     * @param  string $encoding
     * @param  string $className
     * @return SimpleXMLElement
     */
    public function expandSimpleXml($version = "1.0", $encoding = "UTF-8", $className = null)
    {
        $element = $this->expand();
        $document = new DomDocument($version, $encoding);
        $node = $document->importNode($element, true);
        $document->appendChild($node);
        return simplexml_import_dom($node, $className);
    }

    /**
     * Expand current node to DomDocument
     *
     * @param  string $version
     * @param  string $encoding
     * @return DomDocument
     */
    public function expandDomDocument($version = "1.0", $encoding = "UTF-8")
    {
        $element = $this->expand();
        $document = new DomDocument($version, $encoding);
        $node = $document->importNode($element, true);
        $document->appendChild($node);
        return $document;
    }

}
?>
```
  

---

The "XML2Assoc" functions noted here should be used with caution... basically they are duplicating the functionality already present in SimpleXML. They may work but they won&apos;t scale.<br><br>Their are two main uses cases for parsing XML, each suited to either XMLReader or SimpleXML.<br><br>1. SimpleXML is an excellent tool for easy access to an XML document tree using native PHP data types. It starts to flounder with massive (&gt; 50M or so) XML documents, as it reads the entire document into memory before it can be processed. SimpleXML will just laugh at you then die when your server runs out of memory (or it will cause a load spike).<br><br>2. Aside from the reasoning behind massive XML documents, if you have to deal with massive XML documents, use XMLReader to process them. Don&apos;t try and gather an entire XML document into a PHP data structure using XMLReader and a PHP xml2assoc() function, you are reinventing the SimpleXML wheel.<br>When parsing massive XML documents using XMLReader, gather the data you need to perform an operation then perform it before skipping to the next node. Do not build massive data structures from a massive XML document, your server (and it&apos;s admins) will not like you.  

---

Guys, I hope this example will help<br>you can erase prints showing the process-<br>and it will be a piece of nice code.<br><br>

```
<?php 
function xml2assoc($xml, $name)
{ 
    print "<ul>";

    $tree = null;
    print("I'm inside " . $name . "<br>");
    
    while($xml->read()) 
    {
        if($xml->nodeType == XMLReader::END_ELEMENT)
        {
            print "</ul>";
            return $tree;
        }
        
        else if($xml->nodeType == XMLReader::ELEMENT)
        {
            $node = array();
            
            print("Adding " . $xml->name ."<br>");
            $node['tag'] = $xml->name;

            if($xml->hasAttributes)
            {
                $attributes = array();
                while($xml->moveToNextAttribute()) 
                {
                    print("Adding attr " . $xml->name ." = " . $xml->value . "<br>");
                    $attributes[$xml->name] = $xml->value;
                }
                $node['attr'] = $attributes;
            }
            
            if(!$xml->isEmptyElement)
            {
                $childs = xml2assoc($xml, $node['tag']);
                $node['childs'] = $childs;
            }
            
            print($node['tag'] . " added <br>");
            $tree[] = $node;
        }
        
        else if($xml->nodeType == XMLReader::TEXT)
        {
            $node = array();
            $node['text'] = $xml->value;
            $tree[] = $node;
            print "text added = " . $node['text'] . "<br>";
        }
    }
    
    print "returning " . count($tree) . " childs<br>";
    print "</ul>";
    
    return $tree; 
}

echo "<PRE>";

$xml = new XMLReader(); 
$xml->open('test.xml'); 
$assoc = xml2assoc($xml, "root"); 
$xml->close();

print_r($assoc);
echo "</PRE>";

?>
```
<br><br>It reads this xml:<br><br>&lt;test&gt;<br>    &lt;hallo volume="loud"&gt; me &lt;br/&gt; lala &lt;/hallo&gt;<br>    &lt;hallo&gt; me &lt;/hallo&gt;<br>&lt;/test&gt;  

---

[Official documentation page](https://www.php.net/manual/en/class.xmlreader.php)

**[To root](/README.md)**
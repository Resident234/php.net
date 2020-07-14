# The XMLReader class



Wrapper XMLReader class, for simple SAX-reading huge xml:<br>https://github.com/dkrnl/SimpleXMLReader<br><br>Usage example: http://github.com/dkrnl/SimpleXMLReader/blob/master/examples/example1.php<br><br>

```
<?php

/**
 * Simple XML Reader
 *
 * @license Public Domain
 * @author Dmitry Pyatkov(aka dkrnl) &lt;dkrnl@yandex.ru&gt;
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
        if (isset($this-&gt;callback[$nodeType][$name])) {
            throw new Exception("Already exists callback $name($nodeType).");
        }
        if (!is_callable($callback)) {
            throw new Exception("Already exists parser callback $name($nodeType).");
        }
        $this-&gt;callback[$nodeType][$name] = $callback;
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
        if (!isset($this-&gt;callback[$nodeType][$name])) {
            throw new Exception("Unknow parser callback $name($nodeType).");
        }
        unset($this-&gt;callback[$nodeType][$name]);
        return $this;
    }

    /**
     * Run parser
     *
     * @return void
     */
    public function parse()
    {
        if (empty($this-&gt;callback)) {
            throw new Exception("Empty parser callback.");
        }
        $continue = true;
        while ($continue &amp;&amp; $this-&gt;read()) {
            if (isset($this-&gt;callback[$this-&gt;nodeType][$this-&gt;name])) {
                $continue = call_user_func($this-&gt;callback[$this-&gt;nodeType][$this-&gt;name], $this);
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
        return $this-&gt;expandSimpleXml($version, $encoding)-&gt;xpath($path);
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
        return $this-&gt;expandSimpleXml($version, $encoding)-&gt;asXML();
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
        $element = $this-&gt;expand();
        $document = new DomDocument($version, $encoding);
        $node = $document-&gt;importNode($element, true);
        $document-&gt;appendChild($node);
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
        $element = $this-&gt;expand();
        $document = new DomDocument($version, $encoding);
        $node = $document-&gt;importNode($element, true);
        $document-&gt;appendChild($node);
        return $document;
    }

}
?>
```
  

#

The "XML2Assoc" functions noted here should be used with caution... basically they are duplicating the functionality already present in SimpleXML. They may work but they won&apos;t scale.<br><br>Their are two main uses cases for parsing XML, each suited to either XMLReader or SimpleXML.<br><br>1. SimpleXML is an excellent tool for easy access to an XML document tree using native PHP data types. It starts to flounder with massive (&gt; 50M or so) XML documents, as it reads the entire document into memory before it can be processed. SimpleXML will just laugh at you then die when your server runs out of memory (or it will cause a load spike).<br><br>2. Aside from the reasoning behind massive XML documents, if you have to deal with massive XML documents, use XMLReader to process them. Don&apos;t try and gather an entire XML document into a PHP data structure using XMLReader and a PHP xml2assoc() function, you are reinventing the SimpleXML wheel.<br>When parsing massive XML documents using XMLReader, gather the data you need to perform an operation then perform it before skipping to the next node. Do not build massive data structures from a massive XML document, your server (and it&apos;s admins) will not like you.  

#

Guys, I hope this example will help<br>you can erase prints showing the process-<br>and it will be a piece of nice code.<br><br>

```
<?php 
function xml2assoc($xml, $name)
{ 
    print "&lt;ul&gt;";

    $tree = null;
    print("I&apos;m inside " . $name . "&lt;br&gt;");
    
    while($xml-&gt;read()) 
    {
        if($xml-&gt;nodeType == XMLReader::END_ELEMENT)
        {
            print "&lt;/ul&gt;";
            return $tree;
        }
        
        else if($xml-&gt;nodeType == XMLReader::ELEMENT)
        {
            $node = array();
            
            print("Adding " . $xml-&gt;name ."&lt;br&gt;");
            $node[&apos;tag&apos;] = $xml-&gt;name;

            if($xml-&gt;hasAttributes)
            {
                $attributes = array();
                while($xml-&gt;moveToNextAttribute()) 
                {
                    print("Adding attr " . $xml-&gt;name ." = " . $xml-&gt;value . "&lt;br&gt;");
                    $attributes[$xml-&gt;name] = $xml-&gt;value;
                }
                $node[&apos;attr&apos;] = $attributes;
            }
            
            if(!$xml-&gt;isEmptyElement)
            {
                $childs = xml2assoc($xml, $node[&apos;tag&apos;]);
                $node[&apos;childs&apos;] = $childs;
            }
            
            print($node[&apos;tag&apos;] . " added &lt;br&gt;");
            $tree[] = $node;
        }
        
        else if($xml-&gt;nodeType == XMLReader::TEXT)
        {
            $node = array();
            $node[&apos;text&apos;] = $xml-&gt;value;
            $tree[] = $node;
            print "text added = " . $node[&apos;text&apos;] . "&lt;br&gt;";
        }
    }
    
    print "returning " . count($tree) . " childs&lt;br&gt;";
    print "&lt;/ul&gt;";
    
    return $tree; 
}

echo "&lt;PRE&gt;";

$xml = new XMLReader(); 
$xml-&gt;open(&apos;test.xml&apos;); 
$assoc = xml2assoc($xml, "root"); 
$xml-&gt;close();

print_r($assoc);
echo "&lt;/PRE&gt;";

?>
```
<br><br>It reads this xml:<br><br>&lt;test&gt;<br>    &lt;hallo volume="loud"&gt; me &lt;br/&gt; lala &lt;/hallo&gt;<br>    &lt;hallo&gt; me &lt;/hallo&gt;<br>&lt;/test&gt;  

#

[Official documentation page](https://www.php.net/manual/en/class.xmlreader.php)

**[To root](/README.md)**
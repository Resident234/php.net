# XML Parser Functions





```
<?php
/**
 *  XML to Associative Array Class
 * 
 *  Usage:
 *     $domObj = new xmlToArrayParser($xml);
 *     $domArr = $domObj-&gt;array;
 *     
 *     if($domObj-&gt;parse_error) echo $domObj-&gt;get_xml_error();
 *     else print_r($domArr);
 * 
 *     On Success: 
 *     eg. $domArr[&apos;top&apos;][&apos;element2&apos;][&apos;attrib&apos;][&apos;var2&apos;] =&gt; val2
 * 
 *     On Error:
 *     eg. Error Code [76] "Mismatched tag", at char 58 on line 3
 */

/**
 * Convert an xml file or string to an associative array (including the tag attributes): 
 * $domObj = new xmlToArrayParser($xml); 
 * $elemVal = $domObj-&gt;array[&apos;element&apos;]
 * Or:  $domArr=$domObj-&gt;array;  $elemVal = $domArr[&apos;element&apos;].
 * 
 * @version  2.0
 * @param Str $xml file/string.
 */
class xmlToArrayParser {
  /** The array created by the parser can be assigned to any variable: $anyVarArr = $domObj-&gt;array.*/
  public  $array = array();
  public  $parse_error = false;
  private $parser;
  private $pointer;
  
  /** Constructor: $domObj = new xmlToArrayParser($xml); */
  public function __construct($xml) {
    $this-&gt;pointer =&amp; $this-&gt;array;
    $this-&gt;parser = xml_parser_create("UTF-8");
    xml_set_object($this-&gt;parser, $this);
    xml_parser_set_option($this-&gt;parser, XML_OPTION_CASE_FOLDING, false);
    xml_set_element_handler($this-&gt;parser, "tag_open", "tag_close");
    xml_set_character_data_handler($this-&gt;parser, "cdata"); 
    $this-&gt;parse_error = xml_parse($this-&gt;parser, ltrim($xml))? false : true;
  }
  
  /** Free the parser. */
  public function __destruct() { xml_parser_free($this-&gt;parser);}

  /** Get the xml error if an an error in the xml file occured during parsing. */
  public function get_xml_error() {
    if($this-&gt;parse_error) { 
      $errCode = xml_get_error_code ($this-&gt;parser);
      $thisError =  "Error Code [". $errCode ."] \"&lt;strong style=&apos;color:red;&apos;&gt;" . xml_error_string($errCode)."&lt;/strong&gt;\", 
                            at char ".xml_get_current_column_number($this-&gt;parser) . " 
                            on line ".xml_get_current_line_number($this-&gt;parser)."";
    }else $thisError = $this-&gt;parse_error;
    return $thisError;
  }
  
  private function tag_open($parser, $tag, $attributes) {
    $this-&gt;convert_to_array($tag, &apos;attrib&apos;);
    $idx=$this-&gt;convert_to_array($tag, &apos;cdata&apos;); 
    if(isset($idx)) { 
      $this-&gt;pointer[$tag][$idx] = Array(&apos;@idx&apos; =&gt; $idx,&apos;@parent&apos; =&gt; &amp;$this-&gt;pointer);
      $this-&gt;pointer =&amp; $this-&gt;pointer[$tag][$idx];
    }else {
      $this-&gt;pointer[$tag] = Array(&apos;@parent&apos; =&gt; &amp;$this-&gt;pointer);
      $this-&gt;pointer =&amp; $this-&gt;pointer[$tag];
    }
    if (!empty($attributes)) { $this-&gt;pointer[&apos;attrib&apos;] = $attributes; }
  }

  /** Adds the current elements content to the current pointer[cdata] array. */
  private function cdata($parser, $cdata) { $this-&gt;pointer[&apos;cdata&apos;] = trim($cdata); }

  private function tag_close($parser, $tag) {
    $current = &amp; $this-&gt;pointer;
    if(isset($this-&gt;pointer[&apos;@idx&apos;])) {unset($current[&apos;@idx&apos;]);}
    
    $this-&gt;pointer = &amp; $this-&gt;pointer[&apos;@parent&apos;];
    unset($current[&apos;@parent&apos;]);
    
    if(isset($current[&apos;cdata&apos;]) &amp;&amp; count($current) == 1) { $current = $current[&apos;cdata&apos;];}
    else if(empty($current[&apos;cdata&apos;])) {unset($current[&apos;cdata&apos;]);}
  }
  
  /** Converts a single element item into array(element[0]) if a second element of the same name is encountered. */
  private function convert_to_array($tag, $item) { 
    if(isset($this-&gt;pointer[$tag][$item])) { 
      $content = $this-&gt;pointer[$tag];
      $this-&gt;pointer[$tag] = array((0) =&gt; $content);
      $idx = 1;
    }else if (isset($this-&gt;pointer[$tag])) { 
      $idx = count($this-&gt;pointer[$tag]); 
      if(!isset($this-&gt;pointer[$tag][0])) { 
        foreach ($this-&gt;pointer[$tag] as $key =&gt; $value) {
            unset($this-&gt;pointer[$tag][$key]);
            $this-&gt;pointer[$tag][0][$key] = $value;
    }}}else $idx = null;
    return $idx;
  }
}
?>
```


This is supplimental information for the "class xmlToArrayParser".
This is a fully functional error free, extensively tested php class unlike the posts that follow it.

Key phrase: Fully functional, fully tested, error free XML To Array parser.



```
<?php
/**
 * class xmlToArrayParser
 * 
  Notes: 
  1. &apos;attrib&apos; and &apos;cdata&apos; are keys added to the array when the element contains both attributes and content.
  2. Ignores content that is not in between it&apos;s own set of tags.
  3. Don&apos;t know if it recognizes processing instructions nor do I know about processing instructions.
     &lt;\?some_pi some_attr="some_value"?>
```
  This is the same as a document declaration.
  4. Empty elements are not included unless they have attributes.
  5. Version 2.0, Dec. 2, 2011, added xml error reporting.
  
  Usage:
    $domObj = new xmlToArrayParser($xml);
    $elemVal = $domObj-&gt;array[&apos;element&apos;]
    Or assign the entire array to its own variable:
    $domArr = $domObj-&gt;array;
    $elemVal = $domArr[&apos;element&apos;]
  
  Example:
    $xml = &apos;&lt;?xml version="1.0" encoding="UTF-8" standalone="no"?>
```

    &lt;top&gt;
      &lt;element1&gt;element content 1&lt;/element1&gt;
      &lt;element2 var2="val2" /&gt;
      &lt;element3 var3="val3" var4="val4"&gt;element content 3&lt;/element3&gt; 
      &lt;element3 var5="val5"&gt;element content 4&lt;/element3&gt;
      &lt;element3 var6="val6" /&gt;
      &lt;element3&gt;element content 7&lt;/element3&gt;
    &lt;/top&gt;&apos;;
    
    $domObj = new xmlToArrayParser($xml);
    $domArr = $domObj-&gt;array;
    
    if($domObj-&gt;parse_error) echo $domObj-&gt;get_xml_error();
    else print_r($domArr);

    On Success:
    $domArr[&apos;top&apos;][&apos;element1&apos;] =&gt; element content 1
    $domArr[&apos;top&apos;][&apos;element2&apos;][&apos;attrib&apos;][&apos;var2&apos;] =&gt; val2
    $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;0&apos;][&apos;attrib&apos;][&apos;var3&apos;] =&gt; val3
    $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;0&apos;][&apos;attrib&apos;][&apos;var4&apos;] =&gt; val4
    $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;0&apos;][&apos;cdata&apos;] =&gt; element content 3
    $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;1&apos;][&apos;attrib&apos;][&apos;var5&apos;] =&gt; val5
    $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;1&apos;][&apos;cdata&apos;] =&gt; element content 4
    $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;2&apos;][&apos;attrib&apos;][&apos;var6&apos;] =&gt; val6
    $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;3&apos;] =&gt; element content 7
    
    On Error:
    Error Code [76] "Mismatched tag", at char 58 on line 3
 *
 */
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/ref.xml.php)

**[To root](/README.md)**
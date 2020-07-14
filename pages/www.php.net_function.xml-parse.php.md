# xml_parse



Instead of passing a URL, we can pass the XML content to this class (either you<br> want to use CURL, Socks or fopen to retrieve it first) and instead of using <br> array, I&apos;m using separator &apos;|&apos; to identify which data to get (in order to make <br> it short to retrieve a complex XML data). Here is my class with built-in fopen <br> which you can pass URL or you can pass the content instead :<br><br>p/s : thanks to this great help page.<br><br>

```
<?php

class xx_xml {

    // XML parser variables
    var $parser;
    var $name;
    var $attr;
    var $data  = array();
    var $stack = array();
    var $keys;
    var $path;
    
    // either you pass url atau contents. 
    // Use &apos;url&apos; or &apos;contents&apos; for the parameter
    var $type;

    // function with the default parameter value
    function xx_xml($url=&apos;http://www.example.com&apos;, $type=&apos;url&apos;) {
        $this-&gt;type = $type;
        $this-&gt;url  = $url;
        $this-&gt;parse();
    }
    
    // parse XML data
    function parse()
    {
        $data = &apos;&apos;;
        $this-&gt;parser = xml_parser_create();
        xml_set_object($this-&gt;parser, $this);
        xml_set_element_handler($this-&gt;parser, &apos;startXML&apos;, &apos;endXML&apos;);
        xml_set_character_data_handler($this-&gt;parser, &apos;charXML&apos;);

        xml_parser_set_option($this-&gt;parser, XML_OPTION_CASE_FOLDING, false);

        if ($this-&gt;type == &apos;url&apos;) {
            // if use type = &apos;url&apos; now we open the XML with fopen
            
            if (!($fp = @fopen($this-&gt;url, &apos;rb&apos;))) {
                $this-&gt;error("Cannot open {$this-&gt;url}");
            }

            while (($data = fread($fp, 8192))) {
                if (!xml_parse($this-&gt;parser, $data, feof($fp))) {
                    $this-&gt;error(sprintf(&apos;XML error at line %d column %d&apos;, 
                    xml_get_current_line_number($this-&gt;parser), 
                    xml_get_current_column_number($this-&gt;parser)));
                }
            }
        } else if ($this-&gt;type == &apos;contents&apos;) {
            // Now we can pass the contents, maybe if you want 
            // to use CURL, SOCK or other method.
            $lines = explode("\n",$this-&gt;url);
            foreach ($lines as $val) {
                if (trim($val) == &apos;&apos;)
                    continue;
                $data = $val . "\n";
                if (!xml_parse($this-&gt;parser, $data)) {
                    $this-&gt;error(sprintf(&apos;XML error at line %d column %d&apos;, 
                    xml_get_current_line_number($this-&gt;parser), 
                    xml_get_current_column_number($this-&gt;parser)));
                }
            }
        }
    }

    function startXML($parser, $name, $attr)    {
        $this-&gt;stack[$name] = array();
        $keys = &apos;&apos;;
        $total = count($this-&gt;stack)-1;
        $i=0;
        foreach ($this-&gt;stack as $key =&gt; $val)    {
            if (count($this-&gt;stack) &gt; 1) {
                if ($total == $i)
                    $keys .= $key;
                else
                    $keys .= $key . &apos;|&apos;; // The saparator
            }
            else
                $keys .= $key;
            $i++;
        }
        if (array_key_exists($keys, $this-&gt;data))    {
            $this-&gt;data[$keys][] = $attr;
        }    else
            $this-&gt;data[$keys] = $attr;
        $this-&gt;keys = $keys;
    }

    function endXML($parser, $name)    {
        end($this-&gt;stack);
        if (key($this-&gt;stack) == $name)
            array_pop($this-&gt;stack);
    }

    function charXML($parser, $data)    {
        if (trim($data) != &apos;&apos;)
            $this-&gt;data[$this-&gt;keys][&apos;data&apos;][] = trim(str_replace("\n", &apos;&apos;, $data));
    }

    function error($msg)    {
        echo "&lt;div align=\"center\"&gt;
            &lt;font color=\"red\"&gt;&lt;b&gt;Error: $msg&lt;/b&gt;&lt;/font&gt;
            &lt;/div&gt;";
        exit();
    }
}

?>
```


And example of retrieving XML data:
p/s: example use to retrieve weather



```
<?php
include_once "xx_xml.class.php";

// Im using simple curl (the original is in class) to get the contents

$pageurl = "http://xml.weather.yahoo.com/forecastrss?p=MYXX0008&amp;u=c";
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt ($ch, CURLOPT_URL, $pageurl );
$thecontents = curl_exec ( $ch );
curl_close($ch);

// We want to pass only a ready XML content instead of URL
// But if you want to use URL , skip the curl functions above and use this 
// $xx4 = new xx_xml("url here",&apos;url&apos;);

$xx4 = new xx_xml($thecontents,&apos;contents&apos;); 
// As you can see, we use saparator &apos;|&apos; instead of long array
$Code = $xx4-&gt;data [&apos;rss|channel|item|yweather:condition&apos;][&apos;code&apos;] ;
$Celcius = $xx4-&gt;data [&apos;rss|channel|item|yweather:condition&apos;][&apos;temp&apos;] ;
$Text = $xx4-&gt;data [&apos;rss|channel|item|yweather:condition&apos;][&apos;text&apos;] ;
$Cityname = $xx4-&gt;data [&apos;rss|channel|yweather:location&apos;][&apos;city&apos;] ;

?>
```
<br><br>Hope this helps.  

#

[Official documentation page](https://www.php.net/manual/en/function.xml-parse.php)

**[To root](/README.md)**
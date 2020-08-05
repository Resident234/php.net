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
    // Use 'url' or 'contents' for the parameter
    var $type;

    // function with the default parameter value
    function xx_xml($url='http://www.example.com', $type='url') {
        $this->type = $type;
        $this->url  = $url;
        $this->parse();
    }
    
    // parse XML data
    function parse()
    {
        $data = '';
        $this->parser = xml_parser_create();
        xml_set_object($this->parser, $this);
        xml_set_element_handler($this->parser, 'startXML', 'endXML');
        xml_set_character_data_handler($this->parser, 'charXML');

        xml_parser_set_option($this->parser, XML_OPTION_CASE_FOLDING, false);

        if ($this->type == 'url') {
            // if use type = 'url' now we open the XML with fopen
            
            if (!($fp = @fopen($this->url, 'rb'))) {
                $this->error("Cannot open {$this->url}");
            }

            while (($data = fread($fp, 8192))) {
                if (!xml_parse($this->parser, $data, feof($fp))) {
                    $this->error(sprintf('XML error at line %d column %d', 
                    xml_get_current_line_number($this->parser), 
                    xml_get_current_column_number($this->parser)));
                }
            }
        } else if ($this->type == 'contents') {
            // Now we can pass the contents, maybe if you want 
            // to use CURL, SOCK or other method.
            $lines = explode("\n",$this->url);
            foreach ($lines as $val) {
                if (trim($val) == '')
                    continue;
                $data = $val . "\n";
                if (!xml_parse($this->parser, $data)) {
                    $this->error(sprintf('XML error at line %d column %d', 
                    xml_get_current_line_number($this->parser), 
                    xml_get_current_column_number($this->parser)));
                }
            }
        }
    }

    function startXML($parser, $name, $attr)    {
        $this->stack[$name] = array();
        $keys = '';
        $total = count($this->stack)-1;
        $i=0;
        foreach ($this->stack as $key => $val)    {
            if (count($this->stack) > 1) {
                if ($total == $i)
                    $keys .= $key;
                else
                    $keys .= $key . '|'; // The saparator
            }
            else
                $keys .= $key;
            $i++;
        }
        if (array_key_exists($keys, $this->data))    {
            $this->data[$keys][] = $attr;
        }    else
            $this->data[$keys] = $attr;
        $this->keys = $keys;
    }

    function endXML($parser, $name)    {
        end($this->stack);
        if (key($this->stack) == $name)
            array_pop($this->stack);
    }

    function charXML($parser, $data)    {
        if (trim($data) != '')
            $this->data[$this->keys]['data'][] = trim(str_replace("\n", '', $data));
    }

    function error($msg)    {
        echo "<div align=\"center\">
            <font color=\"red\"><b>Error: $msg</b></font>
            </div>";
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
// $xx4 = new xx_xml("url here",'url');

$xx4 = new xx_xml($thecontents,'contents'); 
// As you can see, we use saparator '|' instead of long array
$Code = $xx4->data ['rss|channel|item|yweather:condition']['code'] ;
$Celcius = $xx4->data ['rss|channel|item|yweather:condition']['temp'] ;
$Text = $xx4->data ['rss|channel|item|yweather:condition']['text'] ;
$Cityname = $xx4->data ['rss|channel|yweather:location']['city'] ;

?>
```
<br><br>Hope this helps.  

---

[Official documentation page](https://www.php.net/manual/en/function.xml-parse.php)

**[To root](/README.md)**
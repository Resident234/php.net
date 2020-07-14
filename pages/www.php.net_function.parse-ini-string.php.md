# parse_ini_string



parse_ini_string_m is analog for a parse_ini_string function.<br><br>had to code this function due to the lack of a php 5.3 on some hosting.<br><br>parse_ini_string_m:<br>- ignores commented lines that start with ";" or "#"<br>- ignores broken lines that do not have "="<br>- supports array values and array value keys<br><br>

```
<?php
function parse_ini_string_m($str) {
    
    if(empty($str)) return false;

    $lines = explode("\n", $str);
    $ret = Array();
    $inside_section = false;

    foreach($lines as $line) {
        
        $line = trim($line);

        if(!$line || $line[0] == "#" || $line[0] == ";") continue;
        
        if($line[0] == "[" &amp;amp;&amp;amp; $endIdx = strpos($line, "]")){
            $inside_section = substr($line, 1, $endIdx-1);
            continue;
        }

        if(!strpos($line, &apos;=&apos;)) continue;

        $tmp = explode("=", $line, 2);

        if($inside_section) {
            
            $key = rtrim($tmp[0]);
            $value = ltrim($tmp[1]);

            if(preg_match("/^\".*\"$/", $value) || preg_match("/^&apos;.*&apos;$/", $value)) {
                $value = mb_substr($value, 1, mb_strlen($value) - 2);
            }

            $t = preg_match("^\[(.*?)\]^", $key, $matches);
            if(!empty($matches) &amp;amp;&amp;amp; isset($matches[0])) {

                $arr_name = preg_replace(&apos;#\[(.*?)\]#is&apos;, &apos;&apos;, $key);

                if(!isset($ret[$inside_section][$arr_name]) || !is_array($ret[$inside_section][$arr_name])) {
                    $ret[$inside_section][$arr_name] = array();
                }

                if(isset($matches[1]) &amp;amp;&amp;amp; !empty($matches[1])) {
                    $ret[$inside_section][$arr_name][$matches[1]] = $value;
                } else {
                    $ret[$inside_section][$arr_name][] = $value;
                }

            } else {
                $ret[$inside_section][trim($tmp[0])] = $value;
            }            

        } else {
            
            $ret[trim($tmp[0])] = ltrim($tmp[1]);

        }
    }
    return $ret;
}
?>
```


example usage:



```
<?php
$ini = &apos;

    [simple]
    val_one = "some value"
    val_two = 567

    [array]
    val_arr[] = "arr_elem_one"
    val_arr[] = "arr_elem_two"
    val_arr[] = "arr_elem_three"

    [array_keys]
    val_arr_two[6] = "key_6"
    val_arr_two[some_key] = "some_key_value"

&apos;;

$arr = parse_ini_string_m($ini);
?>
```
<br><br>variable $arr output:<br><br>Array<br>(<br>    [simple] =&gt; Array<br>    (<br>        [val_one] =&gt; some value<br>        [val_two] =&gt; 567<br>    )<br><br>    [array] =&gt; Array<br>    (<br>        [val_arr] =&gt; Array<br>        (<br>            [0] =&gt; arr_elem_one<br>            [1] =&gt; arr_elem_two<br>            [2] =&gt; arr_elem_three<br>        )<br>    )<br><br>    [array_keys] =&gt; Array<br>    (<br>        [val_arr_two] =&gt; Array<br>        (<br>            [6] =&gt; key_6<br>            [some_key] =&gt; some_key_value<br>        )<br><br>    )<br><br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-ini-string.php)

**[To root](/README.md)**
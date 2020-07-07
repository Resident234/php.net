# parse_ini_string





parse_ini_string_m is analog for a parse_ini_string function.

had to code this function due to the lack of a php 5.3 on some hosting.

parse_ini_string_m:
- ignores commented lines that start with &quot;;&quot; or &quot;#&quot;
- ignores broken lines that do not have &quot;=&quot;
- supports array values and array value keys



```
<?php
function parse_ini_string_m($str) {
&#xA0; &#xA0; 
&#xA0; &#xA0; if(empty($str)) return false;

&#xA0; &#xA0; $lines = explode(&quot;\n&quot;, $str);
&#xA0; &#xA0; $ret = Array();
&#xA0; &#xA0; $inside_section = false;

&#xA0; &#xA0; foreach($lines as $line) {
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $line = trim($line);

&#xA0; &#xA0; &#xA0; &#xA0; if(!$line || $line[0] == &quot;#&quot; || $line[0] == &quot;;&quot;) continue;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if($line[0] == &quot;[&quot; &amp;amp;&amp;amp; $endIdx = strpos($line, &quot;]&quot;)){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $inside_section = substr($line, 1, $endIdx-1);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; if(!strpos($line, &apos;=&apos;)) continue;

&#xA0; &#xA0; &#xA0; &#xA0; $tmp = explode(&quot;=&quot;, $line, 2);

&#xA0; &#xA0; &#xA0; &#xA0; if($inside_section) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $key = rtrim($tmp[0]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $value = ltrim($tmp[1]);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(preg_match(&quot;/^\&quot;.*\&quot;$/&quot;, $value) || preg_match(&quot;/^&apos;.*&apos;$/&quot;, $value)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $value = mb_substr($value, 1, mb_strlen($value) - 2);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $t = preg_match(&quot;^\[(.*?)\]^&quot;, $key, $matches);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!empty($matches) &amp;amp;&amp;amp; isset($matches[0])) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $arr_name = preg_replace(&apos;#\[(.*?)\]#is&apos;, &apos;&apos;, $key);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!isset($ret[$inside_section][$arr_name]) || !is_array($ret[$inside_section][$arr_name])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ret[$inside_section][$arr_name] = array();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(isset($matches[1]) &amp;amp;&amp;amp; !empty($matches[1])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ret[$inside_section][$arr_name][$matches[1]] = $value;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ret[$inside_section][$arr_name][] = $value;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ret[$inside_section][trim($tmp[0])] = $value;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ret[trim($tmp[0])] = ltrim($tmp[1]);

&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; return $ret;
}
?>
```


example usage:



```
<?php
$ini = &apos;

&#xA0; &#xA0; [simple]
&#xA0; &#xA0; val_one = &quot;some value&quot;
&#xA0; &#xA0; val_two = 567

&#xA0; &#xA0; [array]
&#xA0; &#xA0; val_arr[] = &quot;arr_elem_one&quot;
&#xA0; &#xA0; val_arr[] = &quot;arr_elem_two&quot;
&#xA0; &#xA0; val_arr[] = &quot;arr_elem_three&quot;

&#xA0; &#xA0; [array_keys]
&#xA0; &#xA0; val_arr_two[6] = &quot;key_6&quot;
&#xA0; &#xA0; val_arr_two[some_key] = &quot;some_key_value&quot;

&apos;;

$arr = parse_ini_string_m($ini);
?>
```


variable $arr output:

Array
(
&#xA0; &#xA0; [simple] =&gt; Array
&#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; [val_one] =&gt; some value
&#xA0; &#xA0; &#xA0; &#xA0; [val_two] =&gt; 567
&#xA0; &#xA0; )

&#xA0; &#xA0; [array] =&gt; Array
&#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; [val_arr] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; arr_elem_one
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; arr_elem_two
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [2] =&gt; arr_elem_three
&#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; )

&#xA0; &#xA0; [array_keys] =&gt; Array
&#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; [val_arr_two] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [6] =&gt; key_6
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [some_key] =&gt; some_key_value
&#xA0; &#xA0; &#xA0; &#xA0; )

&#xA0; &#xA0; )

)

  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-ini-string.php)

**[To root](/README.md)**
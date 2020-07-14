# json_encode



This isn&apos;t mentioned in the documentation for either PHP or jQuery, but if you&apos;re passing JSON data to a javascript program, make sure your program begins with:<br><br>

```
<?php
header(&apos;Content-Type: application/json&apos;);
?>
```
  

#

Are you sure you want to use JSON_NUMERIC_CHECK, really really sure?<br><br>Just watch this usecase:<br><br>

```
<?php
// International phone number
json_encode(array(&apos;phone_number&apos; =&gt; &apos;+33123456789&apos;), JSON_NUMERIC_CHECK);
?>
```
<br><br>And then you get this JSON:<br><br>{"phone_number":33123456789}<br><br>Maybe it makes sense for PHP (as is_numeric(&apos;+33123456789&apos;) returns true), but really, casting it as an int?!<br><br>So be careful when using JSON_NUMERIC_CHECK, it may mess up with your data!  

#

A note of caution: If you are wondering why json_encode() encodes your PHP array as a JSON object instead of a JSON array, you might want to double check your array keys because json_encode() assumes that you array is an object if your keys are not sequential.<br><br>e.g.:<br><br>

```
<?php
$myarray = Array(&apos;isa&apos;, &apos;dalawa&apos;, &apos;tatlo&apos;);
var_dump($myarray);
/* output
array(3) {
  [0]=&gt;
  string(3) "isa"
  [1]=&gt;
  string(6) "dalawa"
  [2]=&gt;
  string(5) "tatlo"
}
*/
?>
```


As you can see, the keys are sequential; $myarray will be correctly encoded as a JSON array.



```
<?php
$myarray = Array(&apos;isa&apos;, &apos;dalawa&apos;, &apos;tatlo&apos;);

unset($myarray[1]);
var_dump($myarray);
/* output
array(2) {
  [0]=&gt;
  string(3) "isa"
  [2]=&gt;
  string(5) "tatlo"
}
*/
?>
```
<br><br>Unsetting an element will also remove the keys. json_encode() will now assume that this is an object, and will encode it as such.<br><br>SOLUTION: Use array_values() to re-index the array.  

#

This is intended to be a simple readable json encode function for PHP 5.3+ (and licensed under GNU/AGPLv3 or GPLv3 like you prefer):<br><br>

```
<?php

function json_readable_encode($in, $indent = 0, $from_array = false)
{
    $_myself = __FUNCTION__;
    $_escape = function ($str)
    {
        return preg_replace("!([\b\t\n\r\f\"\\&apos;])!", "\\\\\\1", $str);
    };

    $out = &apos;&apos;;

    foreach ($in as $key=&gt;$value)
    {
        $out .= str_repeat("\t", $indent + 1);
        $out .= "\"".$_escape((string)$key)."\": ";

        if (is_object($value) || is_array($value))
        {
            $out .= "\n";
            $out .= $_myself($value, $indent + 1);
        }
        elseif (is_bool($value))
        {
            $out .= $value ? &apos;true&apos; : &apos;false&apos;;
        }
        elseif (is_null($value))
        {
            $out .= &apos;null&apos;;
        }
        elseif (is_string($value))
        {
            $out .= "\"" . $_escape($value) ."\"";
        }
        else
        {
            $out .= $value;
        }

        $out .= ",\n";
    }

    if (!empty($out))
    {
        $out = substr($out, 0, -2);
    }

    $out = str_repeat("\t", $indent) . "{\n" . $out;
    $out .= "\n" . str_repeat("\t", $indent) . "}";

    return $out;
}

?>
```
  

#

For PHP5.3 users who want to emulate JSON_UNESCAPED_UNICODE, there is simple way to do it:<br>

```
<?php
function my_json_encode($arr)
{
        //convmap since 0x80 char codes so it takes all multibyte codes (above ASCII 127). So such characters are being "hidden" from normal json_encoding
        array_walk_recursive($arr, function (&amp;$item, $key) { if (is_string($item)) $item = mb_encode_numericentity($item, array (0x80, 0xffff, 0, 0xffff), &apos;UTF-8&apos;); });
        return mb_decode_numericentity(json_encode($arr), array (0x80, 0xffff, 0, 0xffff), &apos;UTF-8&apos;);

}
?>
```
  

#

Solution for UTF-8 Special Chars.<br><br>&lt;?<br><br>$array = array(&apos;nome&apos;=&gt;&apos;Pai&#xE7;&#xE3;o&apos;,&apos;cidade&apos;=&gt;&apos;S&#xE3;o Paulo&apos;);<br><br>$array = array_map(&apos;htmlentities&apos;,$array);<br><br>//encode<br>$json = html_entity_decode(json_encode($array));<br><br>//Output: {"nome":"Pai&#xE7;&#xE3;o","cidade":"S&#xE3;o Paulo"}<br>echo $json;<br><br>?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.json-encode.php)

**[To root](/README.md)**
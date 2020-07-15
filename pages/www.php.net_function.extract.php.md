# extract



[New Version]<br><br>Example Usage:<br>

```
<?php
$_GET['A']['a'] = '  CORRECT(including some spaces)    ';
$_GET['A']['b'] = '  CORRECT(including some spaces)    ';
$_GET['A']['c'] = "Invalid UTF-8 sequence: \xe3\xe3\xe3";
$_GET['A']['d']['invalid_structure'] = 'INVALID';

$_GET['B']['a'] = '  CORRECT(including some spaces)    ';
$_GET['B']['b'] = "Invalid UTF-8 sequence: \xe3\xe3\xe3";
$_GET['B']['c']['invalid_structure'] = 'INVALID';
$_GET['B']["Invalid UTF-8 sequence: \xe3\xe3\xe3"] = 'INVALID';

$_GET['C']['a'] = '  CORRECT(including some spaces)    ';
$_GET['C']['b'] = "Invalid UTF-8 sequence: \xe3\xe3\xe3";
$_GET['C']['c']['invalid_structure'] = 'INVALID';
$_GET['C']["Invalid UTF-8 sequence: \xe3\xe3\xe3"] = 'INVALID';

$_GET['unneeded_item'] = 'UNNEEDED';

var_dump(filter_struct_utf8(INPUT_GET, array(
    'A' => array(
        'a' => '',
        'b' => FILTER_STRUCT_TRIM,
        'c' => '',
        'd' => '',
    ),
    'B' => FILTER_STRUCT_FORCE_ARRAY,
    'C' => FILTER_STRUCT_FORCE_ARRAY | FILTER_STRUCT_TRIM,
)));
?>
```
<br><br>Example Result:<br>array(3) {<br>  ["A"]=&gt;<br>  array(4) {<br>    ["a"]=&gt;<br>    string(36) "  CORRECT(including some spaces)    "<br>    ["b"]=&gt;<br>    string(30) "CORRECT(including some spaces)"<br>    ["c"]=&gt;<br>    string(0) ""<br>    ["d"]=&gt;<br>    string(0) ""<br>  }<br>  ["B"]=&gt;<br>  array(3) {<br>    ["a"]=&gt;<br>    string(36) "  CORRECT(including some spaces)    "<br>    ["b"]=&gt;<br>    string(0) ""<br>    ["c"]=&gt;<br>    string(0) ""<br>  }<br>  ["C"]=&gt;<br>  array(3) {<br>    ["a"]=&gt;<br>    string(30) "CORRECT(including some spaces)"<br>    ["b"]=&gt;<br>    string(0) ""<br>    ["c"]=&gt;<br>    string(0) ""<br>  }<br>}  

#

You can&apos;t extract a numeric indexed array(e.g. non-assoc array).<br>

```
<?php
$a = array(
  1,
  2
);
extract($a);
var_dump(${1});
?>
```
<br><br>result:<br>PHP Notice:  Undefined variable: 1 in /Users/Lutashi/t.php on line 7<br><br>Notice: Undefined variable: 1 in /Users/Lutashi/t.php on line 7<br>NULL  

#

I have made some tests to compare the speed of next constructions:<br>

```
<?php

extract($ARRAY);

// vs. 

foreach($ARRAY as $key=>$value) 
    $key = $value; 
?>
```
<br><br>Surprisingly for me extract is 20%-80% slower then foreach construction. I don&apos;t really understand why, but it&apos;s so.  

#

[Official documentation page](https://www.php.net/manual/en/function.extract.php)

**[To root](/README.md)**
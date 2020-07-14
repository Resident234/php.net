# extract



[New Version]<br><br>Example Usage:<br>

```
<?php
$_GET[&apos;A&apos;][&apos;a&apos;] = &apos;  CORRECT(including some spaces)    &apos;;
$_GET[&apos;A&apos;][&apos;b&apos;] = &apos;  CORRECT(including some spaces)    &apos;;
$_GET[&apos;A&apos;][&apos;c&apos;] = "Invalid UTF-8 sequence: \xe3\xe3\xe3";
$_GET[&apos;A&apos;][&apos;d&apos;][&apos;invalid_structure&apos;] = &apos;INVALID&apos;;

$_GET[&apos;B&apos;][&apos;a&apos;] = &apos;  CORRECT(including some spaces)    &apos;;
$_GET[&apos;B&apos;][&apos;b&apos;] = "Invalid UTF-8 sequence: \xe3\xe3\xe3";
$_GET[&apos;B&apos;][&apos;c&apos;][&apos;invalid_structure&apos;] = &apos;INVALID&apos;;
$_GET[&apos;B&apos;]["Invalid UTF-8 sequence: \xe3\xe3\xe3"] = &apos;INVALID&apos;;

$_GET[&apos;C&apos;][&apos;a&apos;] = &apos;  CORRECT(including some spaces)    &apos;;
$_GET[&apos;C&apos;][&apos;b&apos;] = "Invalid UTF-8 sequence: \xe3\xe3\xe3";
$_GET[&apos;C&apos;][&apos;c&apos;][&apos;invalid_structure&apos;] = &apos;INVALID&apos;;
$_GET[&apos;C&apos;]["Invalid UTF-8 sequence: \xe3\xe3\xe3"] = &apos;INVALID&apos;;

$_GET[&apos;unneeded_item&apos;] = &apos;UNNEEDED&apos;;

var_dump(filter_struct_utf8(INPUT_GET, array(
    &apos;A&apos; =&gt; array(
        &apos;a&apos; =&gt; &apos;&apos;,
        &apos;b&apos; =&gt; FILTER_STRUCT_TRIM,
        &apos;c&apos; =&gt; &apos;&apos;,
        &apos;d&apos; =&gt; &apos;&apos;,
    ),
    &apos;B&apos; =&gt; FILTER_STRUCT_FORCE_ARRAY,
    &apos;C&apos; =&gt; FILTER_STRUCT_FORCE_ARRAY | FILTER_STRUCT_TRIM,
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

foreach($ARRAY as $key=&gt;$value) 
    $key = $value; 
?>
```
<br><br>Surprisingly for me extract is 20%-80% slower then foreach construction. I don&apos;t really understand why, but it&apos;s so.  

#

[Official documentation page](https://www.php.net/manual/en/function.extract.php)

**[To root](/README.md)**
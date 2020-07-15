# array_column



if array_column does not exist the below solution will work.<br><br>if(!function_exists("array_column"))<br>{<br><br>    function array_column($array,$column_name)<br>    {<br><br>        return array_map(function($element) use($column_name){return $element[$column_name];}, $array);<br><br>    }<br><br>}  

#

You can also use array_map fucntion if you haven&apos;t array_column().<br><br>example:<br><br>$a = array(<br>    array(<br>        &apos;id&apos; =&gt; 2135,<br>        &apos;first_name&apos; =&gt; &apos;John&apos;,<br>        &apos;last_name&apos; =&gt; &apos;Doe&apos;,<br>    ),<br>    array(<br>        &apos;id&apos; =&gt; 3245,<br>        &apos;first_name&apos; =&gt; &apos;Sally&apos;,<br>        &apos;last_name&apos; =&gt; &apos;Smith&apos;,<br>    )<br>);<br><br>array_column($a, &apos;last_name&apos;);<br><br>becomes<br><br>array_map(function($element){return $element[&apos;last_name&apos;];}, $a);  

#

This function does not preserve the original keys of the array (when not providing an index_key).<br><br>You can work around that like so:<br><br>

```
<?php
// instead of
array_column($array, 'column');

// to preserve keys
array_combine(array_keys($array), array_column($array, 'column'));
?>
```
  

#

Some remarks not included in the official documentation.<br><br>1) array_column does not support 1D arrays, in which case an empty array is returned.<br><br>2) The $column_key is zero-based.<br><br>3) If $column_key extends the valid index range an empty array is returned.  

#

[Official documentation page](https://www.php.net/manual/en/function.array-column.php)

**[To root](/README.md)**
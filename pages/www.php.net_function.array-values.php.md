# array_values



Remember, array_values() will ignore your beautiful numeric indexes, it will renumber them according tho the &apos;foreach&apos; ordering:<br><br>

```
<?php
$a = array(
 3 =&gt; 11,
 1 =&gt; 22,
 2 =&gt; 33,
);
$a[0] = 44;

print_r( array_values( $a ));
==&gt;
Array(
  [0] =&gt; 11
  [1] =&gt; 22
  [2] =&gt; 33
  [3] =&gt; 44
)
?>
```
  

#

Just a warning that re-indexing an array by array_values() may cause you to reach the memory limit unexpectly.<br><br>For example, if your PHP momory_limits is 8MB,<br> and says there&apos;s a BIG array $bigArray which allocate 5MB of memory.<br><br>Doing this will cause PHP exceeds the momory limits:<br><br>

```
<?php
  $bigArray = array_values( $bigArray );
?>
```
<br><br>It&apos;s because array_values() does not re-index $bigArray directly,<br>it just re-index it into another array, and assign to itself later.  

#

This is another way to get value from a multidimensional array, but for versions of php &gt;= 5.3.x<br>

```
<?php
/**
 * Get all values from specific key in a multidimensional array
 *
 * @param $key string
 * @param $arr array
 * @return null|string|array
 */
function array_value_recursive($key, array $arr){
    $val = array();
    array_walk_recursive($arr, function($v, $k) use($key, &amp;$val){
        if($k == $key) array_push($val, $v);
    });
    return count($val) &gt; 1 ? $val : array_pop($val);
}

$arr = array(
    &apos;foo&apos; =&gt; &apos;foo&apos;,
    &apos;bar&apos; =&gt; array(
        &apos;baz&apos; =&gt; &apos;baz&apos;,
        &apos;candy&apos; =&gt; &apos;candy&apos;,
        &apos;vegetable&apos; =&gt; array(
            &apos;carrot&apos; =&gt; &apos;carrot&apos;,
        )
    ),
    &apos;vegetable&apos; =&gt; array(
        &apos;carrot&apos; =&gt; &apos;carrot2&apos;,
    ),
    &apos;fruits&apos; =&gt; &apos;fruits&apos;,
);

var_dump(array_value_recursive(&apos;carrot&apos;, $arr)); // array(2) { [0]=&gt; string(6) "carrot" [1]=&gt; string(7) "carrot2" }
var_dump(array_value_recursive(&apos;apple&apos;, $arr)); // null
var_dump(array_value_recursive(&apos;baz&apos;, $arr)); // string(3) "baz"
var_dump(array_value_recursive(&apos;candy&apos;, $arr)); // string(5) "candy"
var_dump(array_value_recursive(&apos;pear&apos;, $arr)); // null
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-values.php)

**[To root](/README.md)**
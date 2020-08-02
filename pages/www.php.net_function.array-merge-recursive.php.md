# array_merge_recursive



I refactored the Daniel&apos;s function and I got it:<br><br>

```
<?php
/**
 * array_merge_recursive does indeed merge arrays, but it converts values with duplicate
 * keys to arrays rather than overwriting the value in the first array with the duplicate
 * value in the second array, as array_merge does. I.e., with array_merge_recursive,
 * this happens (documented behavior):
 *
 * array_merge_recursive(array('key' => 'org value'), array('key' => 'new value'));
 *     => array('key' => array('org value', 'new value'));
 *
 * array_merge_recursive_distinct does not change the datatypes of the values in the arrays.
 * Matching keys' values in the second array overwrite those in the first array, as is the
 * case with array_merge, i.e.:
 *
 * array_merge_recursive_distinct(array('key' => 'org value'), array('key' => 'new value'));
 *     => array('key' => array('new value'));
 *
 * Parameters are passed by reference, though only for performance reasons. They're not
 * altered by this function.
 *
 * @param array $array1
 * @param array $array2
 * @return array
 * @author Daniel <daniel (at) danielsmedegaardbuus (dot) dk>
 * @author Gabriel Sobrinho <gabriel (dot) sobrinho (at) gmail (dot) com>
 */
function array_merge_recursive_distinct ( array &amp;$array1, array &amp;$array2 )
{
  $merged = $array1;

  foreach ( $array2 as $key => &amp;$value )
  {
    if ( is_array ( $value ) &amp;&amp; isset ( $merged [$key] ) &amp;&amp; is_array ( $merged [$key] ) )
    {
      $merged [$key] = array_merge_recursive_distinct ( $merged [$key], $value );
    }
    else
    {
      $merged [$key] = $value;
    }
  }

  return $merged;
}
?>
```
<br><br>This fix the E_NOTICE when the the first array doesn&apos;t have the key and the second array have a value which is a array.  

---

I little bit improved daniel&apos;s and gabriel&apos;s contribution to behave more like original array_merge function to append numeric keys instead of overwriting them and added usefull option of specifying which elements to merge as you more often than not need to merge only specific part of array tree, and some parts of array just need  to let overwrite previous. By specifying helper element mergeWithParent=true, that section of array  will be merged, otherwise latter array part will override former. First level of array behave as classic array_merge.<br><br>        function array_merge_recursive_distinct ( array &amp;$array1, array &amp;$array2 )<br>    {<br>        static $level=0;<br>        $merged = [];<br>        if (!empty($array2["mergeWithParent"]) || $level == 0) {<br>            $merged = $array1;<br>        }<br><br>        foreach ( $array2 as $key =&gt; &amp;$value )<br>        {<br>            if (is_numeric($key)) {<br>                $merged [] = $value;<br>            } else {<br>                $merged[$key] = $value;<br>            }<br><br>            if ( is_array ( $value ) &amp;&amp; isset ( $array1 [$key] ) &amp;&amp; is_array ( $array1 [$key] )<br>            ) {<br>                $level++;<br>                $merged [$key] = array_merge_recursive_distinct($array1 [$key], $value);<br>                $level--;<br>            }<br>        }<br>        unset($merged["mergeWithParent"]);<br>        return $merged;<br>    }  

---

This is my version of array_merge_recursive without overwriting numeric keys:<br>

```
<?php
function array_merge_recursive_new() {

        $arrays = func_get_args();
        $base = array_shift($arrays);

        foreach ($arrays as $array) {
            reset($base); //important
            while (list($key, $value) = @each($array)) {
                if (is_array($value) &amp;&amp; @is_array($base[$key])) {
                    $base[$key] = array_merge_recursive_new($base[$key], $value);
                } else {
                    $base[$key] = $value;
                }
            }
        }

        return $base;
    }
?>
```
  

---

There are a lot of examples here for recursion that are meant to behave more like array_merge() but they don&apos;t get it quite right or are fairly customised. I think this version is most similar, takes more than 2 arguments and can be renamed in one place:<br><br>

```
<?php

function array_merge_recursive_simple() {

    if (func_num_args() < 2) {
        trigger_error(__FUNCTION__ .' needs two or more array arguments', E_USER_WARNING);
        return;
    }
    $arrays = func_get_args();
    $merged = array();
    while ($arrays) {
        $array = array_shift($arrays);
        if (!is_array($array)) {
            trigger_error(__FUNCTION__ .' encountered a non array argument', E_USER_WARNING);
            return;
        }
        if (!$array)
            continue;
        foreach ($array as $key => $value)
            if (is_string($key))
                if (is_array($value) &amp;&amp; array_key_exists($key, $merged) &amp;&amp; is_array($merged[$key]))
                    $merged[$key] = call_user_func(__FUNCTION__, $merged[$key], $value);
                else
                    $merged[$key] = $value;
            else
                $merged[] = $value;
    }
    return $merged;
}

$a1 = array(
    88 => 1,
    'foo' => 2,
    'bar' => array(4),
    'x' => 5,
    'z' => array(
        6,
        'm' => 'hi',
    ),
);
$a2 = array(
    99 => 7,
    'foo' => array(8),
    'bar' => 9,
    'y' => 10,
    'z' => array(
        'm' => 'bye',
        11,
    ),
);
$a3 = array(
    'z' => array(
        'm' => 'ciao',
    ),
);
var_dump(array_merge($a1, $a2, $a3));
var_dump(array_merge_recursive_simple($a1, $a2, $a3));
var_dump(array_merge_recursive($a1, $a2, $a3));
?>
```
<br><br>gives:<br><br>array(7) {              array(7) {              array(7) {<br>  int(1)                  int(1)                  int(1)<br>  ["foo"]=&gt;               ["foo"]=&gt;               ["foo"]=&gt;<br>  array(1) {              array(1) {              array(2) {<br>    [0]=&gt;                   [0]=&gt;                   [0]=&gt;<br>    int(8)                  int(8)                  int(2)<br>  }                       }                         [1]=&gt;<br>  ["bar"]=&gt;               ["bar"]=&gt;                 int(8)<br>  int(9)                  int(9)                  }<br>  ["x"]=&gt;                 ["x"]=&gt;                 ["bar"]=&gt;<br>  int(5)                  int(5)                  array(2) {<br>  ["z"]=&gt;                 ["z"]=&gt;                   [0]=&gt;<br>  array(1) {              array(3) {                int(4)<br>    ["m"]=&gt;                 [0]=&gt;                   [1]=&gt;<br>    string(4) "ciao"        int(6)                  int(9)<br>  }                         ["m"]=&gt;               }<br>  [1]=&gt;                     string(4) "ciao"      ["x"]=&gt;<br>  int(7)                    [1]=&gt;                 int(5)<br>  ["y"]=&gt;                   int(11)               ["z"]=&gt;<br>  int(10)                 }                       array(3) {<br>}                         [1]=&gt;                     [0]=&gt;<br>                          int(7)                    int(6)<br>                          ["y"]=&gt;                   ["m"]=&gt;<br>                          int(10)                   array(3) {<br>                        }                             [0]=&gt;<br>                                                      string(2) "hi"<br>                                                      [1]=&gt;<br>                                                      string(3) "bye"<br>                                                      [2]=&gt;<br>                                                      string(4) "ciao"<br>                                                    }<br>                                                    [1]=&gt;<br>                                                    int(11)<br>                                                  }<br>                                                  [1]=&gt;<br>                                                  int(7)<br>                                                  ["y"]=&gt;<br>                                                  int(10)<br>                                                }  

---

Here&apos;s my function to recursively merge two arrays with overwrites. Nice for merging configurations.<br><br>

```
<?php

function MergeArrays($Arr1, $Arr2)
{
  foreach($Arr2 as $key => $Value)
  {
    if(array_key_exists($key, $Arr1) &amp;&amp; is_array($Value))
      $Arr1[$key] = MergeArrays($Arr1[$key], $Arr2[$key]);

    else
      $Arr1[$key] = $Value;

  }

  return $Arr1;

}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.array-merge-recursive.php)

**[To root](/README.md)**
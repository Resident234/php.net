# array_multisort



I came up with an easy way to sort database-style results. This does what example 3 does, except it takes care of creating those intermediate arrays for you before passing control on to array_multisort(). <br><br>

```
<?php
function array_orderby()
{
    $args = func_get_args();
    $data = array_shift($args);
    foreach ($args as $n => $field) {
        if (is_string($field)) {
            $tmp = array();
            foreach ($data as $key => $row)
                $tmp[$key] = $row[$field];
            $args[$n] = $tmp;
            }
    }
    $args[] = &amp;$data;
    call_user_func_array('array_multisort', $args);
    return array_pop($args);
}
?>
```


The sorted array is now in the return value of the function instead of being passed by reference.



```
<?php
$data[] = array('volume' => 67, 'edition' => 2);
$data[] = array('volume' => 86, 'edition' => 1);
$data[] = array('volume' => 85, 'edition' => 6);
$data[] = array('volume' => 98, 'edition' => 2);
$data[] = array('volume' => 86, 'edition' => 6);
$data[] = array('volume' => 67, 'edition' => 7);

// Pass the array, followed by the column names and sort flags
$sorted = array_orderby($data, 'volume', SORT_DESC, 'edition', SORT_ASC);
?>
```
  

#

One-liner function to sort multidimensionnal array by key, thank&apos;s to array_column<br><br>

```
<?php

array_multisort (array_column($array, 'key'), SORT_DESC, $array);

?>
```
  

#

Hi,<br><br>I would like to see the next code snippet to be added to http://nl3.php.net/array_multisort<br><br>Purpose: Sort a 2-dimensional array on some key(s)<br><br>Advantage of function: <br>- uses PHP&apos;s array_multisort function for sorting;<br>- it prepares the arrays (needed by array_multisort) for you;<br>- allows the sort criteria be passed as a separate array (It is possible to use sort order and flags.); <br>- easy to set/overwrite the way strings are sorted (case insensitive instead of case sensitive, which is PHP&apos;s default way of sorting);<br>- performs excellent <br><br>function MultiSort($data, $sortCriteria, $caseInSensitive = true)<br>{<br>  if( !is_array($data) || !is_array($sortCriteria))<br>    return false;       <br>  $args = array(); <br>  $i = 0;<br>  foreach($sortCriteria as $sortColumn =&gt; $sortAttributes)  <br>  {<br>    $colList = array(); <br>    foreach ($data as $key =&gt; $row)<br>    { <br>      $convertToLower = $caseInSensitive &amp;&amp; (in_array(SORT_STRING, $sortAttributes) || in_array(SORT_REGULAR, $sortAttributes)); <br>      $rowData = $convertToLower ? strtolower($row[$sortColumn]) : $row[$sortColumn]; <br>      $colLists[$sortColumn][$key] = $rowData;<br>    }<br>    $args[] = &amp;$colLists[$sortColumn];<br>      <br>    foreach($sortAttributes as $sortAttribute)<br>    {      <br>      $tmp[$i] = $sortAttribute;<br>      $args[] = &amp;$tmp[$i];<br>      $i++;      <br>     }<br>  }<br>  $args[] = &amp;$data;<br>  call_user_func_array(&apos;array_multisort&apos;, $args);<br>  return end($args);<br>} <br><br>Usage:<br><br>//Fill an array with random test data<br>define(&apos;MAX_ITEMS&apos;, 15);<br>define(&apos;MAX_VAL&apos;, 20);<br>for($i=0; $i &lt; MAX_ITEMS; $i++)<br>  $data[] = array(&apos;field1&apos; =&gt; rand(1, MAX_VAL), &apos;field2&apos; =&gt; rand(1, MAX_VAL), &apos;field3&apos; =&gt; rand(1, MAX_VAL) );<br>  <br>//Set the sort criteria (add as many fields as you want)<br>$sortCriteria = <br>  array(&apos;field1&apos; =&gt; array(SORT_DESC, SORT_NUMERIC), <br>       &apos;field3&apos; =&gt; array(SORT_DESC, SORT_NUMERIC)<br>  );<br><br>//Call it like this:  <br>$sortedData = MultiSort($data, $sortCriteria, true);  

#

A more inuitive way of sorting multidimensional arrays using array_msort() in just one line, you don&apos;t have to divide the original array into per-column-arrays:<br><br>

```
<?php

$arr1 = array(
    array('id'=>1,'name'=>'aA','cat'=>'cc'),
    array('id'=>2,'name'=>'aa','cat'=>'dd'),
    array('id'=>3,'name'=>'bb','cat'=>'cc'),
    array('id'=>4,'name'=>'bb','cat'=>'dd')
);

$arr2 = array_msort($arr1, array('name'=>SORT_DESC, 'cat'=>SORT_ASC));

debug($arr1, $arr2);

arr1:
    0:
        id: 1 (int)
        name: aA (string:2)
        cat: cc (string:2)
    1:
        id: 2 (int)
        name: aa (string:2)
        cat: dd (string:2)
    2:
        id: 3 (int)
        name: bb (string:2)
        cat: cc (string:2)
    3:
        id: 4 (int)
        name: bb (string:2)
        cat: dd (string:2)
arr2:
    2:
        id: 3 (int)
        name: bb (string:2)
        cat: cc (string:2)
    3:
        id: 4 (int)
        name: bb (string:2)
        cat: dd (string:2)
    0:
        id: 1 (int)
        name: aA (string:2)
        cat: cc (string:2)
    1:
        id: 2 (int)
        name: aa (string:2)
        cat: dd (string:2)

function array_msort($array, $cols)
{
    $colarr = array();
    foreach ($cols as $col => $order) {
        $colarr[$col] = array();
        foreach ($array as $k => $row) { $colarr[$col]['_'.$k] = strtolower($row[$col]); }
    }
    $eval = 'array_multisort(';
    foreach ($cols as $col => $order) {
        $eval .= '$colarr[\''.$col.'\'],'.$order.',';
    }
    $eval = substr($eval,0,-1).');';
    eval($eval);
    $ret = array();
    foreach ($colarr as $col => $arr) {
        foreach ($arr as $k => $v) {
            $k = substr($k,1);
            if (!isset($ret[$k])) $ret[$k] = $array[$k];
            $ret[$k][$col] = $array[$k][$col];
        }
    }
    return $ret;

}

?>
```
  

#

This is the simpler version of the function by AlberT.<br><br>A lot of times you have got an array like this:<br><br>$test[0][&apos;name&apos;]=&apos;Peter&apos;;<br>$test[0][&apos;points&apos;]=1;<br><br>$test[1][&apos;name&apos;]=&apos;Mike&apos;;<br>$test[1][&apos;points&apos;]=5;<br><br>$test[2][&apos;name&apos;]=&apos;John&apos;;<br>$test[2][&apos;points&apos;]=2;<br><br>You just want to sort on the index in the second dimension, ie. on points in the above example.<br> <br>You can use the function below and call it like this:<br><br>$test = multi_sort($test, $key = &apos;points&apos;);<br><br>function multi_sort($array, $akey)<br>{  <br>  function compare($a, $b)<br>  {<br>     global $key;<br>     return strcmp($a[$key], $b[$key]);<br>  } <br>  usort($array, "compare");<br>  return $array;<br>}<br><br>Note: to be able to use $key in the compare function, it can not simply be passed as a parameter. It has to be declared global and set somewhere outside of compare().  

#

USort function can be used to sort multidimensional arrays with almost no work whatsoever by using the individual values within the custom sort function.<br><br>This function passes the entire child element even if it is not a string. If it is an array, as would be the case in multidimensional arrays, it will pass the whole child array as one parameter.<br><br>Therefore, do something elegant like this:<br><br>

```
<?php
     // Sort the multidimensional array
     usort($results, "custom_sort");
     // Define the custom sort function
     function custom_sort($a,$b) {
          return $a['some_sub_var']&gt;$b['some_sub_var'];
     }
?>
```
<br><br>This does in 4 lines what other functions took 40 to 50 lines to do. This does not require you to create temporary arrays or anything. This is, for me, a highly preferred solution over this function.<br><br>Hope it helps!  

#

Easiest way I find out to sort an entire multidimensional array by one element of it:<br><br>

```
<?php
$multiArray = Array(
    Array("id" => 1, "name" => "Defg"),
    Array("id" => 2, "name" => "Abcd"),
    Array("id" => 3, "name" => "Bcde"),
    Array("id" => 4, "name" => "Cdef"));
$tmp = Array();
foreach($multiArray as &amp;$ma)
    $tmp[] = &amp;$ma["name"];
array_multisort($tmp, $multiArray);
foreach($multiArray as &amp;$ma)
    echo $ma["name"]."&lt;br/&gt;";
                
/* Outputs
    Abcd
    Bcde
    Cdef
    Defg
*/
?>
```
<br><br>^-^  

#

[Official documentation page](https://www.php.net/manual/en/function.array-multisort.php)

**[To root](/README.md)**
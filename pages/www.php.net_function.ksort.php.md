# ksort



A nice way to do sorting of a key on a multi-dimensional array without having to know what keys you have in the array first:<br><br>

```
<?php
$people = array(
array("name"=>"Bob","age"=>8,"colour"=>"red"),
array("name"=>"Greg","age"=>12,"colour"=>"blue"),
array("name"=>"Andy","age"=>5,"colour"=>"purple"));

var_dump($people);

$sortArray = array();

foreach($people as $person){
    foreach($person as $key=>$value){
        if(!isset($sortArray[$key])){
            $sortArray[$key] = array();
        }
        $sortArray[$key][] = $value;
    }
}

$orderby = "name"; //change this to whatever key you want from the array

array_multisort($sortArray[$orderby],SORT_DESC,$people);

var_dump($people);
?>
```
<br><br>Output from first var_dump:<br><br>[0]=&amp;gt;<br>  array(3) {<br>    ["name"]=&amp;gt;<br>    string(3) "Bob"<br>    ["age"]=&amp;gt;<br>    int(8)<br>    ["colour"]=&amp;gt;<br>    string(3) "red"<br>  }<br>  [1]=&amp;gt;<br>  array(3) {<br>    ["name"]=&amp;gt;<br><br>    string(4) "Greg"<br>    ["age"]=&amp;gt;<br>    int(12)<br>    ["colour"]=&amp;gt;<br>    string(4) "blue"<br>  }<br>  [2]=&amp;gt;<br>  array(3) {<br>    ["name"]=&amp;gt;<br>    string(4) "Andy"<br>    ["age"]=&amp;gt;<br>    int(5)<br>    ["colour"]=&amp;gt;<br><br>    string(6) "purple"<br>  }<br>}<br><br>Output from 2nd var_dump:<br><br>array(3) {<br>  [0]=&amp;gt;<br>  array(3) {<br>    ["name"]=&amp;gt;<br>    string(4) "Greg"<br>    ["age"]=&amp;gt;<br>    int(12)<br>    ["colour"]=&amp;gt;<br>    string(4) "blue"<br>  }<br>  [1]=&amp;gt;<br>  array(3) {<br>    ["name"]=&amp;gt;<br><br>    string(3) "Bob"<br>    ["age"]=&amp;gt;<br>    int(8)<br>    ["colour"]=&amp;gt;<br>    string(3) "red"<br>  }<br>  [2]=&amp;gt;<br>  array(3) {<br>    ["name"]=&amp;gt;<br>    string(4) "Andy"<br>    ["age"]=&amp;gt;<br>    int(5)<br>    ["colour"]=&amp;gt;<br><br>    string(6) "purple"<br>  }<br><br>There&apos;s no checking on whether your array keys exist, or the array data you are searching on is actually there, but easy enough to add.  

#

I wrote this function to sort the keys of an array using an array of keynames, in order.<br>

```
<?php
/**
 * function array_reorder_keys
 * reorder the keys of an array in order of specified keynames; all other nodes not in $keynames will come after last $keyname, in normal array order
 * @param array &amp;$array - the array to reorder
 * @param mixed $keynames - a csv or array of keynames, in the order that keys should be reordered
 */
function array_reorder_keys(&amp;$array, $keynames){
    if(empty($array) || !is_array($array) || empty($keynames)) return;
    if(!is_array($keynames)) $keynames = explode(',',$keynames);
    if(!empty($keynames)) $keynames = array_reverse($keynames);
    foreach($keynames as $n){
        if(array_key_exists($n, $array)){
            $newarray = array($n=>$array[$n]); //copy the node before unsetting
            unset($array[$n]); //remove the node
            $array = $newarray + array_filter($array); //combine copy with filtered array
        }
    }
}
$seed_array = array('foo'=>'bar', 'someotherkey'=>'whatev', 'bar'=>'baz', 'baz'=>'foo', 'anotherkey'=>'anotherval');
array_reorder_keys($seed_array, 'baz,foo,bar'); //returns array('baz'=>'foo', 'foo'=>'bar', 'bar'=>'baz', 'someotherkey'=>'whatev', 'anotherkey'=>'anotherval' );
?>
```
  

#

Here is a function to sort an array by the key of his sub-array.<br><br>

```
<?php

function sksort(&amp;$array, $subkey="id", $sort_ascending=false) {

    if (count($array))
        $temp_array[key($array)] = array_shift($array);

    foreach($array as $key => $val){
        $offset = 0;
        $found = false;
        foreach($temp_array as $tmp_key => $tmp_val)
        {
            if(!$found and strtolower($val[$subkey]) &gt; strtolower($tmp_val[$subkey]))
            {
                $temp_array = array_merge(    (array)array_slice($temp_array,0,$offset),
                                            array($key => $val),
                                            array_slice($temp_array,$offset)
                                          );
                $found = true;
            }
            $offset++;
        }
        if(!$found) $temp_array = array_merge($temp_array, array($key => $val));
    }

    if ($sort_ascending) $array = array_reverse($temp_array);

    else $array = $temp_array;
}

?>
```


Example


```
<?php
$info = array("peter" => array("age" => 21,
                                           "gender" => "male"
                                           ),
                   "john"  => array("age" => 19,
                                           "gender" => "male"
                                           ),
                   "mary" => array("age" => 20,
                                           "gender" => "female"
                                          )
                  );

sksort($info, "age");
var_dump($info);

sksort($info, "age", true);
var_dump($ifno);
?>
```
<br><br>This will be the output of the example:<br><br>/*DESCENDING SORT*/<br>array(3) {<br>  ["peter"]=&gt;<br>  array(2) {<br>    ["age"]=&gt;<br>    int(21)<br>    ["gender"]=&gt;<br>    string(4) "male"<br>  }<br>  ["mary"]=&gt;<br>  array(2) {<br>    ["age"]=&gt;<br>    int(20)<br>    ["gender"]=&gt;<br>    string(6) "female"<br>  }<br>  ["john"]=&gt;<br>  array(2) {<br>    ["age"]=&gt;<br>    int(19)<br>    ["gender"]=&gt;<br>    string(4) "male"<br>  }<br>}<br><br>/*ASCENDING SORT*/<br>array(3) {<br>  ["john"]=&gt;<br>  array(2) {<br>    ["age"]=&gt;<br>    int(19)<br>    ["gender"]=&gt;<br>    string(4) "male"<br>  }<br>  ["mary"]=&gt;<br>  array(2) {<br>    ["age"]=&gt;<br>    int(20)<br>    ["gender"]=&gt;<br>    string(6) "female"<br>  }<br>  ["peter"]=&gt;<br>  array(2) {<br>    ["age"]=&gt;<br>    int(21)<br>    ["gender"]=&gt;<br>    string(4) "male"<br>  }<br>}  

#

[Official documentation page](https://www.php.net/manual/en/function.ksort.php)

**[To root](/README.md)**
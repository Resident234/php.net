# array_unique



Create multidimensional array unique for any single key index.<br>e.g I want to create multi dimentional unique array for specific code<br><br>Code : <br>My array is like this,<br><br>

```
<?php
$details = array(
    0 =&gt; array("id"=&gt;"1", "name"=&gt;"Mike",    "num"=&gt;"9876543210"),
    1 =&gt; array("id"=&gt;"2", "name"=&gt;"Carissa", "num"=&gt;"08548596258"),
    2 =&gt; array("id"=&gt;"1", "name"=&gt;"Mathew",  "num"=&gt;"784581254"),
);
?>
```


You can make it unique for any field like id, name or num.

I have develop this function for same : 


```
<?php
function unique_multidim_array($array, $key) {
    $temp_array = array();
    $i = 0;
    $key_array = array();
    
    foreach($array as $val) {
        if (!in_array($val[$key], $key_array)) {
            $key_array[$i] = $val[$key];
            $temp_array[$i] = $val;
        }
        $i++;
    }
    return $temp_array;
}
?>
```


Now, call this function anywhere from your code,

something like this,


```
<?php
$details = unique_multidim_array($details,&apos;id&apos;);
?>
```


Output will be like this :


```
<?php
$details = array(
    0 =&gt; array("id"=&gt;"1","name"=&gt;"Mike","num"=&gt;"9876543210"),
    1 =&gt; array("id"=&gt;"2","name"=&gt;"Carissa","num"=&gt;"08548596258"),
);
?>
```
  

#

It&apos;s often faster to use a foreache and array_keys than array_unique:<br><br>    

```
<?php

    $max = 1000000;
    $arr = range(1,$max,3);
    $arr2 = range(1,$max,2);
    $arr = array_merge($arr,$arr2);

    $time = -microtime(true);
    $res1 = array_unique($arr);
    $time += microtime(true);
    echo "deduped to ".count($res1)." in ".$time;
    // deduped to 666667 in 32.300781965256

    $time = -microtime(true);
    $res2 = array();
    foreach($arr as $key=&gt;$val) {    
        $res2[$val] = true;
    }
    $res2 = array_keys($res2);
    $time += microtime(true);
    echo "&lt;br /&gt;deduped to ".count($res2)." in ".$time;
    // deduped to 666667 in 0.84372591972351

    ?>
```
  

#

For people looking at the flip flip method for getting unique values in a simple array. This is the absolute fastest method:<br><br>

```
<?php
$unique = array_keys(array_flip($array));
?>
```


It&apos;s marginally faster as:


```
<?php
$unique = array_merge(array_flip(array_flip($array)));
?>
```


And it&apos;s marginally slower as:


```
<?php
$unique array_flip(array_flip($array)); // leaves gaps
?>
```
<br><br>It&apos;s still about twice as fast or fast as array_unique.<br><br>This tested on several different machines with 100000 random arrays. All machines used a version of PHP5.  

#

I needed to identify email addresses in a data table that were replicated, so I wrote the array_not_unique() function:<br><br>

```
<?php

function array_not_unique($raw_array) {
    $dupes = array();
    natcasesort($raw_array);
    reset ($raw_array);

    $old_key    = NULL;
    $old_value    = NULL;
    foreach ($raw_array as $key =&gt; $value) {
        if ($value === NULL) { continue; }
        if ($old_value == $value) {
            $dupes[$old_key]    = $old_value;
            $dupes[$key]        = $value;
        }
        $old_value    = $value;
        $old_key    = $key;
    }
return $dupes;
}

$raw_array     = array();
$raw_array[1]    = &apos;abc@xyz.com&apos;;
$raw_array[2]    = &apos;def@xyz.com&apos;;
$raw_array[3]    = &apos;ghi@xyz.com&apos;;
$raw_array[4]    = &apos;abc@xyz.com&apos;; // Duplicate

$common_stuff    = array_not_unique($raw_array);
var_dump($common_stuff);
?>
```
  

#

Case insensitive; will keep first encountered value.<br><br>

```
<?php

function array_iunique($array) {
    $lowered = array_map(&apos;strtolower&apos;, $array);
    return array_intersect_key($array, array_unique($lowered));
}

?>
```
  

#

recursive array unique for multiarrays<br><br>

```
<?php
function super_unique($array)
{
  $result = array_map("unserialize", array_unique(array_map("serialize", $array)));

  foreach ($result as $key =&gt; $value)
  {
    if ( is_array($value) )
    {
      $result[$key] = super_unique($value);
    }
  }

  return $result;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-unique.php)

**[To root](/README.md)**
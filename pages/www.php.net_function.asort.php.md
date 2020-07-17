# asort



This function can be used to sort multidimensional arrays with almost no work whatsoever by using the individual values within the custom sort function.<br><br>This function passes the entire child element even if it is not a string. If it is an array, as would be the case in multidimensional arrays, it will pass the whole child array as one parameter.<br><br>Therefore, do something elegant like this:<br><br>

```
<?php
     // Sort the multidimensional array
     usort($results, "custom_sort");
     // Define the custom sort function
     function custom_sort($a,$b) {
          return $a['some_sub_var']>$b['some_sub_var'];
     }
?>
```
<br><br>This does in 4 lines what other functions took 40 to 50 lines to do. This does not require you to create temporary arrays or anything. This is, for me, a highly preferred solution.<br><br>Hope it helps!  

#

/*<br> * Name : Aditya Mehrotra <br> * Email: aditycse@gmail.com<br> */<br>//Example for sorting by values for an alphanumeric array also having case-sensitive data<br>$exampleArray1 = $exampleArray2 = array(<br>    0 =&gt; &apos;example1&apos;,<br>    1 =&gt; &apos;Example10&apos;,<br>    2 =&gt; &apos;example12&apos;,<br>    3 =&gt; &apos;Example2&apos;,<br>    4 =&gt; &apos;example3&apos;,<br>    5 =&gt; &apos;EXAMPLE10&apos;,<br>    6 =&gt; &apos;example10&apos;<br>);<br><br>//default sorting<br>asort($exampleArray1);<br><br>// alphanumeric with case-sensitive data sorting by values<br>asort($exampleArray2, SORT_STRING | SORT_FLAG_CASE | SORT_NATURAL);<br><br>//output of defaut sorting<br>print_r($exampleArray1);<br>/*<br> * output of default sorting<br>  Array<br>  (<br>  [5] =&gt; EXAMPLE10<br>  [1] =&gt; Example10<br>  [3] =&gt; Example2<br>  [0] =&gt; example1<br>  [6] =&gt; example10<br>  [2] =&gt; example12<br>  [4] =&gt; example3<br>  )<br> */<br><br>print_r($exampleArray2);<br>/*<br> * output of alphanumeric with case-sensitive data sorting by values<br> Array<br>(<br>    [0] =&gt; example1<br>    [3] =&gt; Example2<br>    [4] =&gt; example3<br>    [5] =&gt; EXAMPLE10<br>    [1] =&gt; Example10<br>    [6] =&gt; example10<br>    [2] =&gt; example12<br>)<br> */  

#

For a recent project I needed to sort an associative array by value first, and then by key if a particular value appeared multiple times. I wrote this function to accomplish the task. Note that the parameters default to sort ascending on both keys and values, but allow granular control over each.<br><br>

```
<?php
function aksort(&amp;$array,$valrev=false,$keyrev=false) {
  if ($valrev) { arsort($array); } else { asort($array); }
    $vals = array_count_values($array);
    $i = 0;
    foreach ($vals AS $val=>$num) {
        $first = array_splice($array,0,$i);
        $tmp = array_splice($array,0,$num);
        if ($keyrev) { krsort($tmp); } else { ksort($tmp); }
        $array = array_merge($first,$tmp,$array);
        unset($tmp);
        $i = $num;
    }
}

// Example
$tmp = array('ca'=>1,'cb'=>2,'ce'=>1,'pa'=>2,'pe'=>1);

// Standard asort
asort($tmp);
print_r($tmp);

// Sort value ASC, key ASC
aksort($tmp);
print_r($tmp);

// Sort value DESC, key ASC
aksort($tmp,true);
print_r($tmp);

// Sort value DESC, key DESC
aksort($tmp,true,true);
print_r($tmp);

// Results
Array
(
    [pe] => 1
    [ca] => 1
    [ce] => 1
    [cb] => 2
    [pa] => 2
)
Array
(
    [ca] => 1
    [ce] => 1
    [pe] => 1
    [cb] => 2
    [pa] => 2
)
Array
(
    [cb] => 2
    [pa] => 2
    [ca] => 1
    [ce] => 1
    [pe] => 1
)
Array
(
    [pa] => 2
    [cb] => 2
    [pe] => 1
    [ce] => 1
    [ca] => 1
)?>
```
  

#

This is a function to sort an indexed 2D array by a specified sub array key, either ascending or descending.<br><br>It is usefull for sorting query results from a database by a particular field after the query has been returned<br><br>This function can be quite greedy. It recreates the array as a hash to use ksort() then back again<br><br>By default it will sort ascending but if you specify $reverse as true it will return the records sorted descending <br><br>

```
<?php

function record_sort($records, $field, $reverse=false)
{
    $hash = array();
    
    foreach($records as $record)
    {
        $hash[$record[$field]] = $record;
    }
    
    ($reverse)? krsort($hash) : ksort($hash);
    
    $records = array();
    
    foreach($hash as $record)
    {
        $records []= $record;
    }
    
    return $records;
}

// Example below

$airports = array
(
    array( "code" => "LHR", "name" => "Heathrow" ),
    array( "code" => "LGW", "name" => "Gatwick" ),
);

printf("Before: <pre>%s</pre>", print_r($airports, true));

$airports = record_sort($airports, "name");

printf("After: <pre>%s</pre>", print_r($airports, true));

?>
```
<br><br>Example Outputs:<br><br>Before: Array<br>(<br>    [0] =&gt; Array ( [code] =&gt; LHR, [name] =&gt; Heathrow )<br>    [1] =&gt; Array ( [code] =&gt; LGW, [name] =&gt; Gatwick )<br>)<br><br>After: Array<br>(<br>    [0] =&gt; Array ( [code] =&gt; LGW, [name] =&gt; Gatwick )<br>    [1] =&gt; Array ( [code] =&gt; LHR, [name] =&gt; Heathrow )<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.asort.php)

**[To root](/README.md)**
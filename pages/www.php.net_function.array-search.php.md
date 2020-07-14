# array_search



in (PHP 5 &gt;= 5.5.0) you don&apos;t have to write your own function to search through a multi dimensional array<br><br>ex : <br><br>$userdb=Array<br>(<br>    (0) =&gt; Array<br>        (<br>            (uid) =&gt; &apos;100&apos;,<br>            (name) =&gt; &apos;Sandra Shush&apos;,<br>            (url) =&gt; &apos;urlof100&apos;<br>        ),<br><br>    (1) =&gt; Array<br>        (<br>            (uid) =&gt; &apos;5465&apos;,<br>            (name) =&gt; &apos;Stefanie Mcmohn&apos;,<br>            (pic_square) =&gt; &apos;urlof100&apos;<br>        ),<br><br>    (2) =&gt; Array<br>        (<br>            (uid) =&gt; &apos;40489&apos;,<br>            (name) =&gt; &apos;Michael&apos;,<br>            (pic_square) =&gt; &apos;urlof40489&apos;<br>        )<br>);<br><br>simply u can use this<br><br>$key = array_search(40489, array_column($userdb, &apos;uid&apos;));  

#

About searcing in multi-dimentional arrays; two notes on "xfoxawy at gmail dot com";<br><br>It perfectly searches through multi-dimentional arrays combined with array_column() (min php 5.5.0) but it may not return the values you&apos;d expect.<br><br>

```
<?php array_search($needle, array_column($array, &apos;key&apos;)); ?>
```


Since array_column() will produce a resulting array; it won&apos;t preserve your multi-dimentional array&apos;s keys. So if you check against your keys, it will fail.

For example;



```
<?php
$people = array(
  2 =&gt; array(
    &apos;name&apos; =&gt; &apos;John&apos;,
    &apos;fav_color&apos; =&gt; &apos;green&apos;
  ),
  5=&gt; array(
    &apos;name&apos; =&gt; &apos;Samuel&apos;,
    &apos;fav_color&apos; =&gt; &apos;blue&apos;
  )
);

$found_key = array_search(&apos;blue&apos;, array_column($people, &apos;fav_color&apos;));
?>
```


Here, you could expect that the $found_key would be "5" but it&apos;s NOT. It will be 1. Since it&apos;s the second element of the produced array by the array_column() function.

Secondly, if your array is big, I would recommend you to first assign a new variable so that it wouldn&apos;t call array_column() for each element it searches. For a better performance, you could do;



```
<?php
$colors = array_column($people, &apos;fav_color&apos;);
$found_key = array_search(&apos;blue&apos;, $colors);
?>
```
  

#

$array = [&apos;a&apos;, &apos;b&apos;, &apos;c&apos;];<br>$key = array_search(&apos;a&apos;, $array); //$key = 0<br>if ($key) <br>{<br>//even a element is found in array, but if (0) means false<br>//...<br>}<br><br>//the correct way<br>if (false !== $key)<br>{<br>//....<br>}<br><br>It&apos;s what the document stated "may also return a non-Boolean value which evaluates to FALSE."  

#

the recursive function by tony have a small bug. it failes when a key is 0<br><br>here is the corrected version of this helpful function:<br><br>

```
<?php
function recursive_array_search($needle,$haystack) {
    foreach($haystack as $key=&gt;$value) {
        $current_key=$key;
        if($needle===$value OR (is_array($value) &amp;&amp; recursive_array_search($needle,$value) !== false)) {
            return $current_key;
        }
    }
    return false;
}
?>
```
  

#

If you are using the result of array_search in a condition statement, make sure you use the === operator instead of == to test whether or not it found a match.  Otherwise, searching through an array with numeric indicies will result in index 0 always getting evaluated as false/null.  This nuance cost me a lot of time and sanity, so I hope this helps someone.  In case you don&apos;t know what I&apos;m talking about, here&apos;s an example:<br><br>

```
<?php
$code = array("a", "b", "a", "c", "a", "b", "b"); // infamous abacabb mortal kombat code :-P

// this is WRONG
while (($key = array_search("a", $code)) != NULL)
{
 // infinite loop, regardless of the unset
 unset($code[$key]);
}

// this is _RIGHT_
while (($key = array_search("a", $code)) !== NULL)
{
 // loop will terminate
 unset($code[$key]);
}
?>
```
  

#

for searching case insensitive better this:<br><br>

```
<?php
array_search(strtolower($element),array_map(&apos;strtolower&apos;,$array));
?>
```
  

#

To expand on previous comments, here are some examples of<br>where using array_search within an IF statement can go<br>wrong when you want to use the array key thats returned.<br><br>Take the following two arrays you wish to search:<br><br>

```
<?php
$fruit_array = array("apple", "pear", "orange");
$fruit_array = array("a" =&gt; "apple", "b" =&gt; "pear", "c" =&gt; "orange");

if ($i = array_search("apple", $fruit_array))
//PROBLEM: the first array returns a key of 0 and IF treats it as FALSE

if (is_numeric($i = array_search("apple", $fruit_array)))
//PROBLEM: works on numeric keys of the first array but fails on the second

if ($i = is_numeric(array_search("apple", $fruit_array)))
//PROBLEM: using the above in the wrong order causes $i to always equal 1

if ($i = array_search("apple", $fruit_array) !== FALSE)
//PROBLEM: explicit with no extra brackets causes $i to always equal 1

if (($i = array_search("apple", $fruit_array)) !== FALSE)
//YES: works on both arrays returning their keys
?>
```
  

#

hey i have a easy multidimensional array search function <br><br>

```
<?php
function search($array, $key, $value)
{
    $results = array();

    if (is_array($array))
    {
        if (isset($array[$key]) &amp;&amp; $array[$key] == $value)
            $results[] = $array;

        foreach ($array as $subarray)
            $results = array_merge($results, search($subarray, $key, $value));
    }

    return $results;
}
?>
```
  

#

Better solution of multidimensional searching.<br><br>

```
<?php
function multidimensional_search($parents, $searched) {
  if (empty($searched) || empty($parents)) {
    return false;
  }
 
  foreach ($parents as $key =&gt; $value) {
    $exists = true;
    foreach ($searched as $skey =&gt; $svalue) {
      $exists = ($exists &amp;&amp; IsSet($parents[$key][$skey]) &amp;&amp; $parents[$key][$skey] == $svalue);
    }
    if($exists){ return $key; }
  }
 
  return false;
}

$parents = array();
$parents[] = array(&apos;date&apos;=&gt;1320883200, &apos;uid&apos;=&gt;3);
$parents[] = array(&apos;date&apos;=&gt;1320883200, &apos;uid&apos;=&gt;5);
$parents[] = array(&apos;date&apos;=&gt;1318204800, &apos;uid&apos;=&gt;5);

echo multidimensional_search($parents, array(&apos;date&apos;=&gt;1320883200, &apos;uid&apos;=&gt;5)); // 1
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-search.php)

**[To root](/README.md)**
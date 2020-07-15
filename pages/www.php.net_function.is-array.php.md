# is_array



Please note that the &apos;cast to array&apos; check is horrendously out of date.<br><br>Running that code against PHP 5.6 results in this:<br><br>is_array  :  0.93975400924683<br>cast, === :  1.2425191402435<br><br>So, please use &apos;is_array&apos;, not the horrible casting hack.  

#

Or you could make use of the array_diff_key and array_key function:<br><br>

```
<?php

function is_assoc($var)
{
        return is_array($var) &amp;&amp; array_diff_key($var,array_keys(array_keys($var)));
}

function test($var)
{
        echo is_assoc($var) ? "I'm an assoc array.\n" : "I'm not an assoc array.\n";
}

// an assoc array
$a = array("a"=>"aaa","b"=>1,"c"=>true);
test($a);

// an array
$b = array_values($a);
test($b);

// an object
$c = (object)$a;
test($c);

// other types
test($a->a);
test($a->b);
test($a->c);

?>
```
<br><br>The above code outputs:<br>I&apos;m an assoc array.<br>I&apos;m not an assoc array.<br>I&apos;m not an assoc array.<br>I&apos;m not an assoc array.<br>I&apos;m not an assoc array.<br>I&apos;m not an assoc array.  

#

Yet another simpler, faster is_assoc():<br><br>

```
<?php
function is_assoc($array) {
  foreach (array_keys($array) as $k => $v) {
    if ($k !== $v)
      return true;
  }
  return false;
}
?>
```
<br><br>In my tests it runs about twice as fast as Michael/Gabriel&apos;s array_reduce() method.<br><br>(Speaking of which: Gabriel&apos;s version doesn&apos;t work as written; it reports associative arrays as numeric if only the first key is non-numeric, or if the keys are numeric but ordered backwards.  Michael solves this problem by comparing array_reduce() to count(), but that costs another function call; it also works to just compare to -1 instead of 0, and therefore return -1 as the ternary else from the callback).  

#

I&apos;ve found a faster way of determining an array. If you use is_array() millions of times, you will notice a *huge* difference. On my machine, this method takes about 1/4 the time of using is_array().<br><br>Cast the value to an array, then check (using ===) if it is identical to the original.<br><br>

```
<?php
if ( (array) $unknown !== $unknown ) {
    echo '$unknown is not an array';
} else {
    echo '$unknown is an array';
}
?>
```


You can use this script to test the speed of both methods.

&lt;pre&gt;
What's faster for determining arrays?



```
<?php

$count = 1000000;

$test = array('im', 'an', 'array');
$test2 = 'im not an array';
$test3 = (object) array('im' => 'not', 'going' => 'to be', 'an' => 'array');
$test4 = 42;
// Set this now so the first for loop doesn't do the extra work.
$i = $start_time = $end_time = 0;

$start_time = microtime(true);
for ($i = 0; $i &lt; $count; $i++) {
    if (!is_array($test) || is_array($test2) || is_array($test3) || is_array($test4)) {
        echo 'error';
        break;
    }
}
$end_time = microtime(true);
echo 'is_array  :  '.($end_time - $start_time)."\n";

$start_time = microtime(true);
for ($i = 0; $i &lt; $count; $i++) {
    if (!(array) $test === $test || (array) $test2 === $test2 || (array) $test3 === $test3 || (array) $test4 === $test4) {
        echo 'error';
        break;
    }
}
$end_time = microtime(true);
echo 'cast, === :  '.($end_time - $start_time)."\n";

echo "\nTested $count iterations."

?>
```
<br>&lt;/pre&gt;<br><br>Prints something like:<br><br>What&apos;s faster for determining arrays?<br><br>is_array  :  7.9920151233673<br>cast, === :  1.8978719711304<br><br>Tested 1000000 iterations.  

#

hperrin&apos;s results have indeed changed in PHP 7. The opposite is now true, is_array is faster than comparison:<br><br>is_array  :  0.52148389816284<br>cast, === :  0.84179711341858<br><br>Tested 1000000 iterations.  

#

yousef&apos;s example was wrong because is_vector returned true instead of false if the key was found<br>here is the fixed version (only 2 lines differ)<br>

```
<?php
function is_vector( &amp;$array ) {
   if ( !is_array($array) || empty($array) ) {
      return -1;
   }
   $next = 0;
   foreach ( $array as $k => $v ) {
      if ( $k !== $next ) return false;
      $next++;
   }
   return true;
}
?>
```
  

#

alex frase&apos;s example is fast but elanthis at awesomeplay dot com&apos;s example is faster and Ilgar&apos;s modification of alex&apos;s code is faulty (the part " || $_array[$k] !== $v"). Also, Ilgar&apos;s suggestion of giving a false return value when the variable isnt an array is not suitable in my opinion and i think checking if the array is empty would also be a suitable check before the rest of the code runs.<br><br>So here&apos;s the modified (is_vector) version<br><br>

```
<?php
function is_vector( &amp;$array ) {
   if ( !is_array($array) || empty($array) ) {
      return -1;
   }
   $next = 0;
   foreach ( $array as $k => $v ) {
      if ( $k !== $next ) return true;
      $next++;
   }
   return false;
}
?>
```


and the modified (alex's is_assoc) version



```
<?php
function is_assoc($_array) {
    if ( !is_array($_array) || empty($array) ) {
        return -1;
    }
    foreach (array_keys($_array) as $k => $v) {
        if ($k !== $v) {
            return true;
        }
    }
    return false;
}
?>
```
  

#

I would change the order of the comparison, because if it is really an empty array, it is better to stop at that point before doing several &apos;cpu &amp; memory intensive&apos; function calls.<br><br>In the end on a ratio of 3 not empty arrays to 1 empty array computed for 1000000 iterations it needed 10% less time.<br>Or the other way round:<br>It needed approx 3% to 4% more time if the array is not empty, but was at least 4 times faster on empty arrays.<br><br>Additionally the memory consumption veritably lesser.<br><br>

```
<?php
function is_assoc($array) {
    return (is_array($array) &amp;&amp; (count($array)==0 || 0 !== count(array_diff_key($array, array_keys(array_keys($array))) )));
} 
?>
```
  

#

function is_associate_array($array)<br>{<br>    return $array === array_values($array);<br>}<br><br>or you can add check is_array in functions  

#

The is_associative_array() and is_sequential_array() functions posted by &apos;rjg4013 at rit dot edu&apos; are not accurate.<br><br>The functions fail to recognize indexes that are not in sequence or in order.  For example, array(0=&gt;&apos;a&apos;, 2=&gt;&apos;b&apos;, 1=&gt;&apos;c&apos;) and array(0=&gt;&apos;a&apos;, 3=&gt;&apos;b&apos;, 5=&gt;&apos;c&apos;) would be considered as sequential arrays. A true sequential array would be in consecutive order with no gaps in the indices.<br><br>The following solution utilizes the array_merge properties. If only one array is given and the array is numerically indexed, the keys get re-indexed in a continuous way.  The result must match the array passed to it in order to truly be a numerically indexed (sequential) array.  Otherwise it can be assumed to be an associative array (something unobtainable in languages such as C).<br><br>The following functions will work for PHP &gt;= 4.<br><br>

```
<?php
    function is_sequential_array($var)
    {
        return (array_merge($var) === $var &amp;&amp; is_numeric( implode( array_keys( $var ) ) ) );
    }
    
    function is_assoc_array($var)
    {
        return (array_merge($var) !== $var || !is_numeric( implode( array_keys( $var ) ) ) );
    }
?>
```
<br><br>If you are not concerned about the actual order of the indices, you can change the comparison to == and != respectively.  

#

And here is another variation for a function to test if an array is associative. Based on the idea by mot4h.<br><br>

```
<?php
function is_associative($array)
{
  if (!is_array($array) || empty($array))
    return false;

  $keys = array_keys($array);
  return array_keys($keys) !== $keys;
}
?>
```
  

#

A slight modification of what&apos;s below:<br><br>

```
<?php

function is_assoc($array)
{
    return is_array($array) &amp;&amp; count($array) !== array_reduce(array_keys($array), 'is_assoc_callback', 0);
}

function is_assoc_callback($a, $b)
{
    return $a === $b ? $a + 1 : 0;
}

?>
```
  

#

Using empty() in the previous example posted by Anonymous will result in a "Fatal error: Can&apos;t use function return value in write context".  I suggest using count() instead:<br><br>

```
<?php
function is_assoc($array) {
    return (is_array($array) &amp;&amp; 0 !== count(array_diff_key($array, array_keys(array_keys($array)))));
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-array.php)

**[To root](/README.md)**
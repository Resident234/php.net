# key



Note that using key($array) in a foreach loop may have unexpected results.  <br><br>When requiring the key inside a foreach loop, you should use:<br>foreach($array as $key =&gt; $value)<br><br>I was incorrectly using:<br>

```
<?php
foreach($array as $value)
{
  $mykey = key($array);
}
?>
```


and experiencing errors (the pointer of the array is already moved to the next item, so instead of getting the key for $value, you will get the key to the next value in the array)

CORRECT:


```
<?php<br>foreach($array as $key =&gt; $value)<br>{<br>  $mykey = $key;<br>}<br><br>A noob error, but felt it might help someone else out there.  

#

Suppose if the array values are in numbers and numbers contains `0` then the loop will be terminated. To overcome this you can user like this<br><br>

```
<?php
$array = array(
    &apos;0&apos; =&gt; &apos;5&apos;,
    &apos;1&apos; =&gt; &apos;2&apos;,
    &apos;2&apos; =&gt; &apos;0&apos;,
    &apos;3&apos; =&gt; &apos;3&apos;,
    &apos;4&apos; =&gt; &apos;1&apos;);

// wrong approach

while ($fruit_name = current($array)) {

        echo key($array).&apos;&lt;br /&gt;&apos;;
       next($array);
}

// the way will be break loop when arra(&apos;2&apos;=&gt;0) because its value is &apos;0&apos;, while(0) will terminate the loop

// correct approach
while ( ($fruit_name = current($array)) !== FALSE ) {

        echo key($array).&apos;&lt;br /&gt;&apos;;
       next($array);
}
//this will work properly
?>
```
  

#

Needed to get the index of the max/highest value in an assoc array.<br>max() only returned the value, no index, so I did this instead.<br><br>

```
<?php
reset($x);   // optional.
arsort($x);
$key_of_max = key($x);   // returns the index.
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.key.php)

**[To root](/README.md)**
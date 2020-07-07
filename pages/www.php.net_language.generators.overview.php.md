# Generators overview





for the protection from the leaking of resources 
see RFC https://wiki.php.net/rfc/generators#closing_a_generator

and use finnaly

sample code

function getLines($file) {
&#xA0; &#xA0; $f = fopen($file, &apos;r&apos;);
&#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; while ($line = fgets($f)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; yield $line;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; } finally {
&#xA0; &#xA0; &#xA0; &#xA0; fclose($f);
&#xA0; &#xA0; }
}

foreach (getLines(&quot;file.txt&quot;) as $n =&gt; $line) {
&#xA0; &#xA0; if ($n &gt; 5) break;
&#xA0; &#xA0; echo $line;
}

  

#



Bear in mind that execution of a generator function is postponed until iteration over its result (the Generator object) begins. This might confuse one if the result of a generator is assigned to a variable instead of immediate iteration.



```
<?php

$some_state = &apos;initial&apos;;

function gen() {
&#xA0; &#xA0; global $some_state; 

&#xA0; &#xA0; echo &quot;gen() execution start\n&quot;;
&#xA0; &#xA0; $some_state = &quot;changed&quot;;

&#xA0; &#xA0; yield 1;
&#xA0; &#xA0; yield 2;
}

function peek_state() {
&#xA0; &#xA0; global $some_state;
&#xA0; &#xA0; echo &quot;\$some_state = $some_state\n&quot;;
}

echo &quot;calling gen()...\n&quot;;
$result = gen();
echo &quot;gen() was called\n&quot;;

peek_state();

echo &quot;iterating...\n&quot;;
foreach ($result as $val) {
&#xA0; &#xA0; echo &quot;iteration: $val\n&quot;;
&#xA0; &#xA0; peek_state();
}

?>
```


If you need to perform some action when the function is called and before the result is used, you&apos;ll have to wrap your generator in another function.



```
<?php
/**
&#xA0; * @return Generator
&#xA0; */
function some_generator() {
&#xA0; &#xA0; global $some_state;

&#xA0; &#xA0; $some_state = &quot;changed&quot;;
&#xA0; &#xA0; return gen();
}
?>
```



  

#



Here&apos;s how to detect loop breaks, and how to handle or cleanup after an interruption.



```
<?php
&#xA0; &#xA0; function generator()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $complete = false;
&#xA0; &#xA0; &#xA0; &#xA0; try {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while (($result = some_function())) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; yield $result;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $complete = true;

&#xA0; &#xA0; &#xA0; &#xA0; } finally {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!$complete) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // cleanup when loop breaks 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // cleanup when loop completes
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Do something only after loop completes
&#xA0; &#xA0; }
?>
```



  

#



Same example, different results:

----------------------------------
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; time&#xA0; | memory, mb |
----------------------------------
| not gen&#xA0; | 0.7589 | 146.75&#xA0; &#xA0;&#xA0; |
|---------------------------------
| with gen | 0.7469 | 8.75&#xA0; &#xA0; &#xA0;&#xA0; |
|---------------------------------

Time in results varying from 6.5 to 7.8 on both examples.
So no real drawbacks concerning processing speed.

  

#



Abstract test.


```
<?php

$start_time=microtime(true);
$array = array();
$result = &apos;&apos;;
for($count=1000000; $count--;)
{
&#xA0; $array[]=$count/2;
}
foreach($array as $val)
{
&#xA0; $val += 145.56;
&#xA0; $result .= $val;
}
$end_time=microtime(true);

echo &quot;time: &quot;, bcsub($end_time, $start_time, 4), &quot;\n&quot;;
echo &quot;memory (byte): &quot;, memory_get_peak_usage(true), &quot;\n&quot;;

?>
```




```
<?php

$start_time=microtime(true);
$result = &apos;&apos;;
function it()
{
&#xA0; for($count=1000000; $count--;)
&#xA0; {
&#xA0; &#xA0; yield $count/2;
&#xA0; }
}
foreach(it() as $val)
{
&#xA0; $val += 145.56;
&#xA0; $result .= $val;
}
$end_time=microtime(true);

echo &quot;time: &quot;, bcsub($end_time, $start_time, 4), &quot;\n&quot;;
echo &quot;memory (byte): &quot;, memory_get_peak_usage(true), &quot;\n&quot;;

?>
```

Result:
----------------------------------
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; |&#xA0; time&#xA0; | memory, mb |
----------------------------------
| not gen&#xA0; | 2.1216 | 89.25&#xA0; &#xA0; &#xA0; |
|---------------------------------
| with gen | 6.1963 | 8.75&#xA0; &#xA0; &#xA0;&#xA0; |
|---------------------------------
| diff&#xA0; &#xA0;&#xA0; | &lt; 192% | &gt; 90%&#xA0; &#xA0; &#xA0; |
----------------------------------

  

#

[Official documentation page](https://www.php.net/manual/en/language.generators.overview.php)

**[To root](/README.md)**
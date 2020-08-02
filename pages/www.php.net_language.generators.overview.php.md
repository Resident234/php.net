# Generators overview



for the protection from the leaking of resources <br>see RFC https://wiki.php.net/rfc/generators#closing_a_generator<br><br>and use finnaly<br><br>sample code<br><br>function getLines($file) {<br>    $f = fopen($file, &apos;r&apos;);<br>    try {<br>        while ($line = fgets($f)) {<br>            yield $line;<br>        }<br>    } finally {<br>        fclose($f);<br>    }<br>}<br><br>foreach (getLines("file.txt") as $n =&gt; $line) {<br>    if ($n &gt; 5) break;<br>    echo $line;<br>}  

---

Bear in mind that execution of a generator function is postponed until iteration over its result (the Generator object) begins. This might confuse one if the result of a generator is assigned to a variable instead of immediate iteration.<br><br>

```
<?php

$some_state = 'initial';

function gen() {
    global $some_state; 

    echo "gen() execution start\n";
    $some_state = "changed";

    yield 1;
    yield 2;
}

function peek_state() {
    global $some_state;
    echo "\$some_state = $some_state\n";
}

echo "calling gen()...\n";
$result = gen();
echo "gen() was called\n";

peek_state();

echo "iterating...\n";
foreach ($result as $val) {
    echo "iteration: $val\n";
    peek_state();
}

?>
```


If you need to perform some action when the function is called and before the result is used, you'll have to wrap your generator in another function.



```
<?php
/**
  * @return Generator
  */
function some_generator() {
    global $some_state;

    $some_state = "changed";
    return gen();
}
?>
```
  

---

Here&apos;s how to detect loop breaks, and how to handle or cleanup after an interruption.<br><br>

```
<?php
    function generator()
    {
        $complete = false;
        try {

            while (($result = some_function())) {
                yield $result;
            }
            $complete = true;

        } finally {
            if (!$complete) {
                // cleanup when loop breaks 
            } else {
                // cleanup when loop completes
            }
        }

        // Do something only after loop completes
    }
?>
```
  

---

Same example, different results:<br><br>----------------------------------<br>           |  time  | memory, mb |<br>----------------------------------<br>| not gen  | 0.7589 | 146.75     |<br>|---------------------------------<br>| with gen | 0.7469 | 8.75       |<br>|---------------------------------<br><br>Time in results varying from 6.5 to 7.8 on both examples.<br>So no real drawbacks concerning processing speed.  

---

Abstract test.<br>

```
<?php

$start_time=microtime(true);
$array = array();
$result = '';
for($count=1000000; $count--;)
{
  $array[]=$count/2;
}
foreach($array as $val)
{
  $val += 145.56;
  $result .= $val;
}
$end_time=microtime(true);

echo "time: ", bcsub($end_time, $start_time, 4), "\n";
echo "memory (byte): ", memory_get_peak_usage(true), "\n";

?>
```




```
<?php

$start_time=microtime(true);
$result = '';
function it()
{
  for($count=1000000; $count--;)
  {
    yield $count/2;
  }
}
foreach(it() as $val)
{
  $val += 145.56;
  $result .= $val;
}
$end_time=microtime(true);

echo "time: ", bcsub($end_time, $start_time, 4), "\n";
echo "memory (byte): ", memory_get_peak_usage(true), "\n";

?>
```
<br>Result:<br>----------------------------------<br>           |  time  | memory, mb |<br>----------------------------------<br>| not gen  | 2.1216 | 89.25      |<br>|---------------------------------<br>| with gen | 6.1963 | 8.75       |<br>|---------------------------------<br>| diff     | &lt; 192% | &gt; 90%      |<br>----------------------------------  

---

[Official documentation page](https://www.php.net/manual/en/language.generators.overview.php)

**[To root](/README.md)**
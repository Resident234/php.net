# The Generator class





Unlike return, yield can be used anywhere within a function so logic can flow more naturally. Take for example the following Fibonacci generator:



```
<?php
function fib($n)
{
&#xA0; &#xA0; $cur = 1;
&#xA0; &#xA0; $prev = 0;
&#xA0; &#xA0; for ($i = 0; $i &lt; $n; $i++) {
&#xA0; &#xA0; &#xA0; &#xA0; yield $cur;

&#xA0; &#xA0; &#xA0; &#xA0; $temp = $cur;
&#xA0; &#xA0; &#xA0; &#xA0; $cur = $prev + $cur;
&#xA0; &#xA0; &#xA0; &#xA0; $prev = $temp;
&#xA0; &#xA0; }
}

$fibs = fib(9);
foreach ($fibs as $fib) {
&#xA0; &#xA0; echo &quot; &quot; . $fib;
}

// prints: 1 1 2 3 5 8 13 21 34


  

#

[Official documentation page](https://www.php.net/manual/en/class.generator.php)

**[To root](/README.md)**
# The Generator class



Unlike return, yield can be used anywhere within a function so logic can flow more naturally. Take for example the following Fibonacci generator:<br><br>

```
<?php<br>function fib($n)<br>{<br>    $cur = 1;<br>    $prev = 0;<br>    for ($i = 0; $i &lt; $n; $i++) {<br>        yield $cur;<br><br>        $temp = $cur;<br>        $cur = $prev + $cur;<br>        $prev = $temp;<br>    }<br>}<br><br>$fibs = fib(9);<br>foreach ($fibs as $fib) {<br>    echo " " . $fib;<br>}<br><br>// prints: 1 1 2 3 5 8 13 21 34  

#

[Official documentation page](https://www.php.net/manual/en/class.generator.php)

**[To root](/README.md)**
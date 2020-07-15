# The Generator class



Unlike return, yield can be used anywhere within a function so logic can flow more naturally. Take for example the following Fibonacci generator:<br><br>

```
<?php
function fib($n)
{
    $cur = 1;
    $prev = 0;
    for ($i = 0; $i < $n; $i++) {
        yield $cur;

        $temp = $cur;
        $cur = $prev + $cur;
        $prev = $temp;
    }
}

$fibs = fib(9);
foreach ($fibs as $fib) {
    echo " " . $fib;
}

// prints: 1 1 2 3 5 8 13 21 34?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.generator.php)

**[To root](/README.md)**
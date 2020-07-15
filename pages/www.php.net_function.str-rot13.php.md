# str_rot13



I was reminded again of the desire for a generic str_rot function. Character manipulation loops in PHP are slow compared to their C counterparts, so here&apos;s a tuned version of the previous function I posted. It&apos;s 1.6 times as fast, mainly by avoiding chr() calls.<br><br>

```
<?php
function str_rot($s, $n = 13) {
    static $letters = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $n = (int)$n % 26;
    if (!$n) return $s;
    if ($n == 13) return str_rot13($s);
    for ($i = 0, $l = strlen($s); $i &lt; $l; $i++) {
        $c = $s[$i];
        if ($c &gt;= 'a' &amp;&amp; $c &lt;= 'z') {
            $s[$i] = $letters[(ord($c) - 71 + $n) % 26];
        } else if ($c &gt;= 'A' &amp;&amp; $c &lt;= 'Z') {
            $s[$i] = $letters[(ord($c) - 39 + $n) % 26 + 26];
        }
    }
    return $s;
}
?>
```


But using strtr() you can get something 10 times as fast as the above :



```
<?php
function str_rot($s, $n = 13) {
    static $letters = 'AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz';
    $n = (int)$n % 26;
    if (!$n) return $s;
    if ($n &lt; 0) $n += 26;
    if ($n == 13) return str_rot13($s);
    $rep = substr($letters, $n * 2) . substr($letters, 0, $n * 2);
    return strtr($s, $letters, $rep);
}
?>
```


This technique is faster because PHP's strtr is implemented in C using a byte lookup table (it has O(m + n) complexity). However, PHP 6 will use Unicode, so I guess(?) strtr will then have to be implemented with a search for each character (O(m * n)). Using strtr might still be faster since it offloads the character manipulation to C rather than PHP, but I don't really know. Take your pick.

Happy coding!

(Benchmark code):



```
<?php
for ($k = 0; $k &lt; 10; $k++) {
    $s = 'The quick brown fox jumps over the lazy dog.';
    $t = microtime(1);
    for ($i = 0; $i &lt; 1000; $i++) $s = str_rot($s, $i);
    $t = microtime(1) - $t;
    echo number_format($t, 3) . "\n";
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.str-rot13.php)

**[To root](/README.md)**
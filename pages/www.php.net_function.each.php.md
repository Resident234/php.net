# each



Regarding speed of foreach vs while(list) =each<br>I wrote a benchmark script and the results are that clearly foreach is faster. MUCH faster. Even with huge arrays (especially with huge arrays). I tested with sizes 100,000. 1,000,000 and 10,000,000. To do the test with 10 million i had to set my memory limit real high, it was close to 1gb by the time it actually worked. Anyways, <br><br>

```
<?php
function getDiff($start, $end) {
    $s = explode(&apos; &apos;, $start);
    $stot = $s[1] + $s[0];
    $e = explode(&apos; &apos;, $end);
    $etot = $e[1] + $e[0];
    return $etot - $stot;
}

$lim=10000000;
$arr = array();
for ($i=0; $i&lt;$lim; $i++) {
    $arr[$i] = $i/2;
}

$start = microtime();
foreach ($arr as $key=&gt;$val);

$end = microtime();
echo "time for foreach = " . getDiff($start, $end) . ".\n";

reset($arr);
$start = microtime();
while (list($key, $val) = each($arr));
$end = microtime();
echo "time list each = " . getDiff($start, $end) . ".\n";
?>
```


here are some of my results: with 1,000,000
time for foreach = 0.0244591236115.
time list each = 0.158002853394.
desktop:/media/sda5/mpwolfe/tests$ php test.php
time for foreach = 0.0245339870453.
time list each = 0.154260158539.
desktop:/media/sda5/mpwolfe/tests$ php test.php
time for foreach = 0.0269000530243.
time list each = 0.157305955887.

then with 10,000,000:
desktop:/media/sda5/mpwolfe/tests$ php test.php
time for foreach = 1.96586894989.
time list each = 14.1371650696.
desktop:/media/sda5/mpwolfe/tests$ php test.php
time for foreach = 2.02504014969.
time list each = 13.7696218491.
desktop:/media/sda5/mpwolfe/tests$ php test.php<br>time for foreach = 2.0246758461.<br>time list each = 13.8425710201.<br><br>by the way, these results are with php 5.2 i believe, and a linux machine with 3gb of ram and 2.8ghz dual core pentium  

#

[Official documentation page](https://www.php.net/manual/en/function.each.php)

**[To root](/README.md)**
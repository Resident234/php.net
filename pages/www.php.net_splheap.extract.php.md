# SplHeap::extract





```
<?php
$heap = new SplMaxHeap(); # Ascending order
$heap-&gt;insert(&apos;E&apos;);
$heap-&gt;insert(&apos;B&apos;);
$heap-&gt;insert(&apos;D&apos;);
$heap-&gt;insert(&apos;A&apos;);
$heap-&gt;insert(&apos;C&apos;);

echo $heap-&gt;extract(), PHP_EOL; # E
echo $heap-&gt;extract(), PHP_EOL; # D

$heap = new SplMinHeap(); # Descending order
$heap-&gt;insert(&apos;E&apos;);
$heap-&gt;insert(&apos;B&apos;);
$heap-&gt;insert(&apos;D&apos;);
$heap-&gt;insert(&apos;A&apos;);
$heap-&gt;insert(&apos;C&apos;);

print PHP_EOL;
echo $heap-&gt;extract(), PHP_EOL; # A
echo $heap-&gt;extract(), PHP_EOL; # B
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/splheap.extract.php)

**[To root](/README.md)**
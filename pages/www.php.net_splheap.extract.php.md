# SplHeap::extract





```
<?php
$heap = new SplMaxHeap(); # Ascending order
$heap->insert('E');
$heap->insert('B');
$heap->insert('D');
$heap->insert('A');
$heap->insert('C');

echo $heap->extract(), PHP_EOL; # E
echo $heap->extract(), PHP_EOL; # D

$heap = new SplMinHeap(); # Descending order
$heap->insert('E');
$heap->insert('B');
$heap->insert('D');
$heap->insert('A');
$heap->insert('C');

print PHP_EOL;
echo $heap->extract(), PHP_EOL; # A
echo $heap->extract(), PHP_EOL; # B
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/splheap.extract.php)

**[To root](/README.md)**
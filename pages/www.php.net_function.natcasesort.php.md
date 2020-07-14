# natcasesort



Something that should probably be documented is the fact that both natsort and natcasesort maintain the key-value associations of the array. If you natsort a numerically indexed array, a for loop will not produce the sorted order; a foreach loop, however, will produce the sorted order, but the indices won&apos;t be in numeric order. If you want natsort and natcasesort to break the key-value associations, just use array_values on the sorted array, like so:<br><br>natcasesort($arr);<br>$arr = array_values($arr);  

#

[Official documentation page](https://www.php.net/manual/en/function.natcasesort.php)

**[To root](/README.md)**
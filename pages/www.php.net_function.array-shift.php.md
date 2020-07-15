# array_shift



Using array_shift over larger array was fairly slow.  It sped up as the array shrank, most likely as it has to reindex a smaller data set.<br><br>For my purpose, I used array_reverse, then array_pop, which doesn&apos;t need to reindex the array and will preserve keys if you want it to (didn&apos;t matter in my case).  <br><br>Using direct index references, i.e., array_test[$i], was fast, but direct index referencing + unset for destructive operations was about the same speed as array_reverse and array_pop.  It also requires sequential numeric keys.  

#

Notice:<br>the complexity of array_pop() is O(1). <br>the complexity of array_shift() is O(n).<br>array_shift() requires a re-index process on the array, so it has to run over all the elements and index them.  

#

Just a useful version which returns a simple array with the first key and value. Porbably a better way of doing it, but it works for me ;-)<br><br>

```
<?php

function array_kshift(&amp;$arr)
{
  list($k) = array_keys($arr);
  $r  = array($k=>$arr[$k]);
  unset($arr[$k]);
  return $r;
}

// test it on a simple associative array
$arr = array('x'=>'ball','y'=>'hat','z'=>'apple');

print_r($arr);
print_r(array_kshift($arr));
print_r($arr);

?>
```
<br><br>Output:<br><br>Array<br>(<br>    [x] =&gt; ball<br>    [y] =&gt; hat<br>    [z] =&gt; apple<br>)<br>Array<br>(<br>    [x] =&gt; ball<br>)<br>Array<br>(<br>    [y] =&gt; hat<br>    [z] =&gt; apple<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.array-shift.php)

**[To root](/README.md)**
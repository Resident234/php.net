# The MultipleIterator class



This iterator has a misleading name and description - it actually acts as a parallel iterator: You attach one or more iterators with a key, integer or NULL, and when you iterate over the MultipleIterator, as the result for current() you get ALL results from all attached iterators as an array (under the key or integer you attached it with), and the same is true for the key() call.<br><br>valid() will be valid if any or all iterators are valid, depending on the setting of the $flags - with ANY, you can iterate over a set of iterators with some of them ending before others, and get NULL results from these iterators until the last iterator is at it&apos;s end. With ALL, iteration stops when the first iterator stops delivering results.<br><br>next() and rewind() will be called on all attached iterators in every case.<br><br>

```
<?php

$it1 = new ArrayIterator(array(1,2,3));
$it2 = new ArrayIterator(array(4,5,6));

$multipleIterator = new MultipleIterator(MultipleIterator::MIT_NEED_ALL|MultipleIterator::MIT_KEYS_ASSOC);

$multipleIterator-&gt;attachIterator($it1, 1);
$multipleIterator-&gt;attachIterator($it2, &apos;second&apos;);

foreach ($multipleIterator as $key =&gt; $value) {
    echo "Key\n"; var_dump($key);
    echo "Value\n"; var_dump($value);
    echo "---next---\n";
}
?>
```
<br><br>Result with PHP 5.5.0 and up:<br><br>Key<br>array(2) {<br>  [1]=&gt;<br>  int(0)<br>  ["second"]=&gt;<br>  int(0)<br>}<br>Value<br>array(2) {<br>  [1]=&gt;<br>  int(1)<br>  ["second"]=&gt;<br>  int(4)<br>}<br>---next---<br>Key<br>array(2) {<br>  [1]=&gt;<br>  int(1)<br>  ["second"]=&gt;<br>  int(1)<br>}<br>Value<br>array(2) {<br>  [1]=&gt;<br>  int(2)<br>  ["second"]=&gt;<br>  int(5)<br>}<br>---next---<br>Key<br>array(2) {<br>  [1]=&gt;<br>  int(2)<br>  ["second"]=&gt;<br>  int(2)<br>}<br>Value<br>array(2) {<br>  [1]=&gt;<br>  int(3)<br>  ["second"]=&gt;<br>  int(6)<br>}<br>---next---<br><br>Note that PHP 5.4 and 5.3 do not support accessing the key() values in foreach loops because they expect them to not be an array - doing so will cause "Warning: Illegal type returned from MultipleIterator::key()" and the result of (int)0 as the key for all iterations.<br><br>Without the MultipleIterator::MIT_KEYS_ASSOC flag, the MultipleIterator will create numeric indices based on the order of attachment.  

#

[Official documentation page](https://www.php.net/manual/en/class.multipleiterator.php)

**[To root](/README.md)**
# The RegexIterator class



An exemple :<br><br>

```
<?php
$a = new ArrayIterator(array(&apos;test1&apos;, &apos;test2&apos;, &apos;test3&apos;));
$i = new RegexIterator($a, &apos;/^(test)(\d+)/&apos;, RegexIterator::REPLACE);
$i-&gt;replacement = &apos;$2:$1&apos;;
       
print_r(iterator_to_array($i));
/*
Array
(
    [0] =&gt; 1:test
    [1] =&gt; 2:test
    [2] =&gt; 3:test
)
*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.regexiterator.php)

**[To root](/README.md)**
# The IteratorIterator class



This iterator basically is only a wrapper around another iterator. It does nothing fancy, it simply forwards any calls of rewind(), next(), valid(), current() and key() to the inner iterator. This inner iterator can be fetched with getInnerIterator().<br><br>One special case: When passing an IteratorAggregate object, the getIterator() method of that object will be called and THAT iterator will be iterated over, and this will also be returned when calling getInnerIterator().<br><br>This class can be extended, so it&apos;s an ideal building block for your own classes that only want to modify one or two of the iterator methods, but not all.<br><br>Want to trim the strings returned by the current() method?<br><br>

```
<?php

class TrimIterator extends IteratorIterator
{
    public function current() {
        return trim(parent::current());
    }
}

$innerIterator = new ArrayIterator(array(&apos;normal&apos;, &apos; trimmable &apos;));

$trim = new TrimIterator($innerIterator);

foreach ($trim as $key =&gt; $value) {
    echo "Key:\n";
    var_dump($key);
    echo "Value:\n";
    var_dump($value);
    echo "---next---";
}
?>
```
<br><br>Output:<br><br>Key:<br>int(0)<br>Value:<br>string(6) "normal"<br>---next---Key:<br>int(1)<br>Value:<br>string(9) "trimmable"<br>---next---  

#

[Official documentation page](https://www.php.net/manual/en/class.iteratoriterator.php)

**[To root](/README.md)**
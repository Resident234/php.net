# The SplObjectStorage class



Note some inconsistent/surprising behavior in SplObjectStorage to preserve backwards compatibility. You can&apos;t properly use foreach with key/value syntax.<br><br>

```
<?php
$spl = new SplObjectStorage ();
$keyForA = new StdClass();
$keyForB = new StdClass();
$spl[$keyForA] = &apos;value a&apos;;
$spl[$keyForB] = &apos;value b&apos;;
foreach ($spl as $key =&gt; $value)
{
    // $key is NOT an object, $value is!
    // Must use standard array access to get strings.
    echo $spl[$value] . "\n"; // prints "value a", then "value b"
}
// it may be clearer to use this form of foreach:
foreach ($spl as $key)
{
    // $key is an object.
    // Use standard array access to get values.
    echo $spl[$key] . "\n"; // prints "value a", then "value b"
}
?>
```
<br><br>See https://bugs.php.net/bug.php?id=49967  

#

[Official documentation page](https://www.php.net/manual/en/class.splobjectstorage.php)

**[To root](/README.md)**
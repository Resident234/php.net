# array_fill_keys





```
<?php
$a = array("1");

var_dump(array_fill_keys($a, "test"));
?>
```
<br><br>array(1) {<br>  [1]=&gt;<br>  string(4) "test"<br>}<br><br>now string key "1" become an integer value 1, be careful.  

#

If an associative array is used as the second parameter of array_fill_keys, then the associative array will be appended in all the values of the first array.<br>e.g.<br>

```
<?php
$array1 = array(
    "a" =&gt; "first",
    "b" =&gt; "second",
    "c" =&gt; "something",
    "red"
);

$array2 = array(
    "a" =&gt; "first",
    "b" =&gt; "something",
    "letsc"
);

print_r(array_fill_keys($array1, $array2));
?>
```
<br><br>The output will be<br>Array(<br>    [first] =&gt; Array(<br>        [a] =&gt; first,<br>        [b] =&gt; something,<br>        [0] =&gt; letsc<br>    ),<br>    [second] =&gt; Array(<br>        [a] =&gt; first,<br>        [b] =&gt; something,<br>        [0] =&gt; letsc<br>    ),<br>    [something] =&gt; Array(<br>        [a] =&gt; first,<br>        [b] =&gt; something,<br>        [0] =&gt; letsc<br>    ),<br>    [red] =&gt; Array(<br>        [a] =&gt; first,<br>        [b] =&gt; something,<br>        [0] =&gt; letsc<br>    )<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.array-fill-keys.php)

**[To root](/README.md)**
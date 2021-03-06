# array_pad



Beware, if you try to pad an associative array using numeric keys, your keys will be re-numbered.<br><br>

```
<?php
$a = array('size'=>'large', 'number'=>20, 'color'=>'red');
print_r($a);
print_r(array_pad($a, 5, 'foo'));

// use timestamps as keys
$b = array(1229600459=>'large', 1229604787=>20, 1229609459=>'red');
print_r($b);
print_r(array_pad($b, 5, 'foo'));
?>
```
<br><br>yields this:<br>------------------<br>Array<br>(<br>    [size] =&gt; large<br>    [number] =&gt; 20<br>    [color] =&gt; red<br>)<br>Array<br>(<br>    [size] =&gt; large<br>    [number] =&gt; 20<br>    [color] =&gt; red<br>    [0] =&gt; foo<br>    [1] =&gt; foo<br>)<br>Array<br>(<br>    [1229600459] =&gt; large<br>    [1229604787] =&gt; 20<br>    [1229609459] =&gt; red<br>)<br>Array<br>(<br>    [0] =&gt; large<br>    [1] =&gt; 20<br>    [2] =&gt; red<br>    [3] =&gt; foo<br>    [4] =&gt; foo<br>)  

---

[Official documentation page](https://www.php.net/manual/en/function.array-pad.php)

**[To root](/README.md)**
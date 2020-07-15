# array_combine



If two keys are the same, the second one prevails. <br><br>Example:<br>

```
<?php
print_r(array_combine(Array('a','a','b'), Array(1,2,3)));
?>
```

Returns:
Array
(
    [a] => 2
    [b] => 3
)

But if you need to keep all values, you can use the function below:



```
<?php
function array_combine_($keys, $values)
{
    $result = array();
    foreach ($keys as $i => $k) {
        $result[$k][] = $values[$i];
    }
    array_walk($result, create_function('&amp;$v', '$v = (count($v) == 1)? array_pop($v): $v;'));
    return    $result;
}

print_r(array_combine_(Array('a','a','b'), Array(1,2,3)));
?>
```
<br>Returns:<br>Array<br>(<br>    [a] =&gt; Array<br>        (<br>            [0] =&gt; 1<br>            [1] =&gt; 2<br>        )<br><br>    [b] =&gt; 3<br>)  

#

Further to loreiorg&apos;s script <br>in order to preserve duplicate keys when combining arrays.<br><br>I have modified the script to use a closure instead of create_function<br><br>Reason: see security issue flagged up in the documentation concerning create_function<br><br>

```
<?php

function array_combine_($keys, $values){
    $result = array();

    foreach ($keys as $i => $k) {
     $result[$k][] = $values[$i];
     }

    array_walk($result, function(&amp;$v){
     $v = (count($v) == 1) ? array_pop($v): $v;
     });

    return $result;
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-combine.php)

**[To root](/README.md)**
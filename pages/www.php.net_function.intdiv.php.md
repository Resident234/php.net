# intdiv





This does indeed seem to be equal to intdiv:



```
<?php
function intdiv_1($a, $b){
&#xA0; &#xA0; return ($a - $a % $b) / $b;
}
?>
```


However, this isn&apos;t:



```
<?php
function intdiv_2($a, $b){
&#xA0; &#xA0; return floor($a / $b);
}
?>
```


Consider an example where either of the parameters is negative:


```
<?php
$param1 = -10;
$param2 = 3;
print_r([
&#xA0; &#xA0; &apos;modulus&apos; =&gt; intdiv_1($param1, $param2),
&#xA0; &#xA0; &apos;floor&apos; =&gt; intdiv_2($param1, $param2),
]);

/**
 * Array
 * (
 *&#xA0; &#xA0;&#xA0; [modulus] =&gt; -3
 *&#xA0; &#xA0;&#xA0; [floor] =&gt; -4
 * )
 */
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.intdiv.php)

**[To root](/README.md)**
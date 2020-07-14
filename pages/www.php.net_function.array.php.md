# array



As of PHP 5.4.x you can now use &apos;short syntax arrays&apos; which eliminates the need of this function.<br><br>Example #1 &apos;short syntax array&apos;<br>

```
<?php
    $a = [1, 2, 3, 4];
    print_r($a);
?>
```


The above example will output:
Array
(
    [0] =&gt; 1
    [1] =&gt; 2
    [2] =&gt; 3
    [3] =&gt; 4
)

Example #2 &apos;short syntax associative array&apos;


```
<?php
    $a = [&apos;one&apos; =&gt; 1, &apos;two&apos; =&gt; 2, &apos;three&apos; =&gt; 3, &apos;four&apos; =&gt; 4];
    print_r($a);
?>
```
<br><br>The above example will output:<br>Array<br>(<br>    [one] =&gt; 1<br>    [two] =&gt; 2<br>    [three] =&gt; 3<br>    [four] =&gt; 4<br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.array.php)

**[To root](/README.md)**
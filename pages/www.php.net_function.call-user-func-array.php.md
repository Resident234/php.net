# call_user_func_array



As of PHP 5.6 you can utilize argument unpacking as an alternative to call_user_func_array, and is often 3 to 4 times faster.<br><br>

```
<?php
function foo ($a, $b) {
     return $a + $b;
}

$func = &apos;foo&apos;;
$values = array(1, 2);
call_user_func_array($func, $values); 
//returns 3

$func(...$values);
//returns 3
?>
```
<br><br>Benchmarks from https://gist.github.com/nikic/6390366<br>cufa   with 0 args took 0.43453288078308<br>switch with 0 args took 0.24134302139282<br>unpack with 0 args took 0.12418699264526<br>cufa   with 5 args took 0.73408579826355<br>switch with 5 args took 0.49595499038696<br>unpack with 5 args took 0.18640494346619<br>cufa   with 100 args took 5.0327250957489<br>switch with 100 args took 5.291127204895<br>unpack with 100 args took 1.2362589836121  

#

Just hope this note helps someone (I killed the whole day on issue).<br><br>If you use something like this in PHP &lt; 5.3:<br>

```
<?php call_user_func_array(array($this, &apos;parent::func&apos;), $args); ?>
```

Such a script will cause segmentation fault in your webserver.

In 5.3 you should write it:


```
<?php call_user_func_array(&apos;parent::func&apos;, $args); ?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.call-user-func-array.php)

**[To root](/README.md)**
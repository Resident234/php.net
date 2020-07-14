# apc_add



In order to understand better how APC caching works you can do the following:<br><br>1. Restart web server (Apache or Nginx)<br><br>2. Create file "apc_fetch.php":<br>

```
<?php
var_dump(apc_fetch(array(
    &apos;CUR_DATE_5s_1&apos;,
    &apos;CUR_DATE_5s_2&apos;,
    &apos;CUR_DATE_5s_3&apos;,
    &apos;CUR_DATE_0s_1&apos;,
    &apos;CUR_DATE_0s_2&apos;,
    &apos;CUR_DATE_0s_3&apos;,
)));
?>
```


3. Create file "apc_add.php":


```
<?php
$ttl = 5;

$key = &apos;CUR_DATE_5s_1&apos;;
$var = date(&apos;c&apos;);
$result = apc_add($key, $var, $ttl);
var_dump($result);
echo "\n";

$var = date(&apos;c&apos;);
$result = apc_add($key, $var, $ttl);
var_dump($result);
echo "\n";

$key = &apos;CUR_DATE_0s_1&apos;;
$var = date(&apos;c&apos;);
$result = apc_add($key, $var);
var_dump($result);
echo "\n";

$values = array(
    &apos;CUR_DATE_5s_2&apos; =&gt; date(&apos;c&apos;),
    &apos;CUR_DATE_5s_3&apos; =&gt; rand(),
);
$result = apc_add($values, null, $ttl);
var_dump($result);
echo "\n";

$values = array(
    &apos;CUR_DATE_0s_2&apos; =&gt; date(&apos;c&apos;),
    &apos;CUR_DATE_0s_3&apos; =&gt; rand(),
);
$result = apc_add($values, null);
var_dump($result);
?>
```
<br><br>4. Run "apc_fetch.php" and "apc_add.php" several times in order to see the persistent result and how values change from one request to another.  

#

[Official documentation page](https://www.php.net/manual/en/function.apc-add.php)

**[To root](/README.md)**
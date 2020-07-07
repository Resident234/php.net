# apc_add





In order to understand better how APC caching works you can do the following:

1. Restart web server (Apache or Nginx)

2. Create file &quot;apc_fetch.php&quot;:


```
<?php
var_dump(apc_fetch(array(
&#xA0; &#xA0; &apos;CUR_DATE_5s_1&apos;,
&#xA0; &#xA0; &apos;CUR_DATE_5s_2&apos;,
&#xA0; &#xA0; &apos;CUR_DATE_5s_3&apos;,
&#xA0; &#xA0; &apos;CUR_DATE_0s_1&apos;,
&#xA0; &#xA0; &apos;CUR_DATE_0s_2&apos;,
&#xA0; &#xA0; &apos;CUR_DATE_0s_3&apos;,
)));
?>
```


3. Create file &quot;apc_add.php&quot;:


```
<?php
$ttl = 5;

$key = &apos;CUR_DATE_5s_1&apos;;
$var = date(&apos;c&apos;);
$result = apc_add($key, $var, $ttl);
var_dump($result);
echo &quot;\n&quot;;

$var = date(&apos;c&apos;);
$result = apc_add($key, $var, $ttl);
var_dump($result);
echo &quot;\n&quot;;

$key = &apos;CUR_DATE_0s_1&apos;;
$var = date(&apos;c&apos;);
$result = apc_add($key, $var);
var_dump($result);
echo &quot;\n&quot;;

$values = array(
&#xA0; &#xA0; &apos;CUR_DATE_5s_2&apos; =&gt; date(&apos;c&apos;),
&#xA0; &#xA0; &apos;CUR_DATE_5s_3&apos; =&gt; rand(),
);
$result = apc_add($values, null, $ttl);
var_dump($result);
echo &quot;\n&quot;;

$values = array(
&#xA0; &#xA0; &apos;CUR_DATE_0s_2&apos; =&gt; date(&apos;c&apos;),
&#xA0; &#xA0; &apos;CUR_DATE_0s_3&apos; =&gt; rand(),
);
$result = apc_add($values, null);
var_dump($result);
?>
```


4. Run &quot;apc_fetch.php&quot; and &quot;apc_add.php&quot; several times in order to see the persistent result and how values change from one request to another.

  

#

[Official documentation page](https://www.php.net/manual/en/function.apc-add.php)

**[To root](/README.md)**
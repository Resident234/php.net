# get_magic_quotes_gpc



@ dot dot dot dot dot alexander at gmail dot com<br><br>I suggest replacing foreach by "stripslashes_deep":<br><br>Example #2 Using stripslashes() on an array on <br>&lt;http://www.php.net/manual/en/function.stripslashes.php&gt;:<br><br>

```
<?php
function stripslashes_deep($value)
{
    $value = is_array($value) ?
                array_map(&apos;stripslashes_deep&apos;, $value) :
                stripslashes($value);

    return $value;
}
?>
```


This gives:



```
<?php
if((function_exists("get_magic_quotes_gpc") &amp;&amp; get_magic_quotes_gpc())    || (ini_get(&apos;magic_quotes_sybase&apos;) &amp;&amp; (strtolower(ini_get(&apos;magic_quotes_sybase&apos;))!="off")) ){
    stripslashes_deep($_GET);
    stripslashes_deep($_POST);
    stripslashes_deep($_COOKIE);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-magic-quotes-gpc.php)

**[To root](/README.md)**
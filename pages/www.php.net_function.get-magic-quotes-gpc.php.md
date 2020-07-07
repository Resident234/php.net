# get_magic_quotes_gpc





@ dot dot dot dot dot alexander at gmail dot com



I suggest replacing foreach by &quot;stripslashes_deep&quot;:



Example #2 Using stripslashes() on an array on 

&lt;http://www.php.net/manual/en/function.stripslashes.php&gt;:





```
<?php

function stripslashes_deep($value)

{

&#xA0; &#xA0; $value = is_array($value) ?

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_map(&apos;stripslashes_deep&apos;, $value) :

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; stripslashes($value);



&#xA0; &#xA0; return $value;

}

php?>
```




This gives:





```
<?php

if((function_exists(&quot;get_magic_quotes_gpc&quot;) &amp;&amp; get_magic_quotes_gpc())&#xA0; &#xA0; || (ini_get(&apos;magic_quotes_sybase&apos;) &amp;&amp; (strtolower(ini_get(&apos;magic_quotes_sybase&apos;))!=&quot;off&quot;)) ){

&#xA0; &#xA0; stripslashes_deep($_GET);

&#xA0; &#xA0; stripslashes_deep($_POST);

&#xA0; &#xA0; stripslashes_deep($_COOKIE);

}

php?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.get-magic-quotes-gpc.php)

**[To root](/README.md)**
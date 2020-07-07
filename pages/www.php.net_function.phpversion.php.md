# phpversion





If you&apos;re trying to check whether the version of PHP you&apos;re running on is sufficient, don&apos;t screw around with `strcasecmp` etc.&#xA0; PHP already has a `version_compare` function, and it&apos;s specifically made to compare PHP-style version strings.



```
<?php
if (version_compare(phpversion(), &apos;5.3.10&apos;, &apos;&lt;&apos;)) {
&#xA0; &#xA0; // php version isn&apos;t high enough
}
?>
```



  

#



Note that the version string returned by phpversion() may include more information than expected: &quot;5.5.9-1ubuntu4.17&quot;, for example.

  

#



To know, what are the {php} extensions loaded &amp; version of extensions :





```
<?php

foreach (get_loaded_extensions() as $i =&gt; $ext)

{

&#xA0;&#xA0; echo $ext .&apos; =&gt; &apos;. phpversion($ext). &apos;&lt;br/&gt;&apos;;

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.phpversion.php)

**[To root](/README.md)**